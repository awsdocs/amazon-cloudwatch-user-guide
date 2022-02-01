# Setting up Container Insights on Amazon ECS using AWS Distro for OpenTelemetry<a name="deploy-container-insights-ECS-adot"></a>

Use this section if you want to use AWS Distro for OpenTelemetry to set up CloudWatch Container Insights on an Amazon ECS cluster\. For more information about AWS Distro for Open Telemetry, see [AWS Distro for OpenTelemetry](https://aws.amazon.com/otel/)\. 

These steps assume that you already have a cluster running Amazon ECS\. For more information about using AWS Distro for Open Telemetry with Amazon ECS and setting up an Amazon ECS cluster for this purpose, see [Setting up AWS Distro for OpenTelemetry Collector in Amazon Elastic Container Service](https://aws-otel.github.io/docs/setup/ecs)\.

## Step 1: Create a task role<a name="deploy-container-insights-ECS-adot-CreateTaskRole"></a>

The first step is creating a task role in the cluster that the AWS OpenTelemetry Collector will use\.

**To create a task role for AWS Distro for OpenTelemetry**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies** and then choose **Create policy**\.

1. Choose the **JSON** tab and copy in the following policy:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "logs:PutLogEvents",
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:DescribeLogStreams",
                   "logs:DescribeLogGroups",
                   "ssm:GetParameters"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. For name, enter **AWSDistroOpenTelemetryPolicy**, and then choose **Create policy**\.

1. In the left navigation pane, choose **Roles** and then choose **Create role**\.

1. In the list of services, choose **Elastic Container Service**\.

1. Lower on the page, choose **Elastic Container Service Task** and then choose **Next: Permissions**\.

1. In the list of policies, search for **AWSDistroOpenTelemetryPolicy**\.

1. Select the check box next to **AWSDistroOpenTelemetryPolicy**\.

1. Coose **Next: Tags** and then choose **Next: Review\.**

1. For **Role name** enter **AWSOpenTelemetryTaskRole** and then choose **Create role**\.

## Step 2: Create a task execution role<a name="deploy-container-insights-ECS-adot-CreateTaskExecutionRole"></a>

The next step is creating a task execution role for the AWS OpenTelemetry Collector\.

**To create a task execution role for AWS Distro for OpenTelemetry**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles** and then choose **Create role**\.

1. In the list of services, choose **Elastic Container Service**\.

1. Lower on the page, choose **Elastic Container Service Task** and then choose **Next: Permissions**\.

1. In the list of policies, search for **AmazonECSTaskExecutionRolePolicy** and then select the check box next to **AmazonECSTaskExecutionRolePolicy**\.

1. In the list of policies, search for **CloudWatchLogsFullAccess** and then select the check box next to **CloudWatchLogsFullAccess**\.

1. In the list of policies, search for **AmazonSSMReadOnlyAccess** and then select the check box next to **AmazonSSMReadOnlyAccess**\.

1. Choose **Next: Tags** and then choose **Next: Review\.**

1. For **Role name** enter **AWSOpenTelemetryTaskExecutionRole** and then choose **Create role**\.

## Step 3: Create a task definition<a name="deploy-container-insights-ECS-adot-CreateTaskDefinition"></a>

The next step is creating a task definition\.

**To create a task definition for AWS Distro for OpenTelemetry**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the left navigation pane, choose **Task Definitions** and then choose **Create new Task Definition**\.

1. Select either **FARGATE** or **EC2** and then choose **Next step**\.

1. Enter a task definition name such as **aws\-otel**\.

1. For **Task Role**, select **AWSOpenTelemetryTaskRole** which you created earlier\.

1. For **Task execution role**, select **AWSOpenTelemetryTaskExecutionRole** which you created earlier\.

1. Fill in the **Task memory** and **Task CPU**\.

1. Under **Container Definitions**, choose **Add container**\.

1. For **Container name**, enter **aws\-otel\-collector**\. For **Image**, enter **public\.ecr\.aws/aws\-observability/aws\-otel\-collector**\.

1. Under **ENVIRONMENT**, for **Command** enter **\-\-config=/etc/ecs/container\-insights/otel\-task\-metrics\-config\.yaml** 

   This YAML file is included in the Docker image, and includes the configuration to consume container metrics\.

1. If you're using the EC2 launch type, enter a port mapping of 55680 for TCP\.

1. Finish the steps for adding the container\.

For more information about using the AWS OpenTelemetry collector with Amazon ECS, see [Setting up AWS Distro for OpenTelemetry Collector in Amazon Elastic Container Service](https://aws-otel.github.io/docs/setup/ecs)\.

## Step 4: Run the task<a name="deploy-container-insights-ECS-adot-CreateTaskDefinition"></a>

The final step is running the task that you've created\.

**To run the task for AWS Distro for OpenTelemetry**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. In the left navigation pane, choose **Task Definitions** and then select the task that you just created\.

1. Choose **Actions**, **Run Task**\. 

   Next, you can check for the new metrics in the CloudWatch console\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Metrics**\.

   You should see a **ECS/ContainerInsights** namespace\. Choose that namespace and you should see eight metrics\.