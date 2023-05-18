# Download and configure the CloudWatch agent using the command line<a name="download-cloudwatch-agent-commandline"></a>

Use the following steps to download the CloudWatch agent package, create IAM roles or users, and optionally modify the common configuration file\.

## Download the CloudWatch agent package<a name="download-CloudWatch-Agent-on-EC2-Instance-commandline-first"></a>

The CloudWatch agent is available as a package in Amazon Linux 2\. If you are using this operating system, you can install the package by entering the following command\. You must also make sure that the IAM role attached to the instance has the **CloudWatchAgentServerPolicy** attached\. For more information, see ﻿[Create IAM roles and users for use with CloudWatch agent](create-iam-roles-for-cloudwatch-agent-commandline.md)﻿\.

```
sudo yum install amazon-cloudwatch-agent
```

On all supported operating systems, you can download and install the CloudWatch agent using the command line\.

For each download link, there is a general link as well as links for each Region\. For example, for Amazon Linux and Amazon Linux 2 and the x86\-64 architecture, three of the valid download links are:
+ `https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`
+ `https://s3.us-east-1.amazonaws.com/amazoncloudwatch-agent-us-east-1/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`
+ `https://s3.eu-central-1.amazonaws.com/amazoncloudwatch-agent-eu-central-1/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm`

You can also download a README file about the latest changes to the agent, and a file that indicates the version number that is available for download\. These files are in the following locations:
+ `https://s3.amazonaws.com/amazoncloudwatch-agent/info/latest/RELEASE_NOTES` or `https://s3.region.amazonaws.com/amazoncloudwatch-agent-Region/info/latest/RELEASE_NOTES`
+ `https://s3.amazonaws.com/amazoncloudwatch-agent/info/latest/CWAGENT_VERSION` or `https://s3.region.amazonaws.com/amazoncloudwatch-agent-Region/info/latest/CWAGENT_VERSION`


| Architecture | Platform | Download link | Signature file link | 
| --- | --- | --- | --- | 
|  x86\-64 |  Amazon Linux and Amazon Linux 2  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  x86\-64 |  Centos  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  x86\-64 |  Redhat  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  x86\-64 |  SUSE  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  x86\-64 |  Debian  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  x86\-64 |  Ubuntu  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  x86\-64 |  Oracle  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  x86\-64 |  macOS  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg\.sig  | 
|  x86\-64 |  Windows  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi  |   https://s3\.amazonaws\.com/amazoncloudwatch\-agent/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi\.sig  https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi\.sig  | 
|  ARM64 |  Amazon Linux 2  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  ARM64 |  Redhat  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  ARM64 |  Ubuntu  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  ARM64 |  SUSE  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 

**To use the command line to download and install the CloudWatch agent package**

1. Download the CloudWatch agent\.

   On a Linux server, enter the following\. For *download\-link*, use the appropriate download link from the previous table\.

   ```
   wget download-link
   ```

   On a server running Windows Server, download the following file:

   ```
   https://s3.amazonaws.com/amazoncloudwatch-agent/windows/amd64/latest/amazon-cloudwatch-agent.msi
   ```

1. After you have downloaded the package, you can optionally verify the package signature\. For more information, see [Verifying the signature of the CloudWatch agent package](verify-CloudWatch-Agent-Package-Signature.md)\.

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

   If you downloaded a PKG package on a macOS server, change to the directory containing the package and enter the following:

   ```
   sudo installer -pkg ./amazon-cloudwatch-agent.pkg -target /
   ```

## Create and modify the agent configuration file<a name="CW-Agent-Instance-Create-Configuration-File-first"></a>

After you have downloaded the CloudWatch agent, you must create the configuration file before you start the agent on any servers\. For more information, see [Create the CloudWatch agent configuration file](create-cloudwatch-agent-configuration-file.md)\.