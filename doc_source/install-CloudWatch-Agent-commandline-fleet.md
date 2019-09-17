# Installing and Running the CloudWatch Agent on Your Servers<a name="install-CloudWatch-Agent-commandline-fleet"></a>

After you have created the agent configuration file that you want and created an IAM role or IAM user, use the following steps to install and run the agent on your servers, using that configuration\. First, attach an IAM role or IAM user to the server that will run the agent\. Then, on that server, download the agent package and start it using the agent configuration you created\. 

## Download the CloudWatch Agent Package Using an S3 Download Link<a name="download-CloudWatch-Agent-on-EC2-Instance-commandline-fleet"></a>

On each server where you will run the agent, download the agent package\. Choose the download link from this table, depending on your architecture and platform\.

For each download link, there is a general link as well as links for each Region\. For example, for Amazon Linux and Amazon Linux 2 and the AMD64 architecture, three of the valid download links are:
+ `https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`
+ `https://s3.us-east-1.amazonaws.com/amazoncloudwatch-agent-us-east-1/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`
+ `https://s3.eu-central-1.amazonaws.com/amazoncloudwatch-agent-eu-central-1/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`


| Architecture | Platform | Download Link | Signature File Link | 
| --- | --- | --- | --- | 
|  AMD64 |  Amazon Linux and Amazon Linux 2  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  Centos  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  Redhat  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  SUSE  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  Debian  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  AMD64 |  Ubuntu  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  AMD64 |  Windows  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi  |   https://s3\.amazonaws\.com/amazoncloudwatch\-agent/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi\.sig  https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi\.sig  | 
|  ARM64 |  Amazon Linux 2  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  ARM64 |  Redhat  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  ARM64 |  Ubuntu  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  ARM64 |  SUSE  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 

**To use the command line to install the CloudWatch agent on an Amazon EC2 instance**

1. Download the CloudWatch agent\. For a Linux server, enter the following\. For *download\-link*, use the appropriate download link from the previous table\.

   ```
   wget download-link
   ```

   For a server running Windows Server, download the following file:

   ```
   https://s3.amazonaws.com/amazoncloudwatch-agent/windows/amd64/latest/amazon-cloudwatch-agent.msi
   ```

1. After you have downloaded the package, you can optionally verify the package signature\. For more information, see [Verifying the Signature of the CloudWatch Agent Package](verify-CloudWatch-Agent-Package-Signature.md)\.

1. Install the package\. If you downloaded an RPM package on a Linux server, change to the directory containing the package and enter the following:

   ```
   sudo rpm -U ./amazon-cloudwatch-agent.rpm
   ```

   If you downloaded a DEB package on a Linux server, change to the directory containing the package and enter the following:

   ```
   sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
   ```

   If you downloaded an MSI package on a server running Windows Server, change to the directory containing the package and enter the following:

   ```
   msiexec /i amazon-cloudwatch-agent.msi
   ```

   This command also works from within PowerShell\. For more information about MSI command options, see [Command\-Line Options](https://docs.microsoft.com/en-us/windows/desktop/Msi/command-line-options) in the Microsoft Windows documentation\.

## \(Installing on an EC2 Instance\) Attaching an IAM Role<a name="install-CloudWatch-Agent-iam_permissions-first"></a>

To enable the CloudWatch agent to send data from the instance, you must attach an IAM role to the instance\. The role to attach is **CloudWatchAgentServerRole**\.

For more information on attaching an IAM role to an instance, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Windows Instances*\.

## \(Installing on an On\-Premises Server\) Specify IAM Credentials and AWS Region<a name="install-CloudWatch-Agent-iam_user-first"></a>

To enable the CloudWatch agent to send data from an on\-premises server, you must specify the access key and secret key of the IAM user that you created earlier\. For more information about creating this user, see [Create IAM Roles and Users for Use With CloudWatch Agent](create-iam-roles-for-cloudwatch-agent-commandline.md)\.

You also must specify the AWS Region to send the metrics to, using the `region` field in the `[AmazonCloudWatchAgent]` section of the AWS config file, as in the following example\.

```
[profile AmazonCloudWatchAgent]
region = us-west-1
```

The following is an example of using the `aws configure` command to create a named profile for the CloudWatch agent\. This example assumes that you are using the default profile name of `AmazonCloudWatchAgent`\.

**To create the AmazonCloudWatchAgent profile for the CloudWatch agent**
+ On Linux servers, enter the following command and follow the prompts:

  ```
  sudo aws configure --profile AmazonCloudWatchAgent
  ```

  On Windows Server, open PowerShell as an administrator, enter the following command, and follow the prompts\.

  ```
  aws configure --profile AmazonCloudWatchAgent
  ```

## \(Optional\) Modify the Common Configuration for Proxy or Region Information<a name="CloudWatch-Agent-profile-instance-first"></a>

The CloudWatch agent includes a configuration file called `common-config.toml`\. You can optionally use this file to specify proxy and Region information\.

On a server running Linux, this file is in the `/opt/aws/amazon-cloudwatch-agent/etc` directory\. On a server running Windows Server, this file is in the `C:\ProgramData\Amazon\AmazonCloudWatchAgent` directory\.

The default `common-config.toml` is as follows\.

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

All lines are commented out initially\. To set the credential profile or proxy settings, remove the `#` from that line and specify a value\. You can edit this file manually or by using the `RunShellScript` Run Command in Systems Manager:
+ `shared_credential_profile` – For on\-premises servers, this line specifies the IAM user credential profile to use to send data to CloudWatch\. If you keep this line commented out, `AmazonCloudWatchAgent` is used\. For more information about creating this profile, see [\(Installing on an On\-Premises Server\) Specify IAM Credentials and AWS Region](#install-CloudWatch-Agent-iam_user-first)\. 

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

## Start the CloudWatch Agent Using the Command Line<a name="start-CloudWatch-Agent-EC2-commands-fleet"></a>

Follow these steps to use the command line to start the CloudWatch agent on a server\.

**To use the command line to start the CloudWatch agent on a server**

1. Copy the agent configuration file that you want to use to the server where you're going to run the agent\. Note the pathname where you copy it to\.

1. In this command, `-a fetch-config` causes the agent to load the latest version of the CloudWatch agent configuration file, and `-s` starts the agent\.

   On an EC2 instance running Linux, enter the following:

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:configuration-file-path -s
   ```

   On an on\-premises server running Linux, enter the following:

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m onPremise -c file:configuration-file-path -s
   ```

   On an EC2 instance running Windows Server, enter the following from the PowerShell console:

   ```
   ./amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c file:configuration-file-path -s
   ```

   On an on\-premises server running Windows Server, enter the following from the PowerShell console:

   ```
   ./amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m onPremise -c file:configuration-file-path -s
   ```