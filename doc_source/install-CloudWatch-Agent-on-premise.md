# Installing the CloudWatch Agent on On\-Premises Servers<a name="install-CloudWatch-Agent-on-premise"></a>

If you have downloaded the CloudWatch agent on one computer and created the agent configuration file you want, you can use that configuration file to install the agent on other on\-premises servers\. 

## Download the CloudWatch Agent on an On\-Premises Server<a name="download-CloudWatch-Agent-onprem"></a>

You can download the CloudWatch agent package using either Systems Manager Run Command or an Amazon S3 download link\. For information about using an Amazon S3 download link, see [Download the CloudWatch Agent Package Using an S3 Download Link](download-cloudwatch-agent-commandline.md#download-CloudWatch-Agent-on-EC2-Instance-commandline-first)\.

### Download Using Systems Manager<a name="download-CloudWatch-Agent-onprem-fleet-sys"></a>

To use Systems Manager Run Command, you must register your on\-premises server with Amazon EC2 Systems Manager\. For more information, see [ Setting Up Systems Manager in Hybrid Environments](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html) in the *AWS Systems Manager User Guide*\.

If you have already registered your server, update SSM Agent to the latest version\.

For information about updating SSM Agent on a server running Linux, see [Install SSM Agent for a Hybrid Environment \(Linux\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html#sysman-install-managed-linux) in the *AWS Systems Manager User Guide*\.

For information about updating the SSM Agent on a server running Windows Server, see [ Install SSM Agent for a Hybrid Environment \(Windows\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html#sysman-install-managed-win) in the *AWS Systems Manager User Guide*\.

**To use the SSM Agent to download the CloudWatch agent package on an on\-premises server**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, select the button next to **AWS\-ConfigureAWSPackage**\.

1. In the **Targets** area, select the server to install the CloudWatch agent on\. If you don't see a specific server, it might not be configured for Run Command\. For more information, see [Systems Manager Prerequisites](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-prereqs.html) in the *AWS Systems Manager User Guide*\.

1. In the **Action** list, choose **Install**\.

1. In the **Name** box, enter *AmazonCloudWatchAgent*\.

1. Keep **Version** blank to install the latest version of the agent\.

1. Choose **Run**\.

   The agent package is downloaded, and the next steps are to configure and start it\.

## \(Installing on an On\-Premises Server\) Specify IAM Credentials and AWS Region<a name="install-CloudWatch-Agent-iam_user-SSM-onprem"></a>

To enable the CloudWatch agent to send data from an on\-premises server, you must specify the access key and secret key of the IAM user that you created earlier\. For more information about creating this user, see [Create IAM Roles and Users for Use with the CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)\.

You also must specify the AWS Region to send the metrics to, using the `region` field\.

Following is an example of this file\.

```
[AmazonCloudWatchAgent]
aws_access_key_id=my_access_key
aws_secret_access_key=my_secret_key
region = us-west-1
```

For *my\_access\_key* and *my\_secret\_key*, use the keys from the IAM user that doesn't have the permissions to write to Systems Manager Parameter Store\. For more information about the IAM users needed for CloudWatch agent, see [Create IAM Users to Use with the CloudWatch Agent on On\-Premises Servers](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-users)\.

If you name this profile `AmazonCloudWatchAgent`, you don't need to do anything more\. Optionally, you can give it a different name and specify that name as the value for `shared_credential_profile` in the` common-config.toml` file, which is explained in the following section\.

Following is an example of using the aws configure command to create a named profile for the CloudWatch agent\. This example assumes that you're using the default profile name of `AmazonCloudWatchAgent`\.

**To create the AmazonCloudWatchAgent profile for the CloudWatch agent**
+ On Linux servers, enter the following command and follow the prompts:

  ```
  sudo aws configure --profile AmazonCloudWatchAgent
  ```

  On Windows Server, open PowerShell as an administrator, enter the following command, and follow the prompts\.

  ```
  aws configure --profile AmazonCloudWatchAgent
  ```

## \(Optional\) Modifying the Common Configuration and Named Profile for CloudWatch Agent<a name="CloudWatch-Agent-profile-onprem"></a>

The CloudWatch agent includes a configuration file called `common-config.toml`\. You can optionally use this file to specify proxy and Region information\.

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
+ `shared_credential_profile` – For on\-premises servers, this line specifies the IAM user credential profile to use to send data to CloudWatch\. If you keep this line commented out, `AmazonCloudWatchAgent` is used\. For more information about creating this profile, see [\(Installing on an On\-Premises Server\) Specify IAM Credentials and AWS Region](#install-CloudWatch-Agent-iam_user-SSM-onprem)\. 

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

## Starting the CloudWatch Agent<a name="start-CloudWatch-Agent-on-premise-SSM-onprem"></a>

You can start the CloudWatch agent using either Systems Manager Run Command or the command line\.

**To use SSM Agent to start the CloudWatch agent on an on\-premises server**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, select the button next to **AmazonCloudWatch\-ManageAgent**\.

1. In the **Targets** area, select the instance where you installed the agent\.

1. In the **Action** list, choose **configure**\.

1. In the **Mode** list, choose **onPremise**\.

1. In the **Optional Configuration Location** box, enter the name of the agent configuration file that you created with the wizard and stored in the Parameter Store\.

1. Choose **Run**\.

   The agent starts with the configuration you specified in the configuration file\.

**To use the command line to start the CloudWatch agent on an on\-premises server**
+ In this command, `-a fetch-config` causes the agent to load the latest version of the CloudWatch agent configuration file, and `-s` starts the agent\.

  Linux: If you saved the configuration file in the Systems Manager Parameter Store, enter the following:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -c ssm:configuration-parameter-store-name -s
  ```

  Linux: If you saved the configuration file on the local computer, enter the following:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -c file:configuration-file-path -s
  ```

  Windows Server: If you saved the agent configuration file in Systems Manager Parameter Store, enter the following from the PowerShell console:

  ```
  ./amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m onPremise -c ssm:configuration-parameter-store-name -s
  ```

  Windows Server: If you saved the agent configuration file on the local computer, enter the following from the PowerShell console:

  ```
  ./amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m onPremise -c file:configuration-file-path -s
  ```