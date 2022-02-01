# Common scenarios with the CloudWatch agent<a name="CloudWatch-Agent-common-scenarios"></a>

The following sections outline how to complete some common configuration and customization tasks when using the CloudWatch agent\. 

**Topics**
+ [Running the CloudWatch agent as a different user](#CloudWatch-Agent-run-as-user)
+ [Adding custom dimensions to metrics collected by the CloudWatch agent](#CloudWatch-Agent-adding-custom-dimensions)
+ [Multiple CloudWatch agent configuration files](#CloudWatch-Agent-multiple-config-files)
+ [Aggregating or rolling up metrics collected by the CloudWatch agent](#CloudWatch-Agent-aggregating-metrics)
+ [Collecting high\-resolution metrics with the CloudWatch agent](#CloudWatch-Agent-collect-high-resolution-metrics)
+ [Sending metrics and logs to a different account](#CloudWatch-Agent-send-to-different-AWS-account)
+ [Timestamp differences between the unified CloudWatch agent and the earlier CloudWatch Logs agent](#CloudWatch-Agent-logs-timestamp-differences)

## Running the CloudWatch agent as a different user<a name="CloudWatch-Agent-run-as-user"></a>

On Linux servers, the CloudWatch runs as the root user by default\. To have the agent run as a different user, use the `run_as_user` parameter in the `agent` section in the CloudWatch agent configuration file\. This option is available only on Linux servers\.

If you're already running the agent with the root user and want to change to using a different user, use one of the following procedures\.

**To run the CloudWatch agent as a different user on an EC2 instance running Linux**

1. Download and install a new CloudWatch agent package\. For more information, see [Download the CloudWatch agent package](download-cloudwatch-agent-commandline.md#download-CloudWatch-Agent-on-EC2-Instance-commandline-first)\.

1. Create a new Linux user or use the default user named `cwagent` that the RPM or DEB file created\.

1. Provide credentials for this user in one of these ways:
   + If the file `.aws/credentials` exists in the home directory of the root user, you must create a credentials file for the user you are going to use to run the CloudWatch agent\. This credentials file will be `/home/username/.aws/credentials`\. Then set the value of the `shared_credential_file` parameter in `common-config.toml` to the pathname of the credential file\. For more information, see [\(Optional\) Modify the common configuration for proxy or Region information](install-CloudWatch-Agent-commandline-fleet.md#CloudWatch-Agent-profile-instance-first)\.
   + If the file `.aws/credentials` does not exist in the home directory of the root user, you can do one of the following:
     + Create a credentials file for the user you are going to use to run the CloudWatch agent\. This credentials file will be `/home/username/.aws/credentials`\. Then set the value of the `shared_credential_file` parameter in `common-config.toml` to the pathname of the credential file\. For more information, see [\(Optional\) Modify the common configuration for proxy or Region information](install-CloudWatch-Agent-commandline-fleet.md#CloudWatch-Agent-profile-instance-first)\.
     + Instead of creating a credentials file, attach an IAM role to the instance\. The agent uses this role as the credential provider\.

1. In the CloudWatch agent configuration file, add the following line in the `agent` section:

   ```
   "run_as_user": "username"
   ```

   Make other modifications to the configuration file as needed\. For more information, see [Create the CloudWatch agent configuration file](create-cloudwatch-agent-configuration-file.md)

1. Give the user necessary permissions\. The user must have Read \(r\) permissions for the log files to be collected, and must have Execute \(x\) permission on every directory in the log files' path\.

1. Start the agent with the configuration file that you just modified\.

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:configuration-file-path
   ```

**To run the CloudWatch agent as a different user on an on\-premises server running Linux**

1. Download and install a new CloudWatch agent package\. For more information, see [Download the CloudWatch agent package](download-cloudwatch-agent-commandline.md#download-CloudWatch-Agent-on-EC2-Instance-commandline-first)\.

1. Create a new Linux user or use the default user named `cwagent` that the RPM or DEB file created\.

1. Store the credentials of this user to a path that the user can access, such as `/home/username/.aws/credentials`\.

1. Set the value of the `shared_credential_file` parameter in `common-config.toml` to the pathname of the credential file\. For more information, see [\(Optional\) Modify the common configuration for proxy or Region information](install-CloudWatch-Agent-commandline-fleet.md#CloudWatch-Agent-profile-instance-first)\.

1. In the CloudWatch agent configuration file, add the following line in the `agent` section:

   ```
   "run_as_user": "username"
   ```

   Make other modifications to the configuration file as needed\. For more information, see [Create the CloudWatch agent configuration file](create-cloudwatch-agent-configuration-file.md)

1. Give the user necessary permissions\. The user must have Read \(r\) permissions for the log files to be collected, and must have Execute \(x\) permission on every directory in the log files' path\.

1. Start the agent with the configuration file that you just modified\.

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:configuration-file-path
   ```

## Adding custom dimensions to metrics collected by the CloudWatch agent<a name="CloudWatch-Agent-adding-custom-dimensions"></a>

To add custom dimensions such as tags to metrics collected by the agent, add the `append_dimensions` field to the section of the agent configuration file that lists those metrics\.

For example, the following example section of the configuration file adds a custom dimension named `stackName` with a value of `Prod` to the `cpu` and `disk` metrics collected by the agent\.

```
"cpu":{  
  "resources":[  
    "*"
  ],
  "measurement":[  
    "cpu_usage_guest",
    "cpu_usage_nice",
    "cpu_usage_idle"
  ],
  "totalcpu":false,
  "append_dimensions":{  
    "stackName":"Prod"
  }
},
"disk":{  
  "resources":[  
    "/",
    "/tmp"
  ],
  "measurement":[  
    "total",
    "used"
  ],
  "append_dimensions":{  
    "stackName":"Prod"
  }
}
```

Remember that any time you change the agent configuration file, you must restart the agent to have the changes take effect\.

## Multiple CloudWatch agent configuration files<a name="CloudWatch-Agent-multiple-config-files"></a>

You can set up the CloudWatch agent to use multiple configuration files\. For example, you can use a common configuration file that collects a set of metrics and logs that you always want to collect from all servers in your infrastructure\. You can then use additional configuration files that collect metrics from certain applications or in certain situations\.

To set this up, first create the configuration files that you want to use\. Any configuration files that will be used together on the same server must have different file names\. You can store the configuration files on servers or in Parameter Store\.

Start the CloudWatch agent using the `fetch-config` option and specify the first configuration file\. To append the second configuration file to the running agent, use the same command but with the `append-config` option\. All metrics and logs listed in either configuration file are collected\. The following example Linux commands illustrate this scenario using configurations stores as files\. The first line starts the agent using the `infrastructure.json` configuration file, and the second line appends the `app.json` configuration file\.

```
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/tmp/infrastructure.json
```

```
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a append-config -m ec2 -s -c file:/tmp/app.json
```

The following example configuration files illustrate a use for this feature\. The first configuration file is used for all servers in the infrastructure, and the second collects only logs from a certain application and is appended to servers running that application\.

**infrastructure\.json**

```
{
  "metrics": {
    "metrics_collected": {
      "cpu": {
        "resources": [
          "*"
        ],
        "measurement": [
          "usage_active"
        ],
        "totalcpu": true
      },
      "mem": {
         "measurement": [
           "used_percent"
        ]
      }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
            "log_group_name": "amazon-cloudwatch-agent.log"
          },
          {
            "file_path": "/var/log/messages",
            "log_group_name": "/var/log/messages"
          }
        ]
      }
    }
  }
}
```

**app\.json**

```
{
    "logs": {
        "logs_collected": {
            "files": {
                "collect_list": [
                    {
                        "file_path": "/app/app.log*",
                        "log_group_name": "/app/app.log"
                    }
                ]
            }
        }
    }
}
```

Any configuration files appended to the configuration must have different file names from each other and from the initial configuration file\. If you use `append-config` with a configuration file with the same file name as a configuration file that the agent is already using, the append command overwrites the information from the first configuration file instead of appending to it\. This is true even if the two configuration files with the same file name are on different file paths\.

The preceding example shows the use of two configuration files, but there is no limit to the number of configuration files that you can append to the agent configuration\. You can also mix the use of configuration files located on servers and configurations located in Parameter Store\.

## Aggregating or rolling up metrics collected by the CloudWatch agent<a name="CloudWatch-Agent-aggregating-metrics"></a>

To aggregate or roll up metrics collected by the agent, add an `aggregation_dimensions` field to the section for that metric in the agent configuration file\.

For example, the following configuration file snippet rolls up metrics on the `AutoScalingGroupName` dimension\. The metrics from all instances in each Auto Scaling group are aggregated and can be viewed as a whole\.

```
"metrics": {
  "cpu":{...}
  "disk":{...}
  "aggregation_dimensions" : [["AutoScalingGroupName"]]
}
```

To roll up along the combination of each `InstanceId` and `InstanceType` dimensions in addition to rolling up on the Auto Scaling group name, add the following\.

```
"metrics": {
  "cpu":{...}
  "disk":{...}
  "aggregation_dimensions" : [["AutoScalingGroupName"], ["InstanceId", "InstanceType"]]
}
```

To roll up metrics into one collection instead, use `[]`\.

```
"metrics": {
  "cpu":{...}
  "disk":{...}
  "aggregation_dimensions" : [[]]
}
```

Remember that any time you change the agent configuration file, you must restart the agent to have the changes take effect\.

## Collecting high\-resolution metrics with the CloudWatch agent<a name="CloudWatch-Agent-collect-high-resolution-metrics"></a>

The `metrics_collection_interval` field specifies the time interval for the metrics collected, in seconds\. By specifying a value of less than 60 for this field, the metrics are collected as high\-resolution metrics\.

For example, if your metrics should all be high\-resolution and collected every 10 seconds, specify 10 as the value for `metrics_collection_interval` under the `agent` section as a global metrics collection interval\.

```
"agent": {
  "metrics_collection_interval": 10
}
```

Alternatively, the following example sets the `cpu` metrics to be collected every second, and all other metrics are collected every minute\.

```
"agent":{  
  "metrics_collection_interval": 60
},
"metrics":{  
  "metrics_collected":{  
    "cpu":{  
      "resources":[  
        "*"
      ],
      "measurement":[  
        "cpu_usage_guest"
      ],
      "totalcpu":false,
      "metrics_collection_interval": 1
    },
    "disk":{  
      "resources":[  
        "/",
        "/tmp"
      ],
      "measurement":[  
        "total",
        "used"
      ]
    }
  }
}
```

Remember that any time you change the agent configuration file, you must restart the agent to have the changes take effect\.

## Sending metrics and logs to a different account<a name="CloudWatch-Agent-send-to-different-AWS-account"></a>

To have the CloudWatch agent send the metrics, logs, or both to a different account, specify a `role_arn` parameter in the agent configuration file on the sending server\. The `role_arn` value specifies an IAM role in the target account that the agent uses when sending data to the target account\. This role enables the sending account to assume a corresponding role in the target account when delivering the metrics or logs to the target account\.

You can also specify two separate `role_arn` strings in the agent configuration file: one to use when sending metrics and another for sending logs\.

The following example of part of the `agent` section of the configuration file sets the agent to use `CrossAccountAgentRole` when sending metrics and logs to a different account\.

```
{
  "agent": {
    "credentials": {
      "role_arn": "arn:aws:iam::123456789012:role/CrossAccountAgentRole"
    }
  },
  .....
}
```

Alternatively, the following example sets different roles for the sending account to use for sending metrics and logs:

```
"metrics": {
    "credentials": {
     "role_arn": "RoleToSendMetrics"
    },
    "metrics_collected": {....
```

```
"logs": {
    "credentials": {
    "role_arn": "RoleToSendLogs"
    },
    ....
```

**Policies needed**

When you specify a `role_arn` in the agent configuration file, you must also make sure the IAM roles of the sending and target accounts have certain policies\. The roles in both the sending and target accounts should have `CloudWatchAgentServerPolicy`\. For more information about assigning this policy to a role, see [Create IAM roles to use with the CloudWatch agent on Amazon EC2 instances](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-roles)\.

The role in the sending account also must include the following policy\. You add this policy on the **Permissions** tab in the IAM console when you edit the role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": [
                "arn:aws:iam::target-account-ID:role/agent-role-in-target-account"
            ]
        }
    ]
}
```

The role in the target account must include the following policy so that it recognizes the IAM role used by the sending account\. You add this policy on the **Trust relationships** tab in the IAM console when you edit the role\. The role in the target account where you add this policy is the role you created in [Create IAM roles and users for use with CloudWatch agent](create-iam-roles-for-cloudwatch-agent-commandline.md)\. This role is the role specified in `agent-role-in-target-account` in the policy used by the sending account\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::sending-account-ID:role/role-in-sender-account"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Timestamp differences between the unified CloudWatch agent and the earlier CloudWatch Logs agent<a name="CloudWatch-Agent-logs-timestamp-differences"></a>

The CloudWatch agent supports a different set of symbols for timestamp formats, compared to the earlier CloudWatch Logs agent\. These differences are shown in the following table\.


| Symbols supported by both agents | Symbols supported only by unified CloudWatch agent | Symbols supported only by earlier CloudWatch Logs agent | 
| --- | --- | --- | 
|  %A, %a, %b, %B, %d, %f, %H, %l, %m, %M, %p, %S, %y, %Y, %Z, %z  |  %\-d, %\-l, %\-m, %\-M, %\-S  |  %c,%j, %U, %W, %w  | 

For more information about the meanings of the symbols supported by the new CloudWatch agent, see [ CloudWatch Agent Configuration File: Logs Section](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html#CloudWatch-Agent-Configuration-File-Logssection) in the *Amazon CloudWatch User Guide*\. For information about symbols supported by the CloudWatch Logs agent, see [Agent Configuration File](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html#agent-configuration-file) in the *Amazon CloudWatch Logs User Guide*\.