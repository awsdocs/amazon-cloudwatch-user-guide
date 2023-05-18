# Deploying the AWS Distro for OpenTelemetry to collect EC2 instance\-level metrics on Amazon ECS clusters<a name="deploy-container-insights-ECS-OTEL"></a>

Use the steps in this section to use AWS Distro for OpenTelemetry to collect EC2 instance\-level metrics on an Amazon ECS cluster\. For more information about the AWS Distro for OpenTelemetry, see [AWS Distro for OpenTelemetry](https://aws.amazon.com/otel/)\.

These steps assume that you already have a cluster running Amazon ECS\. This cluster must be deployed with the EC2 launch type\. For more information about using AWS Distro for Open Telemetry with Amazon ECS and setting up an Amazon ECS cluster for this purpose, see [Setting up AWS Distro for OpenTelemetry Collector in Amazon Elastic Container Service for ECS EC2 instance level metrics](https://aws-otel.github.io/docs/setup/ecs#3-setup-the-aws-otel-collector-for-ecs-ec2-instance-metrics)\. 

**Topics**
+ [Quick setup using AWS CloudFormation](#container-insights-ECS-OTEL-quicksetup)
+ [Manual and custom setup](#container-insights-ECS-OTEL-custom)

## Quick setup using AWS CloudFormation<a name="container-insights-ECS-OTEL-quicksetup"></a>

Download the AWS CloudFormation template file for installing the AWS Distro for OpenTelemetry collector for Amazon ECS on EC2\. Run the following curl command\.

```
curl -O https://raw.githubusercontent.com/aws-observability/aws-otel-collector/main/deployment-template/ecs/aws-otel-ec2-instance-metrics-daemon-deployment-cfn.yaml
```

After you download the template file, open it and replace *PATH\_TO\_CloudFormation\_TEMPLATE* with the path where you saved the template file\. Then export the following parameters and run the AWS CloudFormation command, as shown in the following command\.
+ **Cluster\_Name**– The Amazon ECS cluster name
+ **AWS\_Region**– The Region where the data will be sent
+ **PATH\_TO\_CloudFormation\_TEMPLATE**– The path where you saved the AWS CloudFormation template file\.
+ **command**– To enable the AWS Distro for OpenTelemetry collector to collect the instance\-level metrics for Amazon ECS on Amazon EC2, you must specify `--config=/etc/ecs/otel-instance-metrics-config.yaml` for this parameter\.

```
ClusterName=Cluster_Name
Region=AWS_Region
command=--config=/etc/ecs/otel-instance-metrics-config.yaml
aws cloudformation create-stack --stack-name AOCECS-${ClusterName}-${Region} \
--template-body file://PATH_TO_CloudFormation_TEMPLATE \
--parameters ParameterKey=ClusterName,ParameterValue=${ClusterName} \
ParameterKey=CreateIAMRoles,ParameterValue=True \
ParameterKey=command,ParameterValue=${command} \
--capabilities CAPABILITY_NAMED_IAM \
--region ${Region}
```

After running this command, use the Amazon ECS console to see if the task is running\.

### Troubleshooting the quick setup<a name="container-insights-ECS-OTEL-quicksetup-troubleshooting"></a>

To check the status of the AWS CloudFormation stack, enter the following command\.

```
ClusterName=cluster-name
Region=cluster-region
aws cloudformation describe-stack --stack-name AOCECS-$ClusterName-$Region --region $Region
```

If the value of `StackStatus` is anything other than `CREATE_COMPLETE` or `CREATE_IN_PROGRESS`, check the stack events to find the error\. Enter the following command\.

```
ClusterName=cluster-name
Region=cluster-region
aws cloudformation describe-stack-events --stack-name AOCECS-$ClusterName-$Region --region $Region
```

To check the status of the `AOCECS` daemon service, enter the following command\. In the output, you should see that `runningCount` is equal to the `desiredCount` in the deployment section\. If it isn't equal, check the failures section in the output\.

```
ClusterName=cluster-name
Region=cluster-region
aws ecs describe-services --services AOCECS-daemon-service --cluster $ClusterName --region $Region
```

You can also use the CloudWatch Logs console to check the agent log\. Look for the **/aws/ecs/containerinsights/\{ClusterName\}/performance** log group\.

## Manual and custom setup<a name="container-insights-ECS-OTEL-custom"></a>

Follow the steps in this section to manually deploy the AWS Distro for OpenTelemetry to collect instance\-level metrics from your Amazon ECS clusters that are hosted on Amazon EC2 instances\.

### Step 1: Necessary roles and policies<a name="container-insights-ECS-OTEL-custom-iam"></a>

Two IAM roles are required\. You must create them if they don't already exist\. For more information about these roles, see [Create IAM policy](https://aws-otel.github.io/docs/setup/ecs/create-iam-policy) and [Create IAM role](https://aws-otel.github.io/docs/setup/ecs/create-iam-role)\.

### Step 2: Create the task definition<a name="container-insights-ECS-OTEL-custom-task"></a>

Create a task definition and use it to launch the AWS Distro for OpenTelemetry as a daemon service\.

To use the task definition template to create the task definition, follow the instructions in [ Create ECS EC2 Task Definition for EC2 instance with AWS OTel Collector](https://aws-otel.github.io/docs/setup/ecs/task-definition-for-ecs-ec2-instance)\.

To use the Amazon ECS console to create the task definition, follow the instructions in [ Install AWS OTel Collector by creating Task Definition through AWS console for Amazon ECS EC2 instance metrics](https://aws-otel.github.io/docs/setup/ecs/create-task-definition-instance-console)\.

### Step 3: Launch the daemon service<a name="container-insights-ECS-OTEL-custom-launch"></a>

To launch the AWS Distro for OpenTelemetry as a daemon service, follow the instructions in [ Run your task on the Amazon Elastic Container Service \(Amazon ECS\) using daemon service](https://aws-otel.github.io/docs/setup/ecs/run-daemon-service)\.

### \(Optional\) Advanced configuration<a name="container-insights-ECS-OTEL-custom-advancdeconfig"></a>

Optionally, you can use SSM to specify other configuration options for the AWS Distro for OpenTelemetry in your Amazon ECS clusters that are hosted on Amazon EC2 instances\. For more information, about creating a configuration file, see [ Custom OpenTelemetry Configuration](https://aws-otel.github.io/docs/setup/ecs#5-custom-opentelemetry-configuration)\. For more information about the options that you can use in the configuration file, see [AWS Container Insights Receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/receiver/awscontainerinsightreceiver/README.md)\.