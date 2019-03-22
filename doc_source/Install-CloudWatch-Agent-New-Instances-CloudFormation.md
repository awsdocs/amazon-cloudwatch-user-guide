# Install the CloudWatch Agent on New Instances Using AWS CloudFormation<a name="Install-CloudWatch-Agent-New-Instances-CloudFormation"></a>

Amazon has uploaded several AWS CloudFormation templates to GitHub to help you install and update the CloudWatch agent\. For more information about using AWS CloudFormation, see [ What is AWS CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\.

The template location is [ Deploy CloudWatch Agent to EC2 instances by AWS CloudFormation](https://github.com/awslabs/aws-cloudformation-templates/tree/master/aws/solutions/AmazonCloudWatchAgent)\. This location includes both **inline** and **ssm** directories\. Each of these directories contains templates for both Linux and Windows instances\. 
+ The templates in the **inline** directory have the CloudWatch agent configuration embedded into the AWS CloudFormation template\. By default, the Linux templates collect the metrics `mem_used_percent` and `swap_used_percent`, while the Windows templates collect `Memory % Committed Bytes In Use` and `Paging File % Usage`\.

  You can modify these templates to collect different metrics by modifying the following section of the template\. The following example is from the template for Linux servers\. Follow the format and syntax of the agent configuration file to make these changes\. For more information, see [ Manually Create or Edit the CloudWatch Agent Configuration File](CloudWatch-Agent-Configuration-File-Details.md)\.

  ```
  {
     "metrics":{
        "append_dimensions":{
           "AutoScalingGroupName":"${!aws:AutoScalingGroupName}",
           "ImageId":"${!aws:ImageId}",
           "InstanceId":"${!aws:InstanceId}",
           "InstanceType":"${!aws:InstanceType}"
        },
        "metrics_collected":{
           "mem":{
              "measurement":[
                 "mem_used_percent"
              ]
           },
           "swap":{
              "measurement":[
                 "swap_used_percent"
              ]
           }
        }
     }
  }
  ```
**Note**  
In the inline templates, all placeholder variables must have an exclamation mark \(\!\) before them as an escape character\. You can see this in the example template above\. If you add other placeholder variables, be sure to add an exclamation mark before the name\.
+ The templates in the **ssm** directory load an agent configuration file from Parameter Store\. To use these templates, you must first create a configuration file and upload it to Parameter Store\. You then provide the Parameter Store name of the file in the template\. You can create the configuration file manually or by using the wizard\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

You can use both types of templates for installing the CloudWatch agent, and for updating the agent configuration\.

## Tutorial: Install and Update the CloudWatch Agent Using an AWS CloudFormation Inline Template<a name="installing-CloudWatch-Agent-using-CloudFormation-Templates-inline"></a>

This tutorial walks you through using AWS CloudFormation to install the CloudWatch agent on a new Amazon EC2 instance\. This tutorial installs on a new instance running Amazon Linux 2 using the inline templates, which don't require the use of Parameter Store\. The inline template includes the agent configuration in the template\. In this tutorial, you use the default agent configuration contained in the template\.

After the procedure for installing the agent, the tutorial continues with how to update the agent\.

**To use AWS CloudFormation to install the CloudWatch agent on a new instance**

1. Download the template from GitHub\. In this tutorial, download the inline template for Amazon Linux 2:

   ```
   curl -O https://raw.githubusercontent.com/awslabs/aws-cloudformation-templates/master/aws/solutions/AmazonCloudWatchAgent/inline/amazon_linux.template
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create stack**\.

1. For **Choose a template**, select **Upload a template to Amazon S3**, choose the downloaded template, and choose **Next**\.

1. On the **Specify Details** page, fill out the following parameters, and choose **Next**:
   + **Stack name**: Choose a stack name for your AWS CloudFormation stack\. 
   + **IAMRole**: Choose an IAM role that has permissions to write CloudWatch metrics and logs\. For more information, see [Create IAM Roles to Use with CloudWatch Agent on Amazon EC2 Instances](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-roles)\.
   + **InstanceAMI**: Choose an AMI that is valid in the Region where you are going to launch your stack\.
   + **InstanceType**: Choose a valid instance type\.
   + **KeyName**: To enable SSH access to the new instance, choose an existing Amazon EC2 key pair\. If you don't already have an Amazon EC2 key pair, you can create one in the AWS Management Console\. For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.
   + **SSHLocation**: Specifies the IP address range that can be used to connect to the instance using SSH\. The default allows access from any IP address\.

1. On the **Options** page, you can choose to tag your stack resources\. Choose **Next**\.

1. On the **Review** page, review your information, acknowledge that the stack might create IAM resources, and then choose **Create**\.

   If you refresh the console, you see that the new stack has the `CREATE_IN_PROGRESS` status\.

1. When the instance is created, you can see it in the Amazon EC2 console\. Optionally, you can then connect to the host and check the progress\.

   Use the following command to confirm that the agent is installed:

   ```
   rpm -qa amazon-cloudwatch-agent
   ```

   Use the following command to confirm that the agent is running:

   ```
   ps aux | grep amazon-cloudwatch-agent
   ```

The next procedure demonstrates using AWS CloudFormation to update the CloudWatch agent using an inline template\. The default inline template collects the `mem_used_percent` metric\. In this tutorial, you change the agent configuration to stop collecting that metric\.

**To use AWS CloudFormation to update the CloudWatch agent**

1. In the template that you downloaded in the previous procedure, remove the following lines and then save the template:

   ```
   "mem": {
                           
        "measurement": [
            "mem_used_percent"
          ]
    },
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the AWS CloudFormation dashboard, select the stack that you created, and choose **Update Stack**\.

1. For **Select Template**, select **Upload a template to Amazon S3**, choose the template that you modified, and choose **Next**\.

1. On the **Options** page, choose **Next**, **Next**\.

1. On the **Review** page, review your information and choose **Update**\.

   After some time, you see `UPDATE_COMPLETE`\.

## Tutorial: Install the CloudWatch Agent Using AWS CloudFormation and Parameter Store<a name="installing-CloudWatch-Agent-using-CloudFormation-Templates"></a>

This tutorial walks you through using AWS CloudFormation to install the CloudWatch agent on a new Amazon EC2 instance\. This tutorial installs on a new instance running Amazon Linux 2 using an agent configuration file that you create and save in Parameter Store\.

After the procedure for installing the agent, the tutorial continues with how to update the agent\.

**To use AWS CloudFormation to install the CloudWatch agent on a new instance using a configuration from Parameter Store**

1. Create the agent configuration file, and save it in Parameter Store\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

1. Download the template from GitHub:

   ```
   curl -O https://raw.githubusercontent.com/awslabs/aws-cloudformation-templates/master/aws/solutions/AmazonCloudWatchAgent/ssm/amazon_linux.template
   ```

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create stack**\.

1. For **Choose a template**, select **Upload a template to Amazon S3**, choose the template that you downloaded, and choose **Next**\.

1. On the **Specify Details** page, fill out the following parameters accordingly, and choose **Next**:
   + **Stack name**: Choose a stack name for your AWS CloudFormation stack\. 
   + **IAMRole**: Choose an IAM role that has permissions to write CloudWatch metrics and logs\. For more information, see [Create IAM Roles to Use with CloudWatch Agent on Amazon EC2 Instances](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-roles)\.
   + **InstanceAMI**: Choose an AMI that is valid in the Region where you are going to launch your stack\.
   + **InstanceType**: Choose a valid instance type\.
   + **KeyName**: To enable SSH access to the new instance, choose an existing Amazon EC2 key pair\. If you don't already have an Amazon EC2 key pair, you can create one in the AWS Management Console\. For more information, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\.
   + **SSHLocation**: Specifies the IP address range that can be used to connect to the instance using SSH\. The default allows access from any IP address\.
   + **SSMKey**: Specifies the agent configuration file that you created and saved in Parameter Store\.

1. On the **Options** page, you can choose to tag your stack resources\. Choose **Next**\.

1. On the **Review** page, review your information, acknowledge that the stack might create IAM resources, and then choose **Create**\.

   If you refresh the console, you see that the new stack has the `CREATE_IN_PROGRESS` status\.

1. When the instance is created, you can see it in the Amazon EC2 console\. Optionally, you can then connect to the host and check the progress\.

   Use the following command to confirm that the agent is installed:

   ```
   rpm -qa amazon-cloudwatch-agent
   ```

   Use the following command to confirm that the agent is running:

   ```
   ps aux | grep amazon-cloudwatch-agent
   ```

The next procedure demonstrates using AWS CloudFormation to update the CloudWatch agent, using an agent configuration that you saved in Parameter Store\.

**To use AWS CloudFormation to update the CloudWatch agent using a configuration in Parameter Store**

1. Change the agent configuration file stored in Parameter Store to the new configuration you want\.

1. In the AWS CloudFormation template that you downloaded in the [Tutorial: Install the CloudWatch Agent Using AWS CloudFormation and Parameter Store](#installing-CloudWatch-Agent-using-CloudFormation-Templates) topic, change the version number\. For example, you might change `VERSION=1.0` to `VERSION=2.0`\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the AWS CloudFormation dashboard, select the stack that you created, and choose **Update Stack**\.

1. For **Select Template**, select **Upload a template to Amazon S3**, select the template that you just modified, and choose **Next**\.

1. On the **Options** page, choose **Next**, **Next**\.

1. On the **Review** page, review your information and choose **Update**\.

   After some time, you see `UPDATE_COMPLETE`\.

## Troubleshooting Using the CloudWatch Agent With AWS CloudFormation<a name="CloudWatch-Agent-CloudFormation-troubleshooting"></a>

This section helps you troubleshoot issues with installing and updating the CloudWatch agent using AWS CloudFormation\.

### Detecting When an Update Fails<a name="CloudWatch-Agent-troubleshooting-Detecting-CloudFormation-update-issues"></a>

If you use AWS CloudFormation to update your CloudWatch agent configuration, and use an invalid configuration, the agent stops sending any metrics to CloudWatch\. A quick way to check whether an agent configuration update succeeded is to look at the `cfn-init-cmd.log` file\. On a Linux server, the file is located at `/var/log/cfn-init-cmd.log`\. On a Windows instance, the file is located at `C:\cfn\log\cfn-init-cmd.log`\.

### Metrics Are Missing<a name="CloudWatch-Agent-troubleshooting-Cloudformation-missing-metrics"></a>

If you do not see metrics that you expect to see after installing or updating the agent, confirm that the agent is configured to collect that metric\. To do this, check the `amazon-cloudwatch-agent.json` file to make sure that the metric is listed, and that you are looking in the correct metric namespace\. For more information, see [CloudWatch Agent Files and Locations](troubleshooting-CloudWatch-Agent.md#CloudWatch-Agent-files-and-locations)\.