# How Amazon CloudWatch Application Insights works<a name="appinsights-how-works"></a>

This section contains information about how CloudWatch Application Insights works, including:

**Topics**
+ [How Application Insights monitors applications](#appinsights-how-works-sub)
+ [Data retention](#appinsights-retention)
+ [Quotas](#appinsights-limits)

## How Application Insights monitors applications<a name="appinsights-how-works-sub"></a>

Application Insights monitors \.NET and SQL Server applications as follows\.

**Application discovery and configuration**  
The first time an application is added to CloudWatch Application Insights it scans the application components to recommend key metrics, logs, and other data sources to monitor for your application\. You can then configure your application based on these recommendations\. 

**Data preprocessing**  
CloudWatch Application Insights continuously analyzes the data sources being monitored across the application resources to discover metric anomalies and log errors \(observations\)\. 

**Intelligent problem detection**  
The CloudWatch Application Insights engine detects problems in your application by correlating observations using classification algorithms and built\-in rules\. To assist in troubleshooting, it creates automated CloudWatch dashboards, which include contextual information about the problems\. 

**Alert and action**  
When CloudWatch Application Insights detects a problem with your application, it generates CloudWatch Events to notify you of the problem\. See [Set up notifications for detected problems](appinsights-setting-up.md#appinsights-cloudwatch-events) for more information about how to set up these Events\. 

**Example scenario**

You have an ASP \.NET application that is backed by a SQL Server database\. Suddenly, your database begins to malfunction because of high memory pressure\. This leads to application performance degradation and possibly HTTP 500 errors in your web servers and load balancer\.

With CloudWatch Application Insights and its intelligent analytics, you can identify the application layer that is causing the problem by checking the dynamically created dashboard that shows the related metrics and log file snippets\. In this case, the problem might be at the SQL database layer\.

## Data retention<a name="appinsights-retention"></a>

CloudWatch Application Insights retains problems for 55 days and observations for 60 days\.

## Quotas<a name="appinsights-limits"></a>

The following table provides the default quotas for CloudWatch Application Insights\. Unless otherwise noted, each quota is per AWS Region\. Please contact [AWS Support](https://console.aws.amazon.com/support/home#/case/create?issueType=technical) to request an increase in your service quota\. Many services contain quotas that cannot be changed\. For more information about the quotas for a specific service, see the documentation for that service\. 


| Resource  | Default quota | 
| --- | --- | 
|  API requests  |  All API actions are throttled to 5 TPS  | 
| Applications  |  10 per account  | 
| Log Streams  |  5 per resource  | 
| Observations per problem  |  20 per dashboard 40 per DescribeProblemObservations action  | 
| Metrics |  30 per resource  | 
| Resources  |  30 per application  | 