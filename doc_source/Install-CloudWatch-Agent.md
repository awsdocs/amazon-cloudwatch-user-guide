# Collect Metrics and Logs from Amazon EC2 Instances and On\-Premises Servers with the CloudWatch Agent<a name="Install-CloudWatch-Agent"></a>

The unified CloudWatch agent enables you to do the following:

+ Collect more system\-level metrics from Amazon EC2 instances, including in\-guest metrics, in addition to the metrics listed in [Amazon EC2 Metrics and Dimensions](ec2-metricscollected.md)\. The additional metrics are listed in [Metrics Collected by the CloudWatch Agent](CW_Support_For_AWS.md#metrics-collected-by-CloudWatch-agent)\.

+ Collect system\-level metrics from on\-premises servers\. These can include servers in a hybrid environment as well as servers not managed by AWS\.

+ Collect logs from Amazon EC2 instances and on\-premises servers, running either Linux or Windows Server\.

You can store and view the metrics you collect with the CloudWatch agent in CloudWatch just as you can with any other CloudWatch metrics\. The default namespace for metrics collected by the CloudWatch agent is `CWAgent`, although you can specify a different namespace when you configure the agent\.

The logs collected by the unified CloudWatch agent are processed and stored in CloudWatch Logs, just like logs collected by the older CloudWatch Logs agent\. For information about CloudWatch Logs pricing, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

Metrics collected by the CloudWatch agent are billed as custom metrics\. For more information about CloudWatch metrics pricing, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

The steps in this section explain how to install the unified CloudWatch agent on Amazon EC2 instances and on\-premises servers\. For more information about the metrics that can be collected by the CloudWatch agent, see [Metrics Collected by the CloudWatch Agent](CW_Support_For_AWS.md#metrics-collected-by-CloudWatch-agent)\.

**Supported Operating Systems**

The CloudWatch agent is supported on the following operating systems:

+ Amazon Linux version 2014\.03\.02 or later

+ Ubuntu Server version 16\.04 and 14\.04

+ CentOS version 7\.0 and 6\.5

+ Red Hat Enterprise Linux \(RHEL\) version 7\.4, 7\.0, and 6\.5

+ Debian 8\.0

+ 64\-bit versions of Windows Server 2016, Windows Server 2012, and Windows Server 2008\. 

**Installation Process Overview**

The general flow of installing the CloudWatch agent is as follows:

1. Create the IAM roles and users that you need for the CloudWatch agent\. They enable CloudWatch to collect metrics from the server, and to integrate with AWS Systems Manager\.

1. If you are installing on an Amazon EC2 instance, attach an IAM role to the instance\. If you are installing on an on\-premises server, create an IAM user for CloudWatch agent\. 

1. Download the agent package, using either AWS Systems Manager Run Command or a public Amazon S3 download link\.

1. Modify the CloudWatch agent configuration files, and create a named profile for CloudWatch agent\. Creating the named profile is optional when installing the agent on an Amazon EC2 instance\.

1. Start the agent, using either Systems Manager Run Command or the command line\.


+ [Create IAM Roles and Users for Use With CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)
+ [Install the CloudWatch Agent on an Amazon EC2 Instance](install-CloudWatch-Agent-on-EC2-Instance.md)
+ [Install the CloudWatch Agent on an On\-Premises Server](install-CloudWatch-Agent-on-premise.md)
+ [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)
+ [Common Scenarios with CloudWatch Agent](CloudWatch-Agent-common-scenarios.md)
+ [Troubleshooting the CloudWatch Agent](troubleshooting-CloudWatch-Agent.md)