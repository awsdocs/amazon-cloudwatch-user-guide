# Collecting Metrics and Logs from Amazon EC2 Instances and On\-Premises Servers with the CloudWatch Agent<a name="Install-CloudWatch-Agent"></a>

The unified CloudWatch agent enables you to do the following:
+ Collect more system\-level metrics from Amazon EC2 instances across operating systems\. The metrics can include in\-guest metrics, in addition to the metrics for EC2 instances\. The additional metrics that can be collected are listed in [Metrics Collected by the CloudWatch Agent](metrics-collected-by-CloudWatch-agent.md)\.
+ Collect system\-level metrics from on\-premises servers\. These can include servers in a hybrid environment as well as servers not managed by AWS\.
+ Retrieve custom metrics from your applications or services using the `StatsD` and `collectd` protocols\. `StatsD` is supported on both Linux servers and servers running Windows Server\. `collectd` is supported only on Linux servers\.
+ Collect logs from Amazon EC2 instances and on\-premises servers, running either Linux or Windows Server\.

You can store and view the metrics that you collect with the CloudWatch agent in CloudWatch just as you can with any other CloudWatch metrics\. The default namespace for metrics collected by the CloudWatch agent is `CWAgent`, although you can specify a different namespace when you configure the agent\.

The logs collected by the unified CloudWatch agent are processed and stored in Amazon CloudWatch Logs, just like logs collected by the older CloudWatch Logs agent\. For information about CloudWatch Logs pricing, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

Metrics collected by the CloudWatch agent are billed as custom metrics\. For more information about CloudWatch metrics pricing, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

The steps in this section explain how to install the unified CloudWatch agent on Amazon EC2 instances and on\-premises servers\. For more information about the metrics that the CloudWatch agent can collect, see [Metrics Collected by the CloudWatch Agent](metrics-collected-by-CloudWatch-agent.md)\.

**Supported Operating Systems**

The CloudWatch agent is supported on AMD64 architecture on the following operating systems:
+ Amazon Linux version 2014\.03\.02 or later
+ Amazon Linux 2
+ Ubuntu Server versions 18\.04, 16\.04, and 14\.04
+ CentOS versions 7\.6, 7\.2, 7\.0, 6\.8, and 6\.5
+ Red Hat Enterprise Linux \(RHEL\) versions 7\.6, 7\.5, 7\.4, 7\.2, 7\.0, and 6\.5
+ Debian 8\.0
+ SUSE Linux Enterprise Server \(SLES\) 12 or later
+ 64\-bit versions of Windows Server 2016, Windows Server 2012, and Windows Server 2008

The agent is supported on ARM64 architecture on the following operating systems:
+ Amazon Linux 2
+ Ubuntu Server version 18\.04
+ Red Hat Enterprise Linux \(RHEL\) version 7\.6

**Installation Process Overview**

You can download and install the CloudWatch agent manually using the command line, or you can integrate it with SSM\. The general flow of installing the CloudWatch agent using either method is as follows:

1. Create IAM roles or users that enable the agent to collect metrics from the server and optionally to integrate with AWS Systems Manager\.

1. Download the agent package\.

1. Modify the CloudWatch agent configuration file and specify the metrics that you want to collect\.

1. Install and start the agent on your servers\. As you install the agent on an EC2 instance, you attach the IAM role that you created in step 1\. As you install the agent on an on\-premises server, you specify a named profile that contains the credentials of the IAM user that you created in step 1\.

**Topics**
+ [Installing the CloudWatch Agent](install-CloudWatch-Agent-on-EC2-Instance.md)
+ [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)
+ [Metrics Collected by the CloudWatch Agent](metrics-collected-by-CloudWatch-agent.md)
+ [Common Scenarios with the CloudWatch Agent](CloudWatch-Agent-common-scenarios.md)
+ [Troubleshooting the CloudWatch Agent](troubleshooting-CloudWatch-Agent.md)