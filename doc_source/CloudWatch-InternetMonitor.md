# Using Amazon CloudWatch Internet Monitor<a name="CloudWatch-InternetMonitor"></a>

Amazon CloudWatch Internet Monitor provides visibility into how internet issues impact the performance and availability between your applications hosted on AWS and your end users\. It can reduce the time it takes for you to diagnose internet issues from days to minutes\. Internet Monitor uses the connectivity data that AWS captures from its global networking footprint to calculate a baseline of performance and availability for internet\-facing traffic\. This is the same data that AWS uses to monitor internet uptime and availability\. With those measurements as a baseline, Internet Monitor raises awareness for you when there are significant problems for your end users \(clients\) in the different geographic locations where your application runs\.

In the Amazon CloudWatch console, you can see a global view of traffic patterns and health events, and easily drill down into information about events, at different geographic granularities \(locations\)\. You can clearly visualize impact, and pinpoint the locations and providers \(ASNs, typically internet service providers or ISPs\) that are affected\. If Internet Monitor determines that an internet availability or performance issue is caused by a specific provider or by the AWS network, it provides that information\.

**Key features of Internet Monitor**
+ Internet Monitor suggests insights and recommendations that can help you improve your end users' experience\. You can explore, in near real\-time, how to improve the projected latency of your application by switching to use other services, or by rerouting traffic to your workload through different AWS Regions\.
+ With Internet Monitor, you can quickly identify what's impacting your application's performance and availability, so that you can track down and address issues\.
+ Internet Monitor publishes internet measurements to CloudWatch Logs and CloudWatch Metrics, to support using CloudWatch tools with health information for locations and ASNs \(internet service providers\) specific to your application\. Optionally, you can also publish internet measurements to Amazon S3\.
+ Internet Monitor sends health events to Amazon EventBridge so that you can set up notifications\. If an issue is caused by the AWS network, you also automatically receive an AWS Health Dashboard notification with the steps that AWS is taking to mitigate the problem\.

**How to use Internet Monitor**

To use Internet Monitor, you create a *monitor* and associate your application's resources with it, Amazon Virtual Private Clouds \(VPCs\), CloudFront distributions, or WorkSpaces directories, to enable Internet Monitor to know where your application's internet\-facing traffic is\. Internet Monitor then publishes internet measurements from AWS that are specific to the *city\-networks*, that is, the locations and ASNs \(typically internet service providers or ISPs\), where clients access your application\. For more information about how to begin working with Internet Monitor, see [Getting started with Amazon CloudWatch Internet Monitor using the console](CloudWatch-IM-get-started.md)\.

**Topics**
+ [Supported Regions](CloudWatch-InternetMonitor.Regions.md)
+ [Pricing](CloudWatch-InternetMonitor.pricing.md)
+ [Components](CloudWatch-IM-components.md)
+ [How Internet Monitor works](CloudWatch-IM-inside-internet-monitor.md)
+ [Use cases](CloudWatch-IM-use-cases.md)
+ [Getting started](CloudWatch-IM-get-started.md)
+ [Examples with the CLI](CloudWatch-IM-get-started-CLI.md)
+ [Internet Monitor dashboard](CloudWatch-IM-monitor-and-optimize.md)
+ [Log file insights](CloudWatch-IM-view-cw-tools.md)
+ [Create alarms](CloudWatch-IM-create-alarm.md)
+ [EventBridge integration](CloudWatch-IM-EventBridge-integration.md)
+ [Data protection and data privacy](CloudWatch-IM-privacy.md)
+ [Identity and Access Management](security-iam.md)
+ [Quotas](CloudWatch-IM-quotas.md)