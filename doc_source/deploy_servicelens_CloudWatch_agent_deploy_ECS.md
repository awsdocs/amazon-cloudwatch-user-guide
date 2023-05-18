# Deploying the CloudWatch agent and the X\-Ray daemon on Amazon ECS<a name="deploy_servicelens_CloudWatch_agent_deploy_ECS"></a>

On Amazon ECS, you deploy the CloudWatch agent as a sidecar to your application container to collect metrics\. You can configure the CloudWatch Agent through SSM parameter store\.

## Creating IAM roles<a name="deploy_servicelens_CloudWatch_agent_deploy_ECS_roles"></a>

You must create two IAM roles\. If you already have created these roles, you may need to add permissions to them\.
+ **ECS task role—** Containers use this role to run\. The permissions should be whatever your applications need, plus **CloudWatchAgentServerPolicy** and **AWSXRayDaemonWriteAccess**\. 
+ **ECS task execution role—** Amazon ECS uses this role to launch and execute your containers\. If you have already created this role, attach the **AmazonSSMReadOnlyAccess**, **AmazonECSTaskExecutionRolePolicy**, and **CloudWatchAgentServerPolicy** policies to it\.

  If you need to store more sensitive data for Amazon ECS to use, see [Specifying Sensitive Data](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html) for more information\.

For more information about creating IAM roles, see [Creating IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html)\.

## Store the agent configuration in SSM Parameter Store<a name="deploy_servicelens_CloudWatch_agent_deploy_ECS_store"></a>

You need to make sure your agent configuration file has the following section, and then upload it to the SSM parameter store\.

```
{
  "logs": {
    "metrics_collected": {
      "emf": {}
    }
  }
}
```

**To upload the agent configuration to the SSM parameter store**

1. Put the agent configuration content into a local file `/tmp/ecs-cwagent.json`

1. Enter the following command\. Replace *region* with the Region of your cluster\.

   ```
   aws ssm put-parameter \
   --name "ecs-cwagent" \
   --type "String" \
   --value "`cat /tmp/ecs-cwagent.json`" \
   --region "region"
   ```

## Create a task definition and launch the task<a name="deploy_servicelens_CloudWatch_agent_deploy_ECS_definition"></a>

The steps for this task depend on whether you want to use the EC2 launch type or the Fargate launch type\.

### EC2 launch type<a name="deploy_servicelens_CloudWatch_agent_deploy_ECS_definition_EC2"></a>

First, create the task definition\. In this example, the container "demo\-app" sends X\-Ray SDK metrics to the CloudWatch agent and sends trace information to the X\-Ray daemon\.

Copy the following task definition to a local JSON file such as `/tmp/ecs-cwagent-ec2.json`\. Replace the following placeholders:
+ Replace *\{\{ecs\-task\-role\}\}* with the ARN of your ECS task role\.
+ Replace *\{\{ecs\-task\-execution\-role\}\}* with the ARN of your ECS task execution role\.
+ Replace *\{\{demo\-app\-image\}\}* with your application image that has X\-Ray SDK integration enabled\. Change the name from demo\-app to your own application name\.
+ Replace *\{\{region\}\}* with the name of the AWS Region where you want to send the logs for containers\. For example, `us-west-2`\. 

```
{
    "family": "ecs-cwagent-ec2",
    "taskRoleArn": "{{ecs-task-role}}",
    "executionRoleArn": "{{ecs-task-execution-role}}",
    "networkMode": "bridge",
    "containerDefinitions": [
        {
            "name": "demo-app",
            "image": "{{demo-app-image}}",
            "links": [
                "cloudwatch-agent",
                "xray-daemon"
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                  "awslogs-create-group": "True",
                  "awslogs-group": "/ecs/ecs-cwagent-ec2",
                  "awslogs-region": "{{region}}",
                  "awslogs-stream-prefix": "ecs"
                }
            }
        },
        {
            "name": "xray-daemon",
            "image": "public.ecr.aws/xray/aws-xray-daemon:latest",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                  "awslogs-create-group": "True",
                  "awslogs-group": "/ecs/ecs-cwagent-ec2",
                  "awslogs-region": "{{region}}",
                  "awslogs-stream-prefix": "ecs"
                }
            }
        },
        {
            "name": "cloudwatch-agent",
            "image": "public.ecr.aws/cloudwatch-agent/cloudwatch-agent:latest",
            "secrets": [
                {
                    "name": "CW_CONFIG_CONTENT",
                    "valueFrom": "ecs-cwagent"
                }
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                  "awslogs-create-group": "True",
                  "awslogs-group": "/ecs/ecs-cwagent-ec2",
                  "awslogs-region": "{{region}}",
                  "awslogs-stream-prefix": "ecs"
                }
            }
        }
    ],
    "requiresCompatibilities": [
        "EC2"
    ],
    "cpu": "256",
    "memory": "256"
}
```

