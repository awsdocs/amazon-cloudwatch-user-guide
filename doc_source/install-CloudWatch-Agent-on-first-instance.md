# Getting Started: Installing the CloudWatch Agent on Your First Instance<a name="install-CloudWatch-Agent-on-first-instance"></a>

To download and install the CloudWatch agent on a running Amazon EC2 instance, you can use either AWS Systems Manager or the command line\. With either method, you must first create an IAM role and attach it to the instance\.


+ [Attach an IAM Role to the Instance](#install-CloudWatch-Agent-iam_permissions-first)
+ [Download the CloudWatch Agent Package on an Amazon EC2 Instance](#download-CloudWatch-Agent-on-EC2-Instance-first)
+ [\(Optional\) Modify the Common Configuration and Named Profile for CloudWatch Agent](#CloudWatch-Agent-profile-instance-first)
+ [Create the Agent Configuration File on Your First Instance](#CW-Agent-Instance-Create-Configuration-File-first)
+ [Start the CloudWatch Agent](#start-CloudWatch-Agent-EC2)

## Attach an IAM Role to the Instance<a name="install-CloudWatch-Agent-iam_permissions-first"></a>

An IAM role for the instance profile is required when you install the CloudWatch agent on an Amazon EC2 instance\. This role enables the CloudWatch agent to perform actions on the instance\. Use one of the roles you created earlier\. For more information about creating these roles, see [Create IAM Roles and Users for Use With CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)\. You can scroll through the list to find them, or use the search box\.

If you are going to use this instance to create the CloudWatch agent configuration file and copy it to Systems Manager Parameter Store, use the role you created that has permissions to write to Parameter Store\. This role may be called **CloudWatchAgentAdminRole**\.

For all other instances, select the role that includes just the permissions needed to install and run the agent\. This role may be called **CloudWatchAgentServerRole**\.

Attach this role to the instance on which you install the CloudWatch agent\. For more information, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Windows Instances*\.

## Download the CloudWatch Agent Package on an Amazon EC2 Instance<a name="download-CloudWatch-Agent-on-EC2-Instance-first"></a>

You can download the CloudWatch agent package using either Systems Manager Run Command or an Amazon S3 download link\.

### Download the CloudWatch Agent on an Amazon EC2 Instance Using AWS Systems Manager<a name="download-CloudWatch-Agent-on-EC2-Instance-SSM-first"></a>

Before you can use Systems Manager to install the CloudWatch agent, you must make sure that the instance is configured correctly for Systems Manager\.

#### Install or Update the SSM Agent<a name="update-SSM-Agent-EC2instance-first"></a>

On an Amazon EC2 instance, the CloudWatch agent requires that the instance is running version 2\.2\.93\.0 or later\. Before you install the CloudWatch agent, update or install the SSM Agent on the instance if you haven't already done so\. 

For information about installing or updating the SSM Agent on an instance running Linux, see [ Installing and Configuring the SSM Agent on Linux Instances](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html) in the *AWS Systems Manager User Guide*\.

For information about installing or updating the SSM Agent, see [ Installing and Configuring the SSM Agent](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html) in the *AWS Systems Manager User Guide*\.

#### \(Optional\) Verify Systems Manager Prerequisites<a name="install-CloudWatch-Agent-minimum-requirements-first"></a>

Before you use Systems Manager Run Command to install and configure the CloudWatch agent, verify that your instances meet the minimum Systems Manager requirements\. For more information, see [Systems Manager Prerequisites](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html#systems-manager-prereqs) in the *AWS Systems Manager User Guide*\.

#### Verify Internet Access<a name="install-CloudWatch-Agent-internet-access-first"></a>

Your Amazon EC2 instances must have outbound internet access in order to send data to CloudWatch or CloudWatch Logs\. For more information about how to configure internet access, see [Internet Gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html) in the *Amazon VPC User Guide*\.

The endpoints and ports to configure on your proxy are as follows:

+ If you are using the agent to collect metrics, you must whitelist the CloudWatch endpoints for the appropriate regions\. These endpoints are listed in [Amazon CloudWatch](http://docs.aws.amazon.com/general/latest/gr/rande.html#cw_region) in the *Amazon Web Services General Reference*\. 

+ If you are using the agent to collect logs, you must whitelist the CloudWatch Logs endpoints for the appropriate regions\. These endpoints are listed in [Amazon CloudWatch Logs](http://docs.aws.amazon.com/general/latest/gr/rande.html#cwl_region) in the *Amazon Web Services General Reference*\. 

+ If you are using SSM to install the agent or Parameter Store to store your configuration file, you must whitelist the SSM endpoints for the appropriate regions\. These endpoints are listed in [AWS Systems Manager](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *Amazon Web Services General Reference*\. 

### Download the CloudWatch Agent Package Using Run Command<a name="install-CloudWatch-Agent-EC2-first"></a>

Systems Manager Run Command enables you to manage the configuration of your instances\. You specify a Systems Manager document, specify parameters, and execute the command on one or more instances\. The SSM Agent on the instance processes the command and configures the instance as specified\.

**To download the CloudWatch agent using Run Command**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In the **Targets** area, choose the instance on which to install the CloudWatch agent\. If you do not see a specific instance, it might not be configured for Run Command\. For more information, see [Systems Manager Prerequisites](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/systems-manager-setting-up.html) in the *Amazon EC2 User Guide for Windows Instances*\.

1. In the **Action** list, choose **Install**\.

1. In the **Name** field, type **AmazonCloudWatchAgent**\.

1. Leave **Version** set to **latest** to install the latest version of the agent\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully installed\.

### Download the CloudWatch Agent Package on an Amazon EC2 Instance Using a Download Link<a name="download-CloudWatch-Agent-on-EC2-Instance-commandline-first"></a>

You can use an Amazon S3 download link to download the CloudWatch agent package on an Amazon EC2 instance server\.

**To use the command line to install the CloudWatch agent on an Amazon EC2 instance**

1. Make a directory for downloading and unzipping the agent package\. For example, `tmp/AmazonCloudWatchAgent`\. Then change into that directory\.

1. Download the CloudWatch agent\. For a Linux server, type the following:

   ```
   wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip
   ```

   For a server running Windows Server, download the following file:

   ```
   https://s3.amazonaws.com/amazoncloudwatch-agent/windows/amd64/latest/AmazonCloudWatchAgent.zip
   ```

1. After you have downloaded the package, you can optionally use a GPG signature file to verify the package signature\. For more information, see [Verify the Signature of the CloudWatch Agent Package](verify-CloudWatch-Agent-Package-Signature.md)\.

1. Unzip the package\.

   ```
   unzip AmazonCloudWatchAgent.zip
   ```

1. Install the package\. On a Linux server, change to the directory containing the package and type:

   ```
   sudo ./install.sh
   ```

   On a server running Windows Server, open PowerShell, change to the directory containing the unzipped package, and use the `install.ps1` script to install it\.

## \(Optional\) Modify the Common Configuration and Named Profile for CloudWatch Agent<a name="CloudWatch-Agent-profile-instance-first"></a>

The CloudWatch agent package you have downloaded includes a configuration file called `common-config.toml`\. You can use this file to specify proxy, credential, and region information\. On a server running Linux, this file is in the `/opt/aws/amazon-cloudwatch-agent/etc` directory\. On a server running Windows Server, this file is in the `C:\ProgramData\Amazon\AmazonCloudWatchAgent` directory\.

The default `common-config.toml` is as follows:

When you install the CloudWatch agent on an Amazon EC2 instance, you need to modify this file only if you need to specify proxy settings or if the agent should send metrics to CloudWatch in a different region than where the instance is located\.

```
# This common-config is used to configure items used for both ssm and cloudwatch access
 
 
## Configuration for shared credential.
## Default credential strategy will be used if it is absent here:
##            Instance role is used for EC2 case by default.
##            AmazonCloudWatchAgent profile is used for onPremise case by default.
# [credentials]
#    shared_credential_profile = "{profile_name}"
 
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

+ **shared\_credential\_profile** To have the CloudWatch agent send metrics to CloudWatch in the same region where the instance is located, you don't need to modify this line if you have attached an IAM role with the proper permissions to the instance, and you don't need to use the `aws configure` command to create a named profile for the agent\.

  Otherwise, you can use this line to specify the named profile that CloudWatch agent is to use in the AWS credentials and AWS config files\. If you do so, CloudWatch agent uses the credential and the region settings in that named profile\.

+ **proxy settings** If your servers use HTTP or HTTPS proxies to contact AWS services, specify those proxies in the `http_proxy` and `https_proxy` fields\. If there are URLs that should be excluded from proxying, specify them in the `no_proxy` field, separated by commas\.

After modifying `common-config.toml`, if you need to specify credential and region information for CloudWatch agent, create a named profile for the CloudWatch agent in the AWS credentials and AWS config files\. When you create this profile, do so as the root or administrator\. Following is an example of this profile in the credentials file:

```
[AmazonCloudWatchAgent]
aws_access_key_id=my_access_key
aws_secret_access_key=my_secret_key
```

For `my_access_key` and `my_secret_key`, use the keys from the IAM user that does not have the permissions to write to Systems Manager Parameter Store\. For more information about the IAM users needed for CloudWatch agent, see [Create IAM Users to Use with CloudWatch Agent on On\-premises Servers](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-users)\.

Following is an example of the profile for the configuration file:

```
[AmazonCloudWatchAgent]
region=us-west-1
```

Following is an example of using the `aws configure` command to create a named profile for the CloudWatch agent\. This example assumes that you are using the default profile name of `AmazonCloudWatchAgent`\.

**To create the AmazonCloudWatchAgent profile for the CloudWatch agent**

+ Type the following command and follow the prompts:

  ```
  aws configure --profile AmazonCloudWatchAgent
  ```

## Create the Agent Configuration File on Your First Instance<a name="CW-Agent-Instance-Create-Configuration-File-first"></a>

After you have downloaded the CloudWatch agent, you must create the configuration file before you start the agent on any servers\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

## Start the CloudWatch Agent<a name="start-CloudWatch-Agent-EC2"></a>

To start the agent on the same server where you created the agent configuration file, follow these steps\. To use this configuration file on other servers, see [ Installing CloudWatch Agent on Additional Instances Using Your Agent Configuration](install-CloudWatch-Agent-on-EC2-Instance-fleet.md)\.

### Start the CloudWatch Agent Using Run Command<a name="start-CloudWatch-Agent-EC2-SSM-first"></a>

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

1. In the **Optional Configuration Location** box, type the name of the agent configuration file that you created and saved to Systems Manager Parameter Store, as explained in [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

1. In the **Optional Restart** list, choose **yes** to start the agent after you have finished these steps\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully started\. 

### Start the CloudWatch Agent on an Amazon EC2 Instance Using the Command Line<a name="start-CloudWatch-Agent-EC2-commands-first"></a>

Follow these steps to use the command line to install the CloudWatch agent on an Amazon EC2 instance\.

**To use the command line to start the CloudWatch agent on an Amazon EC2 instance**

+ On a Linux server, type the following if you saved the configuration file in the Systems Manager Parameter Store:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:configuration-parameter-store-name -s
  ```

  On a Linux server, type the following if you saved the configuration file on the local computer:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:configuration-file-path -s
  ```

  On a server running Windows Server, type the following if you saved the configuration file in Systems Manager Parameter Store:

  ```
  amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c ssm:configuration-parameter-store-name -s
  ```

  On a server running Windows Server, type the following if you saved the configuration file on the local computer:

  ```
  amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c file:configuration-file-path -s
  ```