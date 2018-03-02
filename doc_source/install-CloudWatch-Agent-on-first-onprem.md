# Getting Started: Installing the CloudWatch Agent on Your First Server<a name="install-CloudWatch-Agent-on-first-onprem"></a>

To download and install the CloudWatch agent on an on\-premises server, you can use either AWS Systems Manager or the command line\.

With either method, you must first create an IAM user with permissions to write to CloudWatch\.


+ [Download the CloudWatch Agent on an On\-Premises Server](#download-CloudWatch-Agent-first)
+ [Modify the Common Configuration and Named Profile for CloudWatch Agent](#CloudWatch-Agent-profile-first)
+ [Create the CloudWatch Agent Configuration File](#CloudWatch-Agent-configuration-file-onpremises-first)
+ [Start the CloudWatch Agent](#start-CloudWatch-Agent-on-premise-SSM-first)
+ [Verify the Signature of the CloudWatch Agent Package](verify-CloudWatch-Agent-Package-Signature.md)

## Download the CloudWatch Agent on an On\-Premises Server<a name="download-CloudWatch-Agent-first"></a>

To download the CloudWatch agent, you can use Systems Manager or the command line\.

To use Systems Manager Run Command, you must register your on\-premises server with Amazon EC2 Systems Manager\. For more information, see [ Setting Up Systems Manager in Hybrid Environments](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html) in the *AWS Systems Manager User Guide*\.

If you have already registered your server, update your SSM Agent to the latest version\.

For information about updating the SSM Agent on a server running Linux, see [ Install the SSM Agent on Servers and VMs in Your Linux Hybrid Environment](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html#sysman-install-managed-linux) in the *AWS Systems Manager User Guide*\.

For information about updating the SSM Agent on a server running Windows Server, see [ Install the SSM Agent on Servers and VMs in Your Windows Hybrid Environment](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html#sysman-install-managed-win) in the *AWS Systems Manager User Guide*\.

**To use the SSM Agent to download the CloudWatch agent package on an on\-premises server**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, select the button next to **AWS\-ConfigureAWSPackage**\.

1. In the **Targets** area, select the server on which to install the CloudWatch agent\. If you do not see a specific server, it might not be configured for Run Command\. For more information, see [Systems Manager Prerequisites](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/systems-manager-setting-up.html) in the *Amazon EC2 User Guide for Windows Instances*\.

1. In the **Action** list, choose **Install**\.

1. In the **Name** box, type **AmazonCloudWatchAgent**\.

1. Leave **Version** blank to install the latest version of the agent\.

1. Choose **Run**\.

   The agent package is downloaded, and the next steps are to configure and start it\.

**To use the command line to download the CloudWatch agent on an on\-premises server**

1. Make a directory for downloading and unzipping the agent package\. For example, `tmp/AmazonCloudWatchAgent`\. Then change into that directory\.

1. Download the CloudWatch agent package\. For a Linux server, type the following:

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

## Modify the Common Configuration and Named Profile for CloudWatch Agent<a name="CloudWatch-Agent-profile-first"></a>

The CloudWatch agent package you have downloaded includes a configuration file called `common-config.toml`\. You can use this file to specify proxy, credential, and region information\. On a server running Linux, this file is in the `/opt/aws/amazon-cloudwatch-agent/etc` directory\. On a server running Windows Server, this file is in the `C:\ProgramData\Amazon\AmazonCloudWatchAgent` directory\.

The default `common-config.toml` is as follows:

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

+ **shared\_credential\_profile** You can specify a name for the named profile that CloudWatch agent is to look for in the AWS credentials and AWS config files\. If you don't specify a name here, the CloudWatch agent looks for the default profile name, `AmazonCloudWatchAgent`\.

+ **proxy settings** If your servers use HTTP or HTTPS proxies to contact AWS services, specify those proxies in the `http_proxy` and `https_proxy` fields\. If there are URLs that should be excluded from proxying, specify them in the `no_proxy` field, separated by commas\.

After modifying `common-config.toml`, you should make sure the profile name that you specified, or the default profile name of `AmazonCloudWatchAgent`, exists in the root user's AWS credentials and config files\. This profile is used for credential and region information during CloudWatch agent setup\. Following is an example of this profile in the credentials file:

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

The named profile in the credentials file contains the credentials to be used for CloudWatch agent\. These credentials are used for permissions to write metric data to CloudWatch, and to download information from Systems Manager Parameter Store during CloudWatch agent installation\. You can obtain the credentials to use for this section by creating an IAM user for the CloudWatch agent, as explained previously in this section\.

The named profile in the configuration file specifies the region to which the CloudWatch metrics are published, when the CloudWatch agent runs on an on\-premises server\. When you use the `aws configure` command to modify the profile, do so as the root or administrator\.

Following is an example of using the `aws configure` command to create a named profile for the CloudWatch agent\. This example assumes that you are using the default profile name of `AmazonCloudWatchAgent`\.

**To create the AmazonCloudWatchAgent profile for the CloudWatch agent**

+ Type the following command and follow the prompts:

  ```
  aws configure --profile AmazonCloudWatchAgent
  ```

## Create the CloudWatch Agent Configuration File<a name="CloudWatch-Agent-configuration-file-onpremises-first"></a>

CloudWatch agent uses a configuration file to specify the metrics to be collected and other agent configuration data\. You must customize this file before you start the agent\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

## Start the CloudWatch Agent<a name="start-CloudWatch-Agent-on-premise-SSM-first"></a>

You can start the CloudWatch agent using either Systems Manager Run Command or the command line\.

**To use the SSM Agent to start the CloudWatch agent on an on\-premises server**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, select the button next to **AmazonCloudWatch\-ManageAgent**\.

1. In the **Targets** area, select the instance where you installed the agent\.

1. In the **Action** list, choose **configure**\.

1. In the **Mode** list, choose **onPremise**\.

1. In the **Optional Configuration Location** box, type the name of the agent configuration file that you created with the wizard and stored in Parameter Store\.

1. Choose **Run**\.

   The agent starts with the configuration that you specified in the configuration file\.

**To use the command line to start the CloudWatch agent on an on\-premises server**

+ On a Linux server, type the following if you saved the agent configuration file in Parameter Store:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -c ssm:configuration-parameter-store-name -s
  ```

  On a Linux server, type the following if you saved the agent configuration file on the local computer:

  ```
  sudo /opt/aws/amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -c file:configuration-file-path -s
  ```

  On a server running Windows Server, type the following if you saved the agent configuration file in Parameter Store:

  ```
  amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m onPremise -c ssm:configuration-parameter-store-name -s
  ```

  On a server running Windows Server, type the following if you saved the agent configuration file on the local computer:

  ```
  amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m onPremise -c file:configuration-file-path -s
  ```