Enter the following command to create the task definition\. Replace *\{\{region\}\}* with the Region of your cluster\.

```
aws ecs register-task-definition \
    --cli-input-json file:///tmp/ecs-cwagent-ec2.json \
    --region {{region}}
```

Enter the following command to launch the task\. Replace *\{\{cluster\-name\}\}* and *\{\{region\}\}* with the name and Region of your cluster\.

```
aws ecs run-task \
    --cluster {{cluster-name}} \
    --task-definition ecs-cwagent-ec2 \
    --region {{region}} \
    --launch-type EC2
```

### Fargate launch type<a name="deploy_servicelens_CloudWatch_agent_deploy_ECS_definition_Fargate"></a>

First, create the task definition\. In this example, the container “demo\-app” sends X\-Ray SDK metrics to the CloudWatch agent and sends trace information to the X\-Ray daemon\.

Copy the following task definition to a local JSON file such as `/tmp/ecs-cwagent-ec2.json`\. Replace the following placeholders:
+ Replace *\{\{ecs\-task\-role\}\}* with the Amazon Resource Name \(ARN\) of your ECS task role\.
+ Replace *\{\{ecs\-task\-execution\-role\}\}* with the ARN of your ECS task execution role\.
+ Replace *\{\{demo\-app\-image\}\}* with your application image that has X\-Ray SDK integration enabled\. Change the name from demo\-app to your own application name\.
+ Replace *\{\{region\}\}* with the name of the AWS Region where you want to send the logs for containers\. For example, `us-west-2`\. 

```
{
    "family": "ecs-cwagent-fargate",
    "taskRoleArn": "{{ecs-task-role}}",
    "executionRoleArn": "{{ecs-task-execution-role}}",
    "networkMode": "awsvpc",
    "containerDefinitions": [
        {
            "name": "demo-app",
            "image": "{{demo-app-image}}",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                  "awslogs-create-group": "True",
                  "awslogs-group": "/ecs/ecs-cwagent-fargate",
                  "awslogs-region": "{{region}}",
                  "awslogs-stream-prefix": "ecs"
                }
            }
        },
        {
            "name": "xray-daemon",
            "image": "public.ecr.aws/xray/aws-xray-daemon:latest",
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                  "awslogs-create-group": "True",
                  "awslogs-group": "/ecs/ecs-cwagent-fargate",
                  "awslogs-region": "{{region}}",
                  "awslogs-stream-prefix": "ecs"
                }
            }
        },
        {
            "name": "cloudwatch-agent",
            "image": "public.ecr.aws/cloudwatch-agent/cloudwatch-agent:latest",
            "secrets": [
                {
                    "name": "CW_CONFIG_CONTENT",
                    "valueFrom": "ecs-cwagent"
                }
            ],
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                  "awslogs-create-group": "True",
                  "awslogs-group": "/ecs/ecs-cwagent-fargate",
                  "awslogs-region": "{{region}}",
                  "awslogs-stream-prefix": "ecs"
                }
            }
        }
    ],
    "requiresCompatibilities": [
        "FARGATE"
    ],
    "cpu": "512",
    "memory": "1024"
}
```

Enter the following command to create the task definition\. Replace *\{\{region\}\}* with the Region of your cluster\.

```
aws ecs register-task-definition \
    --cli-input-json file:///tmp/ecs-cwagent-fargate.json \
    --region {{region}}
```

If you already have a Fargate cluster set up, you can use the task definition you just created to launch the task\. If you do not yet have any Fargate clusters, see [Configure the Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_GetStarted_Fargate.html#first-run-service) for more information about the rest of the steps to set up Fargate\. 