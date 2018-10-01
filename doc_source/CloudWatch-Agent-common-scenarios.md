# Common Scenarios with CloudWatch Agent<a name="CloudWatch-Agent-common-scenarios"></a>

The following sections outline how to complete some common configuration and customization tasks when using the CloudWatch agent\. 

**Topics**
+ [Adding Custom Dimensions to Metrics Collected by the CloudWatch Agent](#CloudWatch-Agent-adding-custom-dimensions)
+ [Aggregating or Rolling Up Metrics Collected by the CloudWatch Agent](#CloudWatch-Agent-aggregating-metrics)
+ [Collecting High\-Resolution Metrics With the CloudWatch agent](#CloudWatch-Agent-collect-high-resolution-metrics)
+ [Sending Metrics and Logs to a Different AWS Account](#CloudWatch-Agent-send-to-different-AWS-account)

## Adding Custom Dimensions to Metrics Collected by the CloudWatch Agent<a name="CloudWatch-Agent-adding-custom-dimensions"></a>

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

Remember that any time you change the agent configuration file, you must then restart the agent to have the changes take effect\.

## Aggregating or Rolling Up Metrics Collected by the CloudWatch Agent<a name="CloudWatch-Agent-aggregating-metrics"></a>

To aggregate or "roll up" metrics collected by the agent, add an `aggregation_dimensions` field to the section for that metric in the agent configuration file\.

For example, the following configuration file snippet rolls up metrics on the `AutoScalingGroupName` dimension\. The metrics from all instances in each Auto Scaling group are aggregated and can be viewed as a whole\.

```
"metrics": {
  "cpu":{...}
  "disk":{...}
  "aggregation_dimensions" : [["AutoScalingGroupName"]]
}
```

To roll up along the combination of each `InstanceId` and `InstanceType` dimensions in addition to rolling up on the Auto Scaling group name, add the following:

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

Remember that any time you change the agent configuration file, you must then restart the agent to have the changes take effect\.

## Collecting High\-Resolution Metrics With the CloudWatch agent<a name="CloudWatch-Agent-collect-high-resolution-metrics"></a>

The `metrics_collection_interval` field specifies the time interval for the metrics collected, in seconds\. By specifying a value of less than 60 for this field, the metrics are collected as high\-resolution metrics\.

For example, if your metrics should all be high\-resolution and collected every 10 seconds, specify 10 as the value for `metrics_collection_interval` under the `agent` section as a global metrics collection interval:

```
"agent": {
  "metrics_collection_interval": 10
}
```

Alternatively, the following example sets the `cpu` metrics to be collected every second, while all other metrics are collected every minute\.

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

Remember that any time you change the agent configuration file, you must then restart the agent to have the changes take effect\.

## Sending Metrics and Logs to a Different AWS Account<a name="CloudWatch-Agent-send-to-different-AWS-account"></a>

To have the CloudWatch agent send the metrics, logs, or both to a different AWS account, specify a `role_arn` parameter in the agent configuration file on the sending server\. The `role_arn` value specifies an IAM role in the sending account that the agent uses when sending data to the target account\. This role enables the sending account to assume a corresponding role in the target account when delivering the metrics or logs to the target account\.

You can also specify two separate `role_arn` strings in the agent configuration file: one to use when sending metrics, and another for sending logs\.

The following example of part of the `agent` section of the configuration file sets the agent to use `CrossAccountAgentRole` when sending metrics and logs to a different AWS account\.

```
{
  "agent": {
    "credentials": {
      "role_arn": "CrossAccountAgentRole"
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

**Policies Needed**

When you specify a `role_arn` in the agent configuration file, you must also make sure the IAM roles of the sending and target accounts have certain policies\. The roles in both the sending and target accounts should have **CloudWatchAgentServerPolicy**\. For more information about assigning this policy to a role, see [Create IAM Roles to Use with CloudWatch Agent on Amazon EC2 Instances](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-roles)\.

The role in the sending account also must include the following policy\. You add this policy in the **Permissions** tab in the IAM console when you edit the role\.

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

The role in the target account must include the following policy, so that it recognizes the IAM role used by the sending account\. You add this policy in the **Trust relationships** tab in the IAM console when you edit the role\. The role in the target account where you add this policy is the role you created in [Create IAM Roles to Use with CloudWatch Agent on Amazon EC2 Instances](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-roles)\. This role is the role specified in `agent-role-in-target-account` in the policy used by the sending account\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::sending-account-ID:role/role-specified-in-role_arn"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```