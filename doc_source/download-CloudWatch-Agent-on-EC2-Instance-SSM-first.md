# Download and configure the CloudWatch agent<a name="download-CloudWatch-Agent-on-EC2-Instance-SSM-first"></a>

This section explains how to use Systems Manager to download the agent and then how to create your agent configuration file\. Before you can use Systems Manager to download the agent, you must make sure that the instance is configured correctly for Systems Manager\.

## Installing or updating SSM Agent<a name="update-SSM-Agent-EC2instance-first"></a>

On an Amazon EC2 instance, the CloudWatch agent requires that the instance is running version 2\.2\.93\.0 or later\. Before you install the CloudWatch agent, update or install SSM Agent on the instance if you haven't already done so\. 

For information about installing or updating SSM Agent on an instance running Linux, see [ Installing and Configuring SSM Agent on Linux Instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html) in the *AWS Systems Manager User Guide*\.

For information about installing or updating the SSM Agent, see [Working with SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) in the *AWS Systems Manager User Guide*\.

## \(Optional\) Verify Systems Manager prerequisites<a name="install-CloudWatch-Agent-minimum-requirements-first"></a>

Before you use Systems Manager Run Command to install and configure the CloudWatch agent, verify that your instances meet the minimum Systems Manager requirements\. For more information, see [Systems Manager Prerequisites](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html#systems-manager-prereqs) in the *AWS Systems Manager User Guide*\.

## Verify internet access<a name="install-CloudWatch-Agent-internet-access-first"></a>

Your Amazon EC2 instances must have outbound internet access to send data to CloudWatch or CloudWatch Logs\. For more information about how to configure internet access, see [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) in the *Amazon VPC User Guide*\.

The endpoints and ports to configure on your proxy are as follows:
+ If you're using the agent to collect metrics, you must whitelist the CloudWatch endpoints for the appropriate Regions\. These endpoints are listed in [Amazon CloudWatch](https://docs.aws.amazon.com/general/latest/gr/rande.html#cw_region) in the *Amazon Web Services General Reference*\. 
+ If you're using the agent to collect logs, you must whitelist the CloudWatch Logs endpoints for the appropriate Regions\. These endpoints are listed in [Amazon CloudWatch Logs](https://docs.aws.amazon.com/general/latest/gr/rande.html#cwl_region) in the *Amazon Web Services General Reference*\. 
+ If you're using Systems Manager to install the agent or Parameter Store to store your configuration file, you must whitelist the Systems Manager endpoints for the appropriate Regions\. These endpoints are listed in [AWS Systems Manager](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *Amazon Web Services General Reference*\. 

Use the following steps to download the CloudWatch agent package using Systems Manager\.

**To download the CloudWatch agent using Systems Manager**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In the **Targets** area, choose the instance to install the CloudWatch agent on\. If you don't see a specific instance, it might not be configured as a managed instance for use with Systems Manager\. For more information, see [Setting Up AWS Systems Manager for Hybrid Environments](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html) in the *AWS Systems Manager User Guide*\.

1. In the **Action** list, choose **Install**\.

1. In the **Name** field, enter *AmazonCloudWatchAgent*\.

1. Keep **Version** set to **latest** to install the latest version of the agent\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully installed\.

   

## Create and modify the agent configuration file<a name="CW-Agent-Instance-Create-Configuration-File-first"></a>

After you have downloaded the CloudWatch agent, you must create the configuration file before you start the agent on any servers\.

If you're going to save your agent configuration file in the Systems Manager Parameter Store, you must use an EC2 instance to save to the Parameter Store\. Additionally, you must first attach to that instance the `CloudWatchAgentAdminRole` IAM role\. For more information about attaching roles, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Windows Instances*\.

For more information about creating the CloudWatch agent configuration file, see [Create the CloudWatch agent configuration file](create-cloudwatch-agent-configuration-file.md)\.