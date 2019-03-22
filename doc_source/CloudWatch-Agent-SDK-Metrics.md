# Monitor Applications Using AWS SDK Metrics<a name="CloudWatch-Agent-SDK-Metrics"></a>

Enterprise customers can use the CloudWatch agent with AWS SDK Metrics for Enterprise Support \(SDK Metrics\) to collect metrics from AWS SDKs on their hosts and clients\. These metrics are shared with AWS Enterprise Support\. SDK Metrics can help you collect relevant metrics and diagnostic data about your application's connections to AWS services without adding custom instrumentation to your code, and reduces the manual work necessary to share logs and data with AWS Support\.

**Important**  
SDK Metrics is available only to customers with an Enterprise Support subscription\. For more information, see [Amazon CloudWatch Support Center](https://console.aws.amazon.com/support/home#/)\.

You can use SDK Metrics with any application that directly calls AWS services and that was built using an AWS SDK that is one of the versions listed in the following section\.

SDK Metrics instruments calls made by the AWS SDK and uses the CloudWatch agent running in the same environment as a client application that is using an AWS SDK\. The following section explains the steps necessary to enable the CloudWatch agent to emit SDK Metrics data\. For information about what you need to configure in your SDK, see your SDK documentation\.

**Topics**
+ [Metrics and Data Collected by AWS SDK Metrics for Enterprise Support](metrics-collected-by-SDK-Metrics.md)
+ [Configure the CloudWatch Agent for SDK Metrics](Configure-CloudWatch-Agent-SDK-Metrics.md)
+ [Set IAM Permissions for SDK Metrics](Set-IAM-Permissions-For-SDK-Metrics.md)

**Supported Versions**

To use SDK Metrics, you must be using version 1\.207573\.0 or later of the CloudWatch agent\. If you are already running the CloudWatch agent, you can find the version in the following file\.
+ Linux: `/opt/aws/amazon-cloudwatch-agent/bin/CWAGENT_VERSION`
+ Windows Server: `C:\Program Files\Amazon\AmazonCloudWatchAgent\CWAGENT_VERSION`

Additionally, you must be using one of the following versions of an AWS SDK\.
+ AWS CLI 1\.16\.84 or later
+ AWS SDK for C\+\+ 1\.7\.47 or later
+ AWS SDK for Go 1\.16\.18 or later
+ AWS SDK for Java 1\.11\.482 or later
+ AWS SDK for JavaScript in Node\.js 2\.387 or later
+ AWS SDK for \.NET 3\.3\.440 or later
+ AWS SDK for PHP 3\.85\.0 or later
+ AWS SDK for Python \(Boto 3\) 1\.9\.78 or later
+ AWS SDK for Ruby 3\.45\.0 or later