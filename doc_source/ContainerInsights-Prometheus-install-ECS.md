# Install the CloudWatch Agent with Prometheus Metrics Collection on Amazon ECS clusters<a name="ContainerInsights-Prometheus-install-ECS"></a>

This section explains how to set up the CloudWatch agent with Prometheus monitoring in a cluster running Amazon ECS\. After you do this, the agent automatically scrapes and imports metrics for the following workloads running in that cluster\.
+ AWS App Mesh
+ Java/JMX

You can also configure the agent to scrape and import metrics from additional Prometheus workloads and sources\.

## Set up IAM roles<a name="ContainerInsights-Prometheus-Setup-ECS-IAM"></a>

You need two IAM roles for the CloudWatch agent task definition\. If you specify **CreateIAMRoles=True** in the AWS CloudFormation stack to have Container Insights create these roles for you, the roles will be created with the correct permissions\. If you want to create them yourself or use existing roles, the following roles and permissions are required\.
+ **CloudWatch agent ECS task role**— The CloudWatch agent container uses this role\. It must include the **CloudwatchAgentServerPolicy** policy and a customer\-managed policy which contains the following read\-only permissions:
  + `ec2:DescribeInstances`
  + `ecs:ListTasks`
  + `ecs:ListServices`
  + `ecs:DescribeContainerInstances`
  + `ecs:DescribeServices`
  + `ecs:DescribeTasks`
  + `ecs:DescribeTaskDefinition`
+ **CloudWatch agent ECS task execution role**— This is the role that Amazon ECS requires to launch and execute your containers\. Ensure that your task execution role has the **AmazonSSMReadOnlyAccess**, **AmazonECSTaskExecutionRolePolicy**, and **CloudWatchAgentServerPolicy** policies attached\. If you want to store more sensitive data for Amazon ECS to use, see [ Specifying sensitive data](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html)\.

## Install the CloudWatch Agent with Prometheus Monitoring by using AWS CloudFormation<a name="ContainerInsights-Prometheus-Setup-ECS-CFN"></a>

You use AWS CloudFormation to install the CloudWatch agent with Prometheus monitoring for Amazon ECS clusters\. The following list shows the parameters you will use in the AWS CloudFormation template\.
+ **ECSClusterName**— Specifies the target Amazon ECS cluster\.
+ **CreateIAMRoles**— Specify **True** to create new roles for the Amazon ECS task role and Amazon ECS task execution role\. Specify **False** to reuse existing roles\.
+ **TaskRoleName**— If you specified **True** for **CreateIAMRoles**, this specifies the name to use for the new Amazon ECS task role\. If you specified **False** for **CreateIAMRoles**, this specifies the existing role to use as the Amazon ECS task role\. 
+ **ExecutionRoleName**— If you specified **True** for **CreateIAMRoles**, this specifies the name to use for the new Amazon ECS task execution role\. If you specified **False** for **CreateIAMRoles**, this specifies the existing role to use as the Amazon ECS task execution role\. 
+ **ECSNetworkMode**— If you are using EC2 launch type, specify the network mode here\. It must be either **bridge** or **host**\.
+ **ECSLaunchType**— Specify either **fargate** or **EC2**\.
+ **SecurityGroupID**— If the **ECSNetworkMode** is **awsvpc**, specify the security group ID here\.
+ **SubnetID**— If the **ECSNetworkMode** is **awsvpc**, specify the subnet ID here\.

### Command samples<a name="ContainerInsights-Prometheus-Setup-ECS-CFNcommands"></a>

This section includes sample AWS CloudFormation commands to install Container Insights with Prometheus monitoring in various scenarios\.

