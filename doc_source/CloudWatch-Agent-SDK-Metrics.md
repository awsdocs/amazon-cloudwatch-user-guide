# Monitor Applications Using AWS SDK Metrics<a name="CloudWatch-Agent-SDK-Metrics"></a>

Enterprise customers can use the CloudWatch agent with AWS SDK Metrics for Enterprise Support \(SDK Metrics\) to collect metrics from AWS SDKs on their hosts and clients\. These metrics are shared with AWS Enterprise Support\. SDK Metrics can help you collect relevant metrics and diagnostic data about your application's connections to AWS services without adding custom instrumentation to your code, and reduces the manual work necessary to share logs and data with AWS Support\.

**Important**  
SDK Metrics is available only to customers with an Enterprise Support subscription\. For more information, see [Amazon CloudWatch Support Center](https://console.aws.amazon.com/support/home#/)\.

You can use SDK Metrics with any application that directly calls AWS services and that was built using a recent version of an AWS SDK\.

SDK Metrics instruments calls made by the AWS SDK and uses the CloudWatch agent running in the same environment as a client application that is using an AWS SDK\. The following section explains the steps necessary to enable the CloudWatch agent to emit SDK Metrics data\. For information about what you need to configure in your SDK, see your SDK documentation\.

**Topics**
+ [Metrics and Data Collected by AWS SDK Metrics for Enterprise Support](metrics-collected-by-SDK-Metrics.md)
+ [Configure the CloudWatch Agent for SDK Metrics](Configure-CloudWatch-Agent-SDK-Metrics.md)
+ [Set IAM Permissions for SDK Metrics](Set-IAM-Permissions-For-SDK-Metrics.md)