# Installing the CloudWatch agent<a name="install-CloudWatch-Agent-on-EC2-Instance"></a>

The CloudWatch agent is available as a package in Amazon Linux 2\. If you are using this operating system, you can install the package by entering the following command\. You must also make sure that the IAM role attached to the instance has the **CloudWatchAgentServerPolicy** attached\. For more information, see ﻿[Create IAM roles to use with the CloudWatch agent on Amazon EC2 instances](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-roles)﻿\.

```
sudo yum install amazon-cloudwatch-agent
```

On all supported operating systems, you can download and install the CloudWatch agent using either the command line with an Amazon S3 download link, using SSM, or using an AWS CloudFormation template\.

**Topics**
+ [Installing the CloudWatch agent using the command line](installing-cloudwatch-agent-commandline.md)
+ [Installing the CloudWatch agent using AWS Systems Manager](installing-cloudwatch-agent-ssm.md)
+ [Installing the CloudWatch agent on new instances using AWS CloudFormation](Install-CloudWatch-Agent-New-Instances-CloudFormation.md)
+ [Verifying the signature of the CloudWatch agent package](verify-CloudWatch-Agent-Package-Signature.md)