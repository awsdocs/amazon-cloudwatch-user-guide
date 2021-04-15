# Monitor Applications Using AWS SDK Metrics<a name="CloudWatch-Agent-SDK-Metrics"></a>

Enterprise customers can use the CloudWatch agent with AWS SDK Metrics for Enterprise Support \(SDK Metrics\) to collect metrics from AWS SDKs on their hosts and clients\. These metrics are shared with AWS Enterprise Support\. SDK Metrics can help you collect relevant metrics and diagnostic data about your application's connections to AWS services without adding custom instrumentation to your code, and reduces the manual work necessary to share logs and data with AWS Support\.

**Important**  
SDK Metrics is available only to customers with an Enterprise Support subscription\. For more information, see [Amazon CloudWatch Support Center](https://console.aws.amazon.com/support/home#/)\.

You can use SDK Metrics with any application that directly calls AWS services and that was built using an AWS SDK that is one of the versions listed in the following section\.

SDK Metrics monitors calls that are made by the AWS SDK and uses the CloudWatch agent running in the same environment as a client application\. The following section explains the steps necessary to enable the CloudWatch agent to emit SDK Metrics data\. For information about what you need to configure in your SDK, see your SDK documentation\.

**Topics**
+ [Metrics and Data Collected by AWS SDK Metrics for Enterprise Support](metrics-collected-by-SDK-Metrics.md)
+ [Set Up SDK Metrics](Set-Up-SDK-Metrics.md)

**Supported Versions**

To use SDK Metrics, you must be using version 1\.207573\.0 or later of the CloudWatch agent\. If you are already running the CloudWatch agent, you can find the version in the following file\.
+ Linux: `/opt/aws/amazon-cloudwatch-agent/bin/CWAGENT_VERSION`
+ Windows Server: `C:\Program Files\Amazon\AmazonCloudWatchAgent\CWAGENT_VERSION`

You can also find the agent version using the command line\. On Linux, enter the following command\.

```
 /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent -version
```

On Windows Server, enter the following command\.

```
& 'C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent.exe' -version
```

Additionally, you must be using one of the following versions of an AWS SDK\.


| Supported SDK | SDK Metrics documentation | 
| --- | --- | 
|  AWS CLI 1\.16\.84 or later |  [SDK Metrics](http://boto3.amazonaws.com/v1/documentation/api/latest/guide/sdk-metrics.html) The instructions for the AWS CLI installation are the same as for SDK for Python \(Boto3\)\.  | 
|  AWS SDK for C\+\+ 1\.7\.47 or later |  [SDK Metrics](https://docs.aws.amazon.com/sdk-for-cpp/latest/developer-guide/sdk-metrics.html)  | 
|  AWS SDK for Go 1\.25\.18 or later |  [SDK Metrics in the AWS SDK for Go](https://docs.aws.amazon.com/sdk-for-go/latest/developer-guide/sdk-metrics.html)  | 
|  AWS SDK for Java 1\.11\.523 or later \(AWS SDK for Java 2\.x is not yet supported\) |  [Enabling AWS SDK Metrics for Enterprise Support](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/sdk-metrics.html)  | 
|  AWS SDK for JavaScript in Node\.js 2\.387 or later |  [SDK Metrics in the AWS SDK for JavaScript](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/metrics.html)  | 
|  AWS SDK for \.NET 3\.3\.440 or later |  [Enabling SDK Metrics](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/sdk-metrics.html)  | 
|  AWS SDK for PHP 3\.85\.0 or later |  [SDK Metrics in the AWS SDK for PHP Version 3](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_sdk-metrics.html)  | 
|  AWS SDK for Python \(Boto3\) 1\.9\.78 or later |  [SDK Metrics](http://boto3.amazonaws.com/v1/documentation/api/latest/guide/sdk-metrics.html)  | 
|  AWS SDK for Ruby 3\.45\.0 or later |  [SDK Metrics in the AWS SDK for Ruby](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/sdk-metrics.html)  | 