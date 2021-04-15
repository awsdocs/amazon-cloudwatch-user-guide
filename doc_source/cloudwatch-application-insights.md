# Amazon CloudWatch Application Insights<a name="cloudwatch-application-insights"></a>

Amazon CloudWatch Application Insights facilitates observability for your applications and underlying AWS resources\. It helps you set up the best monitors for your application resources to continuously analyze data for signs of problems with your applications\. Application Insights, which is powered by [Sagemaker](https://docs.aws.amazon.com/sagemaker/latest/dg/wahtis.html) and other AWS technologies, provides automated dashboards that show potential problems with monitored applications, which help you to quickly isolate ongoing issues with your applications and infrastructure\. The enhanced visibility into the health of your applications that Application Insights provides helps reduce mean time to repair \(MTTR\) to troubleshoot your application issues\.

When you add your applications to Amazon CloudWatch Application Insights, it scans the resources in the applications and recommends and configures metrics and logs on [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) for application components\. Example application components include SQL Server backend databases and Microsoft IIS/Web tiers\. Application Insights analyzes metric patterns using historical data to detect anomalies, and continuously detects errors and exceptions from your application, operating system, and infrastructure logs\. It correlates these observations using a combination of classification algorithms and built\-in rules\. Then, it automatically creates dashboards that show the relevant observations and problem severity information to help you prioritize your actions\. For common problems in \.NET and SQL application stacks, such as application latency, SQL Server failed backups, memory leaks, large HTTP requests, and canceled I/O operations, it provides additional insights that point to a possible root cause and steps for resolution\. Built\-in integration with [AWS SSM OpsCenter](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html) allows you to resolve issues by running the relevant Systems Manager Automation document\. 

**Topics**
+ [What Is Amazon CloudWatch Application Insights?](appinsights-what-is.md)
+ [How Application Insights works](appinsights-how-works.md)
+ [Get started](appinsights-getting-started.md)
+ [Work with component configurations](component-config.md)
+ [Use CloudFormation templates](appinsights-cloudformation.md)
+ [Tutorial: Set up monitors for \.NET and SQL](appinsights-tutorial-dotnet-sql.md)
+ [View and troubleshoot Application Insights](appinsights-troubleshooting.md)
+ [Supported logs and metrics](appinsights-logs-and-metrics.md)