**Note**  
The following setup step pulls the container image from Docker Hub as an anonymous user by default\. This pull may be subject to a rate limit\. For more information, see [CloudWatch agent container image](ContainerInsights.md#container-insights-download-limit)\.

**Create AWS CloudFormation stack for an Amazon ECS cluster in bridge network mode**

```
export AWS_PROFILE=your_aws_config_profile_eg_default
export AWS_DEFAULT_REGION=your_aws_region_eg_ap-southeast-1
export ECS_CLUSTER_NAME=your_ec2_ecs_cluster_name
export ECS_NETWORK_MODE=bridge
export CREATE_IAM_ROLES=True
export ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
export ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name

curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-bridge-host.yaml

aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
    --template-body file://cwagent-ecs-prometheus-metric-for-bridge-host.yaml \
    --parameters ParameterKey=ECSClusterName,ParameterValue=${ECS_CLUSTER_NAME} \
                 ParameterKey=CreateIAMRoles,ParameterValue=${CREATE_IAM_ROLES} \
                 ParameterKey=ECSNetworkMode,ParameterValue=${ECS_NETWORK_MODE} \
                 ParameterKey=TaskRoleName,ParameterValue=${ECS_TASK_ROLE_NAME} \
                 ParameterKey=ExecutionRoleName,ParameterValue=${ECS_EXECUTION_ROLE_NAME} \
    --capabilities CAPABILITY_NAMED_IAM \
    --region ${AWS_DEFAULT_REGION} \
    --profile ${AWS_PROFILE}
```

**Create AWS CloudFormation stack for an Amazon ECS cluster in host network mode**

```
export AWS_PROFILE=your_aws_config_profile_eg_default
export AWS_DEFAULT_REGION=your_aws_region_eg_ap-southeast-1
export ECS_CLUSTER_NAME=your_ec2_ecs_cluster_name
export ECS_NETWORK_MODE=host
export CREATE_IAM_ROLES=True
export ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
export ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name


curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-bridge-host.yaml

aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-EC2-${ECS_NETWORK_MODE} \
    --template-body file://cwagent-ecs-prometheus-metric-for-bridge-host.yaml \
    --parameters ParameterKey=ECSClusterName,ParameterValue=${ECS_CLUSTER_NAME} \
                 ParameterKey=CreateIAMRoles,ParameterValue=${CREATE_IAM_ROLES} \
                 ParameterKey=ECSNetworkMode,ParameterValue=${ECS_NETWORK_MODE} \
                 ParameterKey=TaskRoleName,ParameterValue=${ECS_TASK_ROLE_NAME} \
                 ParameterKey=ExecutionRoleName,ParameterValue=${ECS_EXECUTION_ROLE_NAME} \ 
    --capabilities CAPABILITY_NAMED_IAM \
    --region ${AWS_DEFAULT_REGION} \
    --profile ${AWS_PROFILE}
```

**Create AWS CloudFormation stack for an Amazon ECS cluster in awsvpc network mode**

```
export AWS_PROFILE=your_aws_config_profile_eg_default
export AWS_DEFAULT_REGION=your_aws_region_eg_ap-southeast-1
export ECS_CLUSTER_NAME=your_ec2_ecs_cluster_name
export ECS_LAUNCH_TYPE=EC2
export CREATE_IAM_ROLES=True
export ECS_CLUSTER_SECURITY_GROUP=your_security_group_eg_sg-xxxxxxxxxx
export ECS_CLUSTER_SUBNET=your_subnet_eg_subnet-xxxxxxxxxx
export ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
export ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name

curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-awsvpc.yaml

aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-${ECS_LAUNCH_TYPE}-awsvpc \
    --template-body file://cwagent-ecs-prometheus-metric-for-awsvpc.yaml \
    --parameters ParameterKey=ECSClusterName,ParameterValue=${ECS_CLUSTER_NAME} \
                 ParameterKey=CreateIAMRoles,ParameterValue=${CREATE_IAM_ROLES} \
                 ParameterKey=ECSLaunchType,ParameterValue=${ECS_LAUNCH_TYPE} \
                 ParameterKey=SecurityGroupID,ParameterValue=${ECS_CLUSTER_SECURITY_GROUP} \
                 ParameterKey=SubnetID,ParameterValue=${ECS_CLUSTER_SUBNET} \
                 ParameterKey=TaskRoleName,ParameterValue=${ECS_TASK_ROLE_NAME} \
                 ParameterKey=ExecutionRoleName,ParameterValue=${ECS_EXECUTION_ROLE_NAME} \
    --capabilities CAPABILITY_NAMED_IAM \
    --region ${AWS_DEFAULT_REGION} \
    --profile ${AWS_PROFILE}
```

**Create AWS CloudFormation stack for a Fargate cluster in awsvpc network mode**

```
export AWS_PROFILE=your_aws_config_profile_eg_default
export AWS_DEFAULT_REGION=your_aws_region_eg_ap-southeast-1
export ECS_CLUSTER_NAME=your_ec2_ecs_cluster_name
export ECS_LAUNCH_TYPE=FARGATE
export CREATE_IAM_ROLES=True
export ECS_CLUSTER_SECURITY_GROUP=your_security_group_eg_sg-xxxxxxxxxx
export ECS_CLUSTER_SUBNET=your_subnet_eg_subnet-xxxxxxxxxx
export ECS_TASK_ROLE_NAME=your_selected_ecs_task_role_name
export ECS_EXECUTION_ROLE_NAME=your_selected_ecs_execution_role_name            

curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-awsvpc.yaml

aws cloudformation create-stack --stack-name CWAgent-Prometheus-ECS-${ECS_CLUSTER_NAME}-${ECS_LAUNCH_TYPE}-awsvpc \
    --template-body file://cwagent-ecs-prometheus-metric-for-awsvpc.yaml \
    --parameters ParameterKey=ECSClusterName,ParameterValue=${ECS_CLUSTER_NAME} \
                 ParameterKey=CreateIAMRoles,ParameterValue=${CREATE_IAM_ROLES} \
                 ParameterKey=ECSLaunchType,ParameterValue=${ECS_LAUNCH_TYPE} \
                 ParameterKey=SecurityGroupID,ParameterValue=${ECS_CLUSTER_SECURITY_GROUP} \
                 ParameterKey=SubnetID,ParameterValue=${ECS_CLUSTER_SUBNET} \
                 ParameterKey=TaskRoleName,ParameterValue=${ECS_TASK_ROLE_NAME} \
                 ParameterKey=ExecutionRoleName,ParameterValue=${ECS_EXECUTION_ROLE_NAME} \
    --capabilities CAPABILITY_NAMED_IAM \
    --region ${AWS_DEFAULT_REGION} \
    --profile ${AWS_PROFILE}
```

### AWS resources created by the AWS CloudFormation stack<a name="ContainerInsights-Prometheus-Setup-ECS-resources"></a>

The following table lists the AWS resources that are created when you use AWS CloudFormation to set up Container Insights with Prometheus monitoring on an Amazon ECS cluster\.


| Resource type | Resource name | Comments | 
| --- | --- | --- | 
|  AWS::SSM::Parameter  |  AmazonCloudWatch\-CWAgentConfig\-$*ECS\_CLUSTER\_NAME*\-$*ECS\_LAUNCH\_TYPE*\-$*ECS\_NETWORK\_MODE*  |  This is the CloudWatch agent with the default App Mesh and Java/JMX embedded metric format definition\.  | 
|  AWS::SSM::Parameter  |  AmazonCloudWatch\-PrometheusConfigName\-$*ECS\_CLUSTER\_NAME*\-$*ECS\_LAUNCH\_TYPE*\-$*ECS\_NETWORK\_MODE*  |  This is the Prometheus scraping configuration\.  | 
|  AWS::IAM::Role  |  **$ECS\_TASK\_ROLE\_NAME**\.   |  The Amazon ECS task role\. This is created only if you specified **True** for `CREATE_IAM_ROLES`\.  | 
|  AWS::IAM::Role  |  **$\{ECS\_EXECUTION\_ROLE\_NAME\}**   |  The Amazon ECS task execution role\. This is created only if you specified **True** for `CREATE_IAM_ROLES`\.  | 
|  AWS::ECS::TaskDefinition  |  cwagent\-prometheus\-$*ECS\_CLUSTER\_NAME*\-$*ECS\_LAUNCH\_TYPE*\-$*ECS\_NETWORK\_MODE*   |   | 
|  AWS::ECS::Service  |  cwagent\-prometheus\-replica\-service\-$*ECS\_LAUNCH\_TYPE*\-$*ECS\_NETWORK\_MODE*  |   | 

### Deleting the AWS CloudFormation stack for the CloudWatch agent with Prometheus monitoring<a name="ContainerInsights-Prometheus-ECS-delete"></a>

To delete the CloudWatch agent from an Amazon ECS cluster, enter these commands\.

```
export AWS_PROFILE=your_aws_config_profile_eg_default
export AWS_DEFAULT_REGION=your_aws_region_eg_ap-southeast-1
export CLOUDFORMATION_STACK_NAME=your_cloudformation_stack_name

aws cloudformation delete-stack \
--stack-name ${CLOUDFORMATION_STACK_NAME} \
--region ${AWS_DEFAULT_REGION} \
--profile ${AWS_PROFILE}
```