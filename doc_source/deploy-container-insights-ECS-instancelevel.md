# Deploying the CloudWatch Agent to Collect EC2 Instance\-Level Metrics on Amazon ECS<a name="deploy-container-insights-ECS-instancelevel"></a>

To deploy the CloudWatch agent to collect instance\-level metrics from Amazon ECS clusters that are hosted on EC2 instance, use a quick start setup with a default configuration, or install the agent manually to be able to customize it\.

Both methods require that you already have at least one Amazon ECS cluster deployed with an EC2 launch type\. These methods also assume that you have the AWS CLI installed\. Additionally, to run the commands in the following procedures, you must be logged on to an account or role that has the **IAMFullAccess** and **AmazonECS\_FullAccess** policies\.

**Topics**
+ [Quick Setup Using AWS CloudFormation](#deploy-container-insights-ECS-instancelevel-quickstart)
+ [Manual and Custom Setup](#deploy-container-insights-ECS-instancelevel-manual)

## Quick Setup Using AWS CloudFormation<a name="deploy-container-insights-ECS-instancelevel-quickstart"></a>

To use the quick setup, enter the following command to use AWS CloudFormation to install the agent\. Replace *cluster\-name* and *cluster\-region* with the name and Region of your Amazon ECS cluster\.

This command creates the IAM roles **CWAgentECSTaskRole** and **CWAgentECSExecutionRole**\. If these roles already exist in your account, use `ParameterKey=CreateIAMRoles,ParameterValue=False` instead of `ParameterKey=CreateIAMRoles,ParameterValue=True` when you enter the command\. Otherwise, the command will fail\.

```
ClusterName=cluster-name
Region=cluster-region
aws cloudformation create-stack --stack-name CWAgentECS-${ClusterName}-${Region} \
    --template-body https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/daemon-service/cwagent-ecs-instance-metric/cloudformation-quickstart/cwagent-ecs-instance-metric-cfn.json \
    --parameters ParameterKey=ClusterName,ParameterValue=${ClusterName} \
                 ParameterKey=CreateIAMRoles,ParameterValue=True \
    --capabilities CAPABILITY_NAMED_IAM \
    --region ${Region}
```

**\(Alternative\) Using Your Own IAM Roles**

If you want to use your own custom ECS task role and ECS task execution role instead of the **CWAgentECSTaskRole** and **CWAgentECSExecutionRole** roles, first make sure that the role to be used as the ECS task role has **CloudWatchAgentServerPolicy** attached\. Also, make sure that the role to be used as the ECS task execution role has both the **CloudWatchAgentServerPolicy** and **AmazonECSTaskExecutionRolePolicy** policies attached\. Then enter the following command\. In the command, replace *task\-role\-arn* with the ARN of your custom ECS task role, and replace *execution\-role\-arn* with the ARN of your custom ECS task execution role\.

```
ClusterName=cluster-name
Region=cluster-region
TaskRoleArn=task-role-arn
ExecutionRoleArn=execution-role-arn
aws cloudformation create-stack --stack-name CWAgentECS-${ClusterName}-${Region} \
    --template-body https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/daemon-service/cwagent-ecs-instance-metric/cloudformation-quickstart/cwagent-ecs-instance-metric-cfn.json \
    --parameters ParameterKey=ClusterName,ParameterValue=${ClusterName} \
                 ParameterKey=TaskRoleArn,ParameterValue=${TaskRoleArn} \
                 ParameterKey=ExecutionRoleArn,ParameterValue=${ExecutionRoleArn} \
    --capabilities CAPABILITY_NAMED_IAM \
    --region ${Region}
```

**Troubleshooting the Quick Setup**

To check the status of the AWS CloudFormation stack, enter the following command\.

```
ClusterName=cluster-name
Region=cluster-region
aws cloudformation describe-stacks --stack-name CWAgentECS-$ClusterName-$Region --region $Region
```

If you see the StackStatus is other than CREATE\_COMPLETE or CREATE\_IN\_PROGRESS, check the stack events to find the error\. Enter the following command\.

```
ClusterName=cluster-name
Region=cluster-region
aws cloudformation describe-stack-events --stack-name CWAgentECS-$ClusterName-$Region --region $Region
```

To check the status of the `cwagent` daemon service, enter the following command\. In the output, you should see that the runningCount is equal to the desiredCount in the deployment section\. If it isn't equal, check the failures section in the output\.

```
ClusterName=cluster-name
Region=cluster-region
aws ecs describe-services --services cwagent-daemon-service --cluster $ClusterName --region $Region
```

You can also use the CloudWatch Logs console to check the agent log\. Look for the **/ecs/ecs\-cwagent\-daemon\-service** log group\.

**Deleting the AWS CloudFormation Stack for the CloudWatch Agent**

If you need to delete the AWS CloudFormation stack, enter the following command\.

```
ClusterName=cluster-name
Region=cluster-region
aws cloudformation delete-stack --stack-name CWAgentECS-${ClusterName}-${Region} --region ${Region}
```

## Manual and Custom Setup<a name="deploy-container-insights-ECS-instancelevel-manual"></a>

Follow the steps in this section to manually deploy the CloudWatch agent to collect instance\-level metrics from your Amazon ECS clusters that are hosted on EC2 instances\.

### Necessary IAM Roles and Policies<a name="deploy-container-insights-ECS-instancelevel-IAMRoles"></a>

Two IAM roles are required\. You must create them if they don't already exist\. For more information about these roles, see [Amazon ECS Task Role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_IAM_role.html) and [Amazon ECS Task Execution Role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)\.
+ An *ECS task role*, which is used by the CloudWatch agent to publish metrics\. If this role already exists, you must make sure it has the `CloudWatchAgentServerPolicy` policy attached\.
+ An *ECS task execution role*, which is used by Amazon ECS agent to launch the CloudWatch agent\. If this role already exists, you must make sure it has the `AmazonECSTaskExecutionRolePolicy` and `CloudWatchAgentServerPolicy` policies attached\.

If you do not already have these roles, you can use the following commands to create them and attach the necessary policies\. This first command creates the ECS task role\.

```
aws iam create-role --role-name CWAgentECSTaskRole \
    --assume-role-policy-document "{\"Version\": \"2012-10-17\",\"Statement\": [{\"Sid\": \"\",\"Effect\": \"Allow\",\"Principal\": {\"Service\": \"ecs-tasks.amazonaws.com\"},\"Action\": \"sts:AssumeRole\"}]}"
```

After you enter the previous command, note the value of Arn from the command output as "TaskRoleArn"\. You'll need to use it later when you create the task definition\. Then enter the following command to attach the necessary policies\.

```
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy \
    --role-name CWAgentECSTaskRole
```

This next command creates the ECS task execution role\.

```
aws iam create-role --role-name CWAgentECSExecutionRole \
    --assume-role-policy-document "{\"Version\": \"2012-10-17\",\"Statement\": [{\"Sid\": \"\",\"Effect\": \"Allow\",\"Principal\": {\"Service\": \"ecs-tasks.amazonaws.com\"},\"Action\": \"sts:AssumeRole\"}]}"
```

After you enter the previous command, note the value of Arn from the command output as "ExecutionRoleArn"\. You'll need to use it later when you create the task definition\. Then enter the following commands to attach the necessary policies\.

```
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy \
    --role-name CWAgentECSExecutionRole
          
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy \
    --role-name CWAgentECSExecutionRole
```

### Create the Task Definition and Launch the Daemon Service<a name="deploy-container-insights-ECS-instancelevel-taskdefinition"></a>

Create a task definition and use it to launch the CloudWatch agent as a daemon service\. To create the task definition, enter the following command\. In the first lines, replace the placeholders with the actual values for your deployment\. *logs\-region* is the Region where CloudWatch Logs is located, and *cluster\-region* is the Region where your cluster is located\. *task\-role\-arn* is the Arn of the ECS task role that you are using, and *execution\-role\-arn* is the Arn of the ECS task execution role\.

```
TaskRoleArn=task-role-arn
ExecutionRoleArn=execution-role-arn
AWSLogsRegion=logs-region
Region=cluster-region
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/daemon-service/cwagent-ecs-instance-metric/cwagent-ecs-instance-metric.json \
    | sed "s|{{task-role-arn}}|${TaskRoleArn}|;s|{{execution-role-arn}}|${ExecutionRoleArn}|;s|{{awslogs-region}}|${AWSLogsRegion}|" \
    | xargs -0 aws ecs register-task-definition --region ${Region} --cli-input-json
```

Then run the following command to launch the daemon service\. Replace *cluster\-name* and *cluster\-region* with the name and Region of your Amazon ECS cluster\.

```
ClusterName=cluster-name
Region=cluster-region
aws ecs create-service \
    --cluster ${ClusterName} \
    --service-name cwagent-daemon-service \
    --task-definition ecs-cwagent-daemon-service \
    --scheduling-strategy DAEMON \
    --region ${Region}
```

If you see this error message, An error occurred \(InvalidParameterException\) when calling the CreateService operation: Creation of service was not idempotent, you have already created a daemon service named `cwagent-daemon-service`\. You must delete that service first, using the following command as an example\.

```
ClusterName=cluster-name
Region=cluster-region
aws ecs delete-service \
    --cluster ${ClusterName} \
    --service cwagent-daemon-service \
    --region ${Region} \
    --force
```

### \(Optional\) Advanced Configuration<a name="deploy-container-insights-ECS-instancelevel-advanced"></a>

Optionally, you can use SSM to specify other configuration options for the CloudWatch agent in your Amazon ECS clusters that are hosted on EC2 instances\. These options are as follows:
+ `metrics_collection_interval` – How often in seconds that the CloudWatch agent collects metrics\. The default is 60\. The range is 1–172,000\.
+ `endpoint_override` – \(Optional\) Specifies a different endpoint to send logs to\. You might want to do this if you're publishing from a cluster in a VPC and you want the logs data to go to a VPC endpoint\.

  The value of `endpoint_override` must be a string that is a URL\.
+ `force_flush_interval` – Specifies in seconds the maximum amount of time that logs remain in the memory buffer before being sent to the server\. No matter the setting for this field, if the size of the logs in the buffer reaches 1 MB, the logs are immediately sent to the server\. The default value is 5 seconds\.
+ `region` – By default, the agent publishes metrics to the same Region where the Amazon ECS container instance is located\. To override this, you can specify a different Region here\. For example, `"region" : "us-east-1"`

The following is an example of a customized configuration:

```
{
    "agent": {
        "region": "us-east-1"
    },
    "logs": {
        "metrics_collected": {
            "ecs": {
                "metrics_collection_interval": 30
            }
        },
        "force_flush_interval": 5
    }
}
```

**To customize your CloudWatch agent configuration in your Amazon ECS containers**

1. Make sure that the **AmazonSSMReadOnlyAccess** policy is attached to your Amazon ECS Task Execution role\. You can enter the following command to do so\. This example assumes that your Amazon ECS Task Execution role is CWAgentECSExecutionRole\. If you are using a different role, substitute that role name in the following command\.

   ```
   aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMReadOnlyAccess \
           --role-name CWAgentECSExecutionRole
   ```

1. Create the customized configuration file similar to the preceding example\. Name this file `/tmp/ecs-cwagent-daemon-config.json`\.

1. Run the following command to put this configuration into the Parameter Store\. Replace *cluster\-region* with the Region of your Amazon ECS cluster\. To run this command, you must be logged on to a user or role that has the **AmazonSSMFullAccess** policy\.

   ```
   Region=cluster-region
   aws ssm put-parameter \
       --name "ecs-cwagent-daemon-service" \
       --type "String" \
       --value "`cat /tmp/ecs-cwagent-daemon-config.json`" \
       --region $Region
   ```

1. Download the task definition file to a local file, such as `/tmp/cwagent-ecs-instance-metric.json`

   ```
   curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/ecs-task-definition-templates/deployment-mode/daemon-service/cwagent-ecs-instance-metric/cwagent-ecs-instance-metric.json -o /tmp/cwagent-ecs-instance-metric.json
   ```

1. Modify the task definition file\. Remove the following section:

   ```
   "environment": [
                   {
                       "name": "USE_DEFAULT_CONFIG",
                       "value": "True"
                   }
               ],
   ```

   Replace that section with the following:

   ```
   "secrets": [
                   {
                       "name": "CW_CONFIG_CONTENT",
                       "valueFrom": "ecs-cwagent-daemon-service"
                   }
               ],
   ```

1. Restart the agent as a daemon service by following these steps:

   1. Run the following command\.

      ```
      TaskRoleArn=task-role-arn
      ExecutionRoleArn=execution-role-arn
      AWSLogsRegion=logs-region
      Region=cluster-region
      cat /tmp/cwagent-ecs-instance-metric.json \
          | sed "s|{{task-role-arn}}|${TaskRoleArn}|;s|{{execution-role-arn}}|${ExecutionRoleArn}|;s|{{awslogs-region}}|${AWSLogsRegion}|" \
          | xargs -0 aws ecs register-task-definition --region ${Region} --cli-input-json
      ```

   1. Run the following command to launch the daemon service\. Replace *cluster\-name* and *cluster\-region* with the name and Region of your Amazon ECS cluster\.

      ```
      ClusterName=cluster-name
      Region=cluster-region
      aws ecs create-service \
          --cluster ${ClusterName} \
          --service-name cwagent-daemon-service \
          --task-definition ecs-cwagent-daemon-service \
          --scheduling-strategy DAEMON \
          --region ${Region}
      ```

      If you see this error message, An error occurred \(InvalidParameterException\) when calling the CreateService operation: Creation of service was not idempotent, you have already created a daemon service named `cwagent-daemon-service`\. You must delete that service first, using the following command as an example\.

      ```
      ClusterName=cluster-name
      Region=Region
      aws ecs delete-service \
          --cluster ${ClusterName} \
          --service cwagent-daemon-service \
          --region ${Region} \
          --force
      ```