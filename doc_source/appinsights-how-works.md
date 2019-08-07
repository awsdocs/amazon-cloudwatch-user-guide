# How Amazon CloudWatch Application Insights for \.NET and SQL Server Works<a name="appinsights-how-works"></a>

**Application Discovery and Configuration**  
The first time an application is added to Application Insights for \.NET and SQL Server, it scans the application components to recommend key metrics, logs, and other data sources to monitor for your application\. You can then configure your application based on these recommendations\. 

**Data Preprocessing**  
Application Insights for \.NET and SQL Server continuously analyzes the data sources being monitored across the application resources to discover metric anomalies and log errors \(observations\)\. 

**Intelligent Problem Detection**  
The Application Insights engine detects problems in your application by correlating observations using classification algorithms and built\-in rules\. To assist in troubleshooting, it creates automated CloudWatch dashboards, which include contextual information about the problems\. 

**Alert and Action**  
When Application Insights detects a problem with your application, it generates CloudWatch Events to notify you of the problem\. See [Set Up Notifications and Actions for Detected Problems](appinsights-troubleshooting.md#appinsights-cloudwatch-events) for more information about how to set up these Events\. 

**Example Scenario**

You have an ASP \.NET application that is backed by a SQL Server database\. Suddenly, your database begins to malfunction because of high memory pressure\. This leads to application performance degradation and possibly HTTP 500 errors in your web servers and load balancer\.

With CloudWatch Application Insights for \.NET and SQL Server and its intelligent analytics, you can identify the application layer that is causing the problem by checking the dynamically created dashboard that shows the related metrics and log file snippets\. In this case, the problem might be at the SQL database layer\.

## Data Retention<a name="appinsights-retention"></a>

CloudWatch Application Insights for \.NET and SQL Server retains problems for 55 days and observations for 60 days\.