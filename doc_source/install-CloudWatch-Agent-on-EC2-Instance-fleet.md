# Installing the CloudWatch Agent on EC2 Instances Using Your Agent Configuration<a name="install-CloudWatch-Agent-on-EC2-Instance-fleet"></a>

After you have a CloudWatch agent configuration saved in Parameter Store, you can use it when you install the agent on other servers\.

**Topics**
+ [Attach an IAM Role to the Instance](#install-CloudWatch-Agent-iam_permissions-fleet)
+ [Download the CloudWatch Agent Package on an Amazon EC2 Instance](#download-CloudWatch-Agent-on-EC2-Instance-fleet)
+ [\(Optional\) Modify the Common Configuration and Named Profile for CloudWatch Agent](#CloudWatch-Agent-profile-instance-fleet)
+ [Start the CloudWatch Agent](#start-CloudWatch-Agent-EC2-fleet)

## Attach an IAM Role to the Instance<a name="install-CloudWatch-Agent-iam_permissions-fleet"></a>

You must attach an IAM role to the EC2 instance to be able to run the CloudWatch agent on the instance\. This role enables the CloudWatch agent to perform actions on the instance\. Use the role you created earlier that includes just the permissions needed for installing and running the agent\. This role might be called `CloudWatchAgentServerPolicy`\.

For more information, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Windows Instances*\.

## Download the CloudWatch Agent Package on an Amazon EC2 Instance<a name="download-CloudWatch-Agent-on-EC2-Instance-fleet"></a>

You can download the CloudWatch agent package using either Systems Manager Run Command or an Amazon S3 download link\. For information about using an Amazon S3 download link, see [Download the CloudWatch Agent Package Using an S3 Download Link](download-cloudwatch-agent-commandline.md#download-CloudWatch-Agent-on-EC2-Instance-commandline-first)\.

### Download the CloudWatch Agent on an Amazon EC2 Instance Using Systems Manager<a name="download-CloudWatch-Agent-on-EC2-Instance-SSM-fleet"></a>

Before you can use Systems Manager to install the CloudWatch agent, you must make sure that the instance is configured correctly for Systems Manager\.

#### Installing or Updating SSM Agent<a name="update-SSM-Agent-EC2instance-fleet"></a>

On an Amazon EC2 instance, the CloudWatch agent requires that the instance is running version 2\.2\.93\.0 or later\. Before you install the CloudWatch agent, update or install SSM Agent on the instance if you haven't already done so\. 

For information about installing or updating SSM Agent on an instance running Linux, see [Installing and Configuring the SSM Agent on Linux Instances](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html) in the *AWS Systems Manager User Guide*\.

For information about installing or updating SSM Agent, see [Working with SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) in the *AWS Systems Manager User Guide*\.

#### \(Optional\) Verify Systems Manager Prerequisites<a name="install-CloudWatch-Agent-minimum-requirements-fleet"></a>

Before you use Systems Manager Run Command to install and configure the CloudWatch agent, verify that your instances meet the minimum Systems Manager requirements\. For more information, see [Systems Manager Prerequisites](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) in the *AWS Systems Manager User Guide*\.

#### Verify Internet Access<a name="install-CloudWatch-Agent-internet-access-fleet"></a>

Your Amazon EC2 instances must have outbound internet access in order to send data to CloudWatch or CloudWatch Logs\. For more information about how to configure internet access, see [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) in the *Amazon VPC User Guide*\.

#### Download the CloudWatch Agent Package<a name="install-CloudWatch-Agent-EC2-fleet"></a>

Systems Manager Run Command enables you to manage the configuration of your instances\. You specify a Systems Manager document, specify parameters, and execute the command on one or more instances\. SSM Agent on the instance processes the command and configures the instance as specified\.

**To download the CloudWatch agent using Run Command**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In the **Targets** area, choose the instance on which to install the CloudWatch agent\. If you do not see a specific instance, it might not be configured for Run Command\. For more information, see [Systems Manager Prerequisites](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-prereqs.html) in the *AWS Systems Manager User Guide*\.

1. In the **Action** list, choose **Install**\.

1. In the **Name** box, enter *AmazonCloudWatchAgent*\.

1. Keep **Version** set to **latest** to install the latest version of the agent\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully installed\. 

## \(Optional\) Modify the Common Configuration and Named Profile for CloudWatch Agent<a name="CloudWatch-Agent-profile-instance-fleet"></a>

The CloudWatch agent includes a configuration file called `common-config.toml`\. You can use this file optionally specify proxy and Region information\.

On a server running Linux, this file is in the `/opt/aws/amazon-cloudwatch-agent/etc` directory\. On a server running Windows Server, this file is in the `C:\ProgramData\Amazon\AmazonCloudWatchAgent` directory\.

The default `common-config.toml` is as follows:

```
# This common-config is used to configure items used for both ssm and cloudwatch access
 
 
## Configuration for shared credential.
## Default credential strategy will be used if it is absent here:
##            Instance role is used for EC2 case by default.
##            AmazonCloudWatchAgent profile is used for onPremise case by default.
# [credentials]
#    shared_credential_profile = "{profile_name}"
#    shared_credential_file= "{file_name}"
 
## Configuration for proxy.
## System-wide environment-variable will be read if it is absent here.
## i.e. HTTP_PROXY/http_proxy; HTTPS_PROXY/https_proxy; NO_PROXY/no_proxy
## Note: system-wide environment-variable is not accessible when using ssm run-command.
## Absent in both here and environment-variable means no proxy will be used.
# [proxy]
#    http_proxy = "{http_url}"
#    https_proxy = "{https_url}"
#    no_proxy = "{domain}"
```

All lines are commented out initially\. To set the credential profile or proxy settings, remove the `#` from that line and specify a value\. You can edit this file manually, or by using the `RunShellScript` Run Command in Systems Manager:
+ `shared_credential_profile` – For on\-premises servers, this line specifies the IAM user credential profile to use to send data to CloudWatch\. If you keep this line commented out, `AmazonCloudWatchAgent` is used\. 

  On an EC2 instance, you can use this line to have the CloudWatch agent send data from this instance to CloudWatch in a different AWS Region\. To do so, specify a named profile that includes a `region` field specifying the name of the Region to send to\.

  If you specify a `shared_credential_profile`, you must also remove the `#` from the beginning of the `[credentials]` line\.
+ `shared_credential_file` – To have the agent look for credentials in a file located in a path other than the default path, specify that complete path and file name here\. The default path is `/root/.aws` on Linux and is `C:\\Users\\Administrator\\.aws` on Windows Server\.

  The first example below shows the syntax of a valid `shared_credential_file` line for Linux servers, and the second example is valid for Windows Server\. On Windows Server, you must escape the \\ characters\.

  ```
  shared_credential_file= "C:\\Documents and Settings\\username\\.aws\\credentials"
  ```

  ```
  shared_credential_file= "/usr/username/credentials"
  ```

  If you specify a `shared_credential_file`, you must also remove the `#` from the beginning of the `[credentials]` line\.
+ Proxy settings – If your servers use HTTP or HTTPS proxies to contact AWS services, specify those proxies in the `http_proxy` and `https_proxy` fields\. If there are URLs that should be excluded from proxying, specify them in the `no_proxy` field, separated by commas\.

## Start the CloudWatch Agent<a name="start-CloudWatch-Agent-EC2-fleet"></a>

You can start the agent using Systems Manager Run Command or the command line\.

### Start the CloudWatch Agent Using Systems Manager Run Command<a name="start-CloudWatch-Agent-EC2-SSM-fleet"></a>

Follow these steps to start the agent using Systems Manager Run Command\.

**To start the CloudWatch agent using Run Command**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AmazonCloudWatch\-ManageAgent**\.

1. In the **Targets** area, choose the instance where you installed the CloudWatch agent\.

1. In the **Action** list, choose **configure**\.

1. In the **Optional Configuration Source** list, choose **ssm**\.

1. In the **Optional Configuration Location** box, enter the name of the agent configuration file that you created and saved to Systems Manager Parameter Store, as explained in [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

1. In the **Optional Restart** list, choose **yes** to start the agent after you have finished these steps\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully started\. 

### Start the CloudWatch Agent on an Amazon EC2 Instance Using the Command Line<a name="start-CloudWatch-Agent-EC2-commands-fleet"></a>

Follow these steps to use the command line to install the CloudWatch agent on an Amazon EC2 instance\.

**To use the command line to start the CloudWatch agent on an Amazon EC2 instance**
+ In this command, `-a fetch-config` causes the agent to load the latest version of the CloudWatch agent configuration file, and `-s` starts the agent\.

  Linux: If you saved the configuration file in the Systems Manager Parameter Store, enter the following:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:configuration-parameter-store-name -s
  ```

  Linux: If you saved the configuration file on the local computer, enter the following:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:configuration-file-path -s
  ```

  Windows Server: If you saved the agent configuration file in Systems Manager Parameter Store, enter the following from the PowerShell console:

  ```
  ./amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c ssm:configuration-parameter-store-name -s
  ```

  Windows Server: If you saved the agent configuration file on the local computer, enter the following from the PowerShell console:

  ```
  ./amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c file:configuration-file-path -s
  ```