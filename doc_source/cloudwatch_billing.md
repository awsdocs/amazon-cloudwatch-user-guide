# CloudWatch billing and cost<a name="cloudwatch_billing"></a>

 This section describes how Amazon CloudWatch features generate costs\. It also provides methods that can help you analyze, optimize, and reduce CloudWatch costs\. Throughout this section, we sometimes refer to pricing when describing CloudWatch features\. For information about pricing, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/?nc1=h_ls)\.

**Topics**
+ [Analyze CloudWatch cost and usage data with Cost Explorer](#w2aab5c19b7)
+ [Analyze CloudWatch cost and usage data with AWS Cost and Usage Reports and Athena](#w2aab5c19b9)
+ [Best practices for optimizing and reducing costs](#w2aab5c19c11)

## Analyze CloudWatch cost and usage data with Cost Explorer<a name="w2aab5c19b7"></a>

 With AWS Cost Explorer, you can visualize and analyze cost and usage data for AWS services over time, including CloudWatch\. For more information, see [Getting started with AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/getting-started/)\. 

 The following procedure describes how to use Cost Explorer to visualize and analyze CloudWatch cost and usage data\. 

### To visualize and analyze CloudWatch cost and usage data<a name="w2aab5c19b7b9"></a>

1.  Sign in to the Cost Explorer console at [https://console\.aws\.amazon\.com/cost\-management/home\#/custom](https://console.aws.amazon.com/cost-management/home#/custom)\. 

1.  Under **FILTERS**, for **Service**, select **CloudWatch**\. 

1.  For **Group by**, choose **Usage Type**\. You can also group your results by other categories, such as the following: 
   +  ** API Operation ** – See which API operations generated the most costs\. 
   +  ** Region ** – See which Regions generated the most costs\. 

 The following image shows an example of the costs that CloudWatch features generated over six months\. 

![\[A screenshot of the AWS Cost Explorer interface, showing Usage Type costs in a bar graph format.\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/ce.png)

 To see which CloudWatch features generated the most costs, look at the values for `UsageType`\. For example, `EU-CW:GMD-Metrics` represents the costs that CloudWatch bulk API requests generated\. 

**Note**  
 The strings for `UsageType` match specific features and Regions\. For example, the first part of `EU-CW:GMD-Metrics` \(`EU`\) matches the Europe \(Ireland\) Region, and the second part of `EU-CW:GMD-Metrics` \(`GMD-Metrics`\) matches CloudWatch bulk API requests\.   
 The entire string for `UsageType` can be formatted as follows: `<Region>-CW:<Feature>` or `<Region>-<Feature>`\.   
 To enhance readability, the strings for `UsageType` in the tables throughout this document have been shortened to their string suffixes\. For example, `EU-CW:GMD-Metrics` is shortened to `GMD-Metrics`\. 

 The following table includes the names of each CloudWatch feature, lists the names of each subfeature, and lists the strings for `UsageType`\. 


| CloudWatch feature | *CloudWatch subfeature* |  `UsageType`  | 
| --- | --- | --- | 
| CloudWatch metrics | Custom metrics |  `MetricMonitorUsage`  | 
|  | Detailed monitoring |  `MetricMonitorUsage`  | 
|  | Embedded metrics |  `MetricMonitorUsage`  | 
| CloudWatch API requests | API requests |  `Requests`  | 
|  | Bulk \(Get\) |  `GMD-Metrics`  | 
|  | Contributor Insights |  `GIRR-Metrics`  | 
|  | Bitmap image snapshot |  `GMWI-Metrics`  | 
| CloudWatch metric streams | Metric streams |  `MetricStreamUsage`  | 
| CloudWatch dashboards | Dashboard with 50 or fewer metrics |  `DashboardsUsageHour-Basic`  | 
|  | Dashboard with more than 50 metrics |  `DashboardsUsageHour`  | 
| CloudWatch alarms | Standard \(metric alarm\) |  `AlarmMonitorUsage`  | 
|  | High resolution \(metric alarm\) |  `HighResAlarmMonitorUsage`  | 
|  | Metrics Insights query alarm |  `MetricInsightAlarmUsage`  | 
|  | Composite \(aggregated alarm\) |  `CompositeAlarmMonitorUsage`  | 
| CloudWatch custom logs | Collect \(ingest\) |  `DataProcessing-Bytes`  | 
|  | Store \(archive\) |  `TimedStorage-ByteHrs`  | 
|  | Analyze \(query\) |  `DataScanned-Bytes`  | 
| CloudWatch vended logs | Delivery \(Amazon CloudWatch Logs\) |  `VendedLog-Bytes`  | 
|  | Delivery \(Amazon Simple Storage Service\) |  `S3-Egress-ComprBytes` `S3-Egress-Bytes`  | 
|  | Delivery \(Amazon Kinesis Data Firehose\) |  `FH-Egress-Bytes`  | 
| Contributor Insights | CloudWatch Logs \(Rules\) |  `ContributorInsightRules`  | 
|  | CloudWatch Logs \(Events\) |  `ContributorInsightEvents`  | 
|  | Amazon DynamoDB \(Rules\) |  `ContributorRulesManaged`  | 
|  | DynamoDB Events\) |  `ContributorEventsManaged`  | 
| Canaries \(Synthetics\) | Run |  `Canary-runs`  | 
| Evidently | Events |  `Evidently-event`  | 
|  | Analysis Units |  `Evidently-eau`  | 
| RUM | Events |  `RUM-event`  | 

## Analyze CloudWatch cost and usage data with AWS Cost and Usage Reports and Athena<a name="w2aab5c19b9"></a>

Another way to analyze CloudWatch cost and usage data is by using AWS Cost and Usage Reports with Amazon Athena\. AWS Cost and Usage Reports contain a comprehensive set of cost and usage data\. You can create reports that track your costs and usage, and you can publish these reports to an S3 bucket of your choice\. You also can download and delete your reports from your S3 bucket\. For more information, see [What are AWS Cost and Usage Reports?](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html) in the *AWS Cost and Usage Reports User Guide*\. 

**Note**  
 There is no charge for using AWS Cost and Usage Reports\. You only pay for storage when you publish your reports to Amazon Simple Storage Service \(Amazon S3\)\. For more information, see [Quotas and restrictions](https://docs.aws.amazon.com/cur/latest/userguide/billing-cur-limits.html) in the *AWS Cost and Usage Reports User Guide*\. 

 Athena is a query service that you can use with AWS Cost and Usage Reports to analyze cost and usage data\. You can query your reports in your S3 bucket without needing to download them first\. For more information, see [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) in the *Amazon Athena User Guide*\. For more information, see [What is Amazon Athena?](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) in the *Amazon Athena User Guide*\. For information about pricing, see [Amazon Athena pricing](https://aws.amazon.com/athena/pricing/)\. 

 The following procedure describes the process for enabling AWS Cost and Usage Reports and integrating the service with Athena\. The procedure contains two example queries that you can use to analyze CloudWatch cost and usage data\. 

**Note**  
 You can use any of the example queries in this document\. All of the example queries in this document correspond to a database named ***costandusagereport***, and show results for the month of April and the year 2022\. You can change this information\. However, before you run a query, make sure that the name of your database matches the name of the database in the query\. 

### To analyze cost and usage data with AWS Cost and Usage Reports and Athena<a name="w2aab5c19b9c13"></a>

1.  Enable AWS Cost and Usage Reports\. For more information, see [Creating cost and usage reports](https://docs.aws.amazon.com/cur/latest/userguide/cur-create.html) in the *AWS Cost and Usage Reports User Guide*\. 
**Tip**  
 When you create your reports, make sure to select **Include resource IDs**\. Otherwise, your reports won't include the column `line_item_resource_id`\. This line helps you further identify costs when analyzing cost and usage data\.

1.  Integrate AWS Cost and Usage Reports with Athena\. For more information, see [Setting up Athena using AWS CloudFormation templates](https://docs.aws.amazon.com/cur/latest/userguide/use-athena-cf.html) in the *AWS Cost and Usage Reports User Guide*\. 

1.  Query your cost and usage reports\. 

 ** *Example: Athena query* ** 

 You can use the following query to show which CloudWatch features generated the most costs for a given month\. 

```
SELECT
CASE
-- Metrics
WHEN line_item_usage_type LIKE '%%MetricMonitorUsage%%' THEN 'Metrics (Custom, Detailed monitoring management portal EMF)'
WHEN line_item_usage_type LIKE '%%Requests%%' THEN 'Metrics (API Requests)'
WHEN line_item_usage_type LIKE '%%GMD-Metrics%%' THEN 'Metrics (Bulk API Requests)'
WHEN line_item_usage_type LIKE '%%MetricStreamUsage%%' THEN 'Metric Streams'
-- Dashboard
WHEN line_item_usage_type LIKE '%%DashboardsUsageHour%%' THEN 'Dashboards'
-- Alarms
WHEN line_item_usage_type LIKE '%%AlarmMonitorUsage%%' THEN 'Alarms (Standard)'
WHEN line_item_usage_type LIKE '%%HighResAlarmMonitorUsage%%' THEN 'Alarms (High Resolution)'
WHEN line_item_usage_type LIKE '%%MetricInsightAlarmUsage%%' THEN 'Alarms (Metrics Insights)'
WHEN line_item_usage_type LIKE '%%CompositeAlarmMonitorUsage%%' THEN 'Alarms (Composite)'
-- Logs
WHEN line_item_usage_type LIKE '%%DataProcessing-Bytes%%' THEN 'Logs (Collect - Data Ingestion)'
WHEN line_item_usage_type LIKE '%%TimedStorage-ByteHrs%%' THEN 'Logs (Storage - Archival)'
WHEN line_item_usage_type LIKE '%%DataScanned-Bytes%%' THEN 'Logs (Analyze - Logs Insights queries)'
-- Vended Logs
WHEN line_item_usage_type LIKE '%%VendedLog-Bytes%%' THEN 'Vended Logs (Delivered to CW)'
WHEN line_item_usage_type LIKE '%%FH-Egress-Bytes%%' THEN 'Vended Logs (Delivered to Kinesis FH)'
WHEN (line_item_usage_type LIKE '%%S3-Egress-Bytes%%') OR (line_item_usage_type LIKE '%%S3-Egress-
ComprBytes%%') THEN 'Vended Logs (Delivered to S3)'
-- Other
WHEN line_item_usage_type LIKE '%%Canary-runs%%' THEN 'Synthetics'
WHEN line_item_usage_type LIKE '%%Evidently%%' THEN 'Evidently' 
WHEN line_item_usage_type LIKE '%%RUM-event%%' THEN 'RUM'
ELSE 'Others'
END AS UsageType,
-- REGEXP_EXTRACT(line_item_resource_id,'^(?:.+?:){5}(.+)$',1) as ResourceID,
-- SUM(CAST(line_item_usage_amount AS double)) AS UsageQuantity,
SUM(CAST(line_item_unblended_cost AS decimal(16,8))) AS TotalSpend
FROM
costandusagereport 
WHERE
product_product_name = 'AmazonCloudWatch'
AND year='2022' 
AND month='4' 
AND line_item_line_item_type NOT IN ('Tax','Credit','Refund','EdpDiscount','Fee','RIFee')
-- AND line_item_usage_account_id = '123456789012' – If you want to filter on a specific account, you can
remove this comment at the beginning of the line and specify an AWS account.
GROUP BY
1
ORDER BY
TotalSpend DESC,
UsageType;
```

 ** *Example: Athena query* ** 

 You can use the following query to show the results for `UsageType` and `Operation`\. This shows you how CloudWatch features generated costs\. The results also show the values for `UsageQuantity` and `TotalSpend`, so that you can see your total usage costs\. 

**Tip**  
 For more information about `UsageType`, add the following line to this query:   
 `line_item_line_item_description`   
This line creates a column called ***Description***\. 

```
SELECT
bill_payer_account_id as Payer,
line_item_usage_account_id as LinkedAccount,
line_item_usage_type AS UsageType,
line_item_operation AS Operation,
line_item_resource_id AS ResourceID,
SUM(CAST(line_item_usage_amount AS double)) AS UsageQuantity,
SUM(CAST(line_item_unblended_cost AS decimal(16,8))) AS TotalSpend
FROM
costandusagereport 
WHERE
product_product_name = 'AmazonCloudWatch'
AND year='2022' 
AND month='4' 
AND line_item_line_item_type NOT IN ('Tax','Credit','Refund','EdpDiscount','Fee','RIFee')
GROUP BY
bill_payer_account_id,
line_item_usage_account_id,
line_item_usage_type,
line_item_resource_id,
line_item_operation
```

## Best practices for optimizing and reducing costs<a name="w2aab5c19c11"></a>

### CloudWatch metrics<a name="w2aab5c19c11b3"></a>

 Many AWS services, such as Amazon Elastic Compute Cloud \(Amazon EC2\) , Amazon S3, and Amazon Kinesis Data Firehose, automatically send metrics to CloudWatch at no charge\. However, metrics that are grouped in the following categories can incur additional costs: 
+ ***Custom metrics, detailed monitoring, and embedded metrics***
+ ***API requests***
+ ***Metric streams***

 For more information, see [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)\. 

#### Custom metrics, detailed monitoring, and embedded metrics<a name="w2aab5c19c11b3b9"></a>

##### Custom metrics<a name="w2aab5c19c11b3b9b3"></a>

 You can create custom metrics to organize data points in any order and at any rate\. 

 All custom metrics are prorated by the hour\. They're metered only when they're sent to CloudWatch\. For information about how metrics are priced, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/?nc1=h_ls)\. 

 The following table lists the names of relevant subfeatures for CloudWatch metrics\. The table includes the strings for `UsageType` and `Operation`, which can help you analyze and identify metric\-related costs\. 

**Note**  
 To get more details about the metrics that are listed in the following table while you're querying cost and usage data with Athena, match the strings for `Operation` with the results that are shown for `line_item_operation`\. 


| *CloudWatch subfeature* | `UsageType` | `Operation` | Purpose | 
| --- | --- | --- | --- | 
|  *Custom metrics*  |  `MetricMonitorUsage`  |  `MetricStorage`  |  Custom metrics  | 
| *Detailed monitoring* | `MetricMonitorUsage` | `MetricStorage:AWS/{Service}` | Detailed monitoring | 
| *Embedded metrics* | `MetricMonitorUsage` | `MetricStorage:AWS/Logs-EMF` | Logs embedded metrics | 
| *Log filters* | `MetricMonitorUsage` | `MetricStorage:AWS/CloudWatchLogs` | Log group metric filters | 

##### Detailed monitoring<a name="w2aab5c19c11b3b9b3c13"></a>

 CloudWatch has two types of monitoring: 
+  ***Basic monitoring*** 

   Basic monitoring is free and automatically enabled for all AWS services that support the feature\. 
+  ***Detailed monitoring*** 

   Detailed monitoring incurs costs and adds different enhancements depending on the AWS service\. For each AWS service that supports detailed monitoring, you can choose whether to enable it for that service\. For more information, see [Basic and detailed monitoring](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-metrics-basic-detailed.html)\. 

**Note**  
 Other AWS services support detailed monitoring and might refer to this feature using a different name\. For example, detailed monitoring for Amazon S3 is referred to as ***request metrics***\. 

 Similar to custom metrics, detailed monitoring is prorated by the hour and metered only when data is sent to CloudWatch\. Detailed monitoring generates costs by the number of metrics that are sent to CloudWatch\. To reduce costs, only enable detailed monitoring when necessary\. For information about how detailed monitoring is priced, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/?nc1=h_ls)\. 

 ** * Example: Athena query * ** 

 You can use the following query to show which EC2 instances have detailed monitoring enabled\. 

```
SELECT
bill_payer_account_id as Payer,
line_item_usage_account_id as LinkedAccount,
line_item_usage_type AS UsageType,
line_item_operation AS Operation,
line_item_resource_id AS ResourceID,
SUM(CAST(line_item_usage_amount AS double)) AS UsageQuantity,
SUM(CAST(line_item_unblended_cost AS decimal(16,8))) AS TotalSpend
FROM
costandusagereport 
WHERE
product_product_name = 'AmazonCloudWatch'
AND year='2022' 
AND month='4' 
AND line_item_operation='MetricStorage:AWS/EC2'
AND line_item_line_item_type NOT IN ('Tax','Credit','Refund','EdpDiscount','Fee','RIFee')
GROUP BY
bill_payer_account_id,
line_item_usage_account_id,
line_item_usage_type,
line_item_resource_id,
line_item_operation,
line_item_line_item_description
ORDER BY line_item_operation
```

##### Embedded metrics<a name="w2aab5c19c11b3b9b5"></a>

 With the CloudWatch embedded metric format, you can ingest application data as log data, so that you can generate actionable metrics\. For more information, see [Ingesting high\-cardinality logs and generating metrics with the CloudWatch embedded metric format](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format.html)\. 

 Embedded metrics generate costs by the number of logs ingested, number of logs archived, and number of custom metrics generated\. 

 The following table lists the names of relevant subfeatures for the CloudWatch embedded metric format\. The table includes the strings for `UsageType` and `Operation`, which can help you analyze and identify costs\. 


| *CloudWatch subfeature* | `UsageType` | `Operation` | Purpose | 
| --- | --- | --- | --- | 
|  *Custom metrics*  |  `MetricMonitorUsage`  |  `MetricStorage:AWS/Logs-EMF`  | Logs embedded metrics | 
|  *Logs ingestion*  |  `DataProcessing-Bytes`  |  `PutLogEvents`  | Uploads a batch of log events to the specified log group or log stream | 
|  *Logs archival*  |  `TimedStorage-ByteHrs`  |  `HourlyStorageMetering`  | Stores logs per hour and logs per byte in CloudWatch Logs | 

To analyze costs, use AWS Cost and Usage Reports with Athena so that you can identify which metrics are generating costs and determine how the costs are generated\. 

 To make the most of costs generated by the CloudWatch embedded metric format, avoid creating metrics based on high\-cardinality dimensions\. This way, CloudWatch doesn't create a custom metric for each unique dimension combination\. For more information, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension)\. 

 If you're using CloudWatch Container Insights to leverage the embedded metric format, you can use AWS Distro for Open Telemetry as an alternative to make the most of metric\-related costs\. With Container Insights, you can collect, aggregate, and summarize metrics and logs from your containerized applications and microservices\. When you enable Container Insights, the CloudWatch agent sends your logs to CloudWatch, so it can use the logs to generate embedded metrics\. However, the CloudWatch agent only sends a fixed number of metrics to CloudWatch, and you're charged for all available metrics, including any that you're not using\. With AWS Distro for Open Telemetry, you can configure and customize which metrics and dimensions are sent to CloudWatch\. This helps you reduce the volume of data and cost that Container Insights generates\. For more information, see the following resources: 
+  [Using Container Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ContainerInsights.html) 
+  [AWS Distro for Open Telemetry](https://aws-otel.github.io/) 

#### API requests<a name="w2aab5c19c11b3c11"></a>

 CloudWatch has the following types of API requests: 
+  ** * API requests * ** 
+  ** * Bulk \(Get\) * ** 
+  ** * Contributor Insights * ** 
+  ** * Bitmap image snapshot * ** 

 API requests generate costs by the request type and number of metrics requested\. 

 The following table lists the types of API requests and includes the strings for `UsageType` and `Operation`, which can help you analyze and identify API\-related costs\. 


| *API request type* | `UsageType` | `Operation` | Purpose | 
| --- | --- | --- | --- | 
| API requests | `Requests` | `GetMetricStatistics` | Retrieves statistics for the specified metrics | 
|  | `Requests` | `ListMetrics` | Lists the specified metrics | 
|  | `Requests` | `PutMetricData` | Publishes metric data points to CloudWatch | 
|  | `Requests` | `GetDashboard` | Displays details for the specified dashboards | 
|  | `Requests` | `ListDashboards` | Lists the dashboards in your account | 
|  | `Requests` | `PutDashboard` | Creates or updates a dashboard | 
|  | `Requests` | `DeleteDashboards` | Deletes all specified dashboards | 
| Bulk \(Get\) | `GMD-Metrics` | `GetMetricData` | Retrieves CloudWatch metric values | 
| Contributor Insights | `GIRR-Metrics` | `GetInsightRuleReport` | Returns time\-series data that's collected by a Contributor Insights rule  | 
| Bitmap image snapshot | `GMWI-Metrics` | `GetMetricWidgetImage` | Retrieves a snapshot of one or more CloudWatch metrics as a bitmap image  | 

 To analyze costs, use Cost Explorer, and group your results by ***API Operation***\. 

 Costs for API requests vary, and you incur costs when you exceed the number of API calls provided to you under the AWS Free Tier limit\. 

**Note**  
 `GetMetricData` and `GetMetricWidgetImage` aren't included under the AWS Free Tier limit\. For more information, see [Using the AWS Free Tier](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-free-tier.html) in the *AWS Billing User Guide*\. 

 The API requests that typically drive cost are `Put` and `Get` requests\. 

***`PutMetricData`***

 `PutMetricData` generates costs every time that it's called and can incur significant costs depending on the use case\. For more information, see [PutMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) in the *Amazon CloudWatch API Reference*\. 

 To make the most of costs that are generated by `PutMetricData`, batch more data into your API calls\. Depending on your use case, consider using CloudWatch Logs or the CloudWatch embedded metric format to inject metric data\. For more information, see the following resources: 
+ [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) in the *Amazon CloudWatch Logs User Guide*
+ [Ingesting high\-cardinality logs and generating metrics with CloudWatch embedded metric format](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Embedded_Metric_Format.html)
+ [Lowering costs and focusing on our customers with Amazon CloudWatch embedded custom metrics](https://aws.amazon.com/blogs/mt/lowering-costs-and-focusing-on-our-customers-with-amazon-cloudwatch-embedded-custom-metrics/) 

***`GetMetricData`***

 `GetMetricData` can also generate significant costs\. Common use cases that drive cost involve third\-party monitoring tools that pull data to generate insights\. For more information, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html) in the *Amazon CloudWatch API Reference*\. 

 To reduce costs generated by `GetMetricData`, consider only pulling data that's monitored and used, or consider pulling data less often\. Depending on your use case, you might consider using metric streams instead of `GetMetricData`, so that you can push data in near real time to third parties at a lower cost\. For more information, see the following resources: 
+ [Using metric streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Metric-Streams.html)
+ [CloudWatch Metric Streams \- Send AWS Metrics to Partners and to Your Apps in Real Time](https://aws.amazon.com/blogs/aws/cloudwatch-metric-streams-send-aws-metrics-to-partners-and-to-your-apps-in-real-time/)

***`GetMetricStatistics`***

 Depending on your use case, you might consider using `GetMetricStatistics` instead of `GetMetricData`\. With `GetMetricData`, you can retrieve data quickly and at scale\. However, `GetMetricStatistics` is included under the AWS Free Tier limit for up to one million API requests, which can help you reduce costs if you don't need to retrieve as many metrics and data points per call\. For more information, see the following resources: 
+  [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference* 
+  [Should I use GetMetricData or GetMetricStatistics?](https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-getmetricdata-api/) 

**Note**  
 External callers make API calls\. Currently, the only way to identify these callers is by opening a technical support request to the CloudWatch team and asking for information about them\. For information about creating a technical support request, see [How do I get technical support from AWS?](https://aws.amazon.com/de/premiumsupport/knowledge-center/get-aws-technical-support/)\. 

#### CloudWatch metric streams<a name="w2aab5c19c11b3c13"></a>

 With CloudWatch metric streams, you can send metrics continuously to AWS destinations and third\-party service provider destinations\. 

 Metric streams generate costs by the number of metric updates\. Metric updates always include values for the following statistics: 
+ `Minimum`
+ `Maximum`
+ `Sample Count`
+  `Sum` 

 For more information, see [Statistics that can be streamed](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-metric-streams-statistics.html)\. 

To analyze costs that are generated by CloudWatch metric streams, use AWS Cost and Usage Reports with Athena\. This way, you can identify which metric streams are generating costs and determine how the costs are generated\. 

 ** * Example: Athena query * ** 

 You can use the following query to track which metric streams generate costs by Amazon Resource Name \(ARN\)\. 

```
SELECT
SPLIT_PART(line_item_resource_id,'/',2) AS "Stream Name",
line_item_resource_id as ARN,
SUM(CAST(line_item_unblended_cost AS decimal(16,2))) AS TotalSpend
FROM
costandusagereport 
WHERE
product_product_name = 'AmazonCloudWatch'
AND year='2022' 
AND month='4' 
AND line_item_line_item_type NOT IN ('Tax','Credit','Refund','EdpDiscount','Fee','RIFee')
-- AND line_item_usage_account_id = '123456789012' – If you want to filter on a specific account, you can
remove this comment at the beginning of the line and specify an AWS account.
AND line_item_usage_type LIKE '%%MetricStreamUsage%%'
GROUP BY line_item_resource_id
ORDER BY TotalSpend DESC
```

To reduce costs generated by CloudWatch metric streams, stream only the metrics that bring your business value\. You also can stop or pause any metric stream that you're not using\. 

### CloudWatch alarms<a name="w2aab5c19c11b5"></a>

 With CloudWatch alarms, you can create alarms based on a single metric, alarms based on a Metrics Insights query, and composite alarms which watch other alarms\. 

**Note**  
 Costs for metric and composite alarms are prorated by the hour\. You incur costs for your alarms only while your alarms exist\. 

 ** Metric alarms ** 

 Metric alarms have the following resolution settings: 
+  **Standard** \(evaluated every 60 seconds\) 
+  **High resolution** \(evaluated every 10 seconds\) 

 When you create a metric alarm, your costs are based on your alarm’s resolution setting and the number of metrics that your alarm references\. For example, a metric alarm that references one metric incurs one alarm\-metric cost per hour\. For more information, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. 

 If you create a metric alarm that contains a metric math expression, which references multiple metrics, you incur a cost for each alarm\-metric that’s referenced in the metric math expression\. For information about how to create a metric alarm that contains a metric math expression, see [Creating a CloudWatch alarm based on a metric math expression](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create-alarm-on-metric-math-expression.html)\. 

 If you create an anomaly detection alarm, where your alarm analyzes past metric data to create a model of expected values, you incur a cost for each alarm\-metric that's referenced in your alarm plus two additional metrics, one for the upper and lower band metrics that the anomaly detection model creates\. For information about how to create an anomaly detection alarm, see [Creating a CloudWatch alarm based on anomaly detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create_Anomaly_Detection_Alarm.html)\. 

 ** Metrics Insights query alarms ** 

 Metric Insights query alarms are a specific type of metric alarm, only available with standard resolution \(evaluated every 60 seconds\)\. 

 When you create a Metric Insights query alarm, your costs are based on the number of metrics analyzed by the query that your alarm references\. For example, a Metric Insights query alarm that references a query whose filter matches ten metrics incurs ten metrics analyzed cost per hour\. For more information, see the pricing example on [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/?nc1=h_ls)\. 

 If you create an alarm that contains both a Metrics Insights query and a metric math expression, it is reported as a Metrics Insights query alarm\. If your alarm contains a metric math expression which references other metrics in addition to the metrics analyzed by the Metrics Insights query, you incur an additional cost for each alarm\-metric that’s referenced in the metric math expression\. For information about how to create a metric alarm that contains a metric math expression, see [Creating a CloudWatch alarm based on a metric math expression](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create-alarm-on-metric-math-expression.html)\. 

 ** Composite alarms ** 

 Composite alarms contain rule expressions that specify how they should evaluate the states of other alarms to determine their own states\. Composite alarms incur a standard cost per hour, regardless of how many other alarms they evaluate\. Alarms that composite alarms reference in rule expressions incur separate costs\. For more information, see [Creating a composite alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create_Composite_Alarm.html)\. 

**Alarm usage types**

The following table lists the names of relevant subfeatures for CloudWatch alarms\. The table includes the strings for `UsageType`, which can help you analyze and identify alarm\-related costs\. 


| *CloudWatch subfeature* | `UsageType` | 
| --- | --- | 
| Standard metric alarm | `AlarmMonitorUsage` | 
| High\-resolution metric alarm | `HighResAlarmMonitorUsage` | 
| Metrics Insights query alarm | `MetricInsightAlarmUsage` | 
| Composite alarm | `CompositeAlarmMonitorUsage` | 

**Reducing alarm costs**

 To optimize costs generated by metric math alarms that aggregate four or more metrics, you can aggregate data before the data is sent to CloudWatch\. This way, you can create an alarm for a single metric instead of an alarm that aggregates data for multiple metrics\. For more information, see [Publishing custom metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/publishingMetrics.html#publishingDataPoints1)\. 

 To optimize costs generated by Metrics Insights query alarms, you can ensure that the filter used for the query matches only the metrics that you want to monitor\. 

 The best way to reduce costs is to remove all unnecessary or unused alarms\. For example, you can delete alarms that evaluate metrics emitted by AWS resources that no longer exist\. 

***Example: Check for alarms in `INSUFFICIENT_DATA` state with `DescribeAlarms`***

 If you delete a resource, but not the metric alarms that the resource emits, the alarms still exist and typically will go into the `INSUFFICIENT_DATA` state\. To check for alarms that are in the `INSUFFICIENT_DATA` state, use the following AWS Command Line Interface \(AWS CLI\) command\. 

```
$ aws cloudwatch describe-alarms –state-value INSUFFICIENT_DATA
```

 Other ways to reduce costs include the following: 
+  Make sure to create alarms for the correct metrics\. 
+  Make sure that you don't have any alarms enabled in Regions where you're not working\. 
+  Remember that, although composite alarms reduce noise, they also generate additional costs\. 
+  When deciding whether to create a standard alarm or high\-resolution alarm, consider your use case and the value that each type of alarm brings\. 

### CloudWatch Logs<a name="w2aab5c19c11b7"></a>

 Amazon CloudWatch Logs has the following log types: 
+  ***Custom logs** \(logs that you create for your applications\)* 
+  ***Vended logs** \(logs that other AWS services, such as Amazon Virtual Private Cloud \(Amazon VPC\) and Amazon Route 53, create on your behalf\)* 

 For more information about vended logs, see [Enabling logging from certain AWS services](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AWS-logs-and-resource-policy.html) in the *Amazon CloudWatch Logs User Guide*\. 

 Custom and vended logs generate costs based on the number of logs that are *collected*, *stored*, and *analyzed*\. Separately, vended logs generate costs for delivery to Amazon S3 and Kinesis Data Firehose\. 

 The following table lists the names of the CloudWatch Logs features and names of relevant subfeatures\. The table includes the strings for `UsageType` and `Operation`, which can help you analyze and identify log\-related costs\. 


| CloudWatch Logs feature | *CloudWatch Logs subfeature* | `UsageType` | `Operation` | Purpose | 
| --- | --- | --- | --- | --- | 
| Custom logs | Collect \(ingest\) | `DataProcessing-Bytes` | `PutLogEvents` | Uploads a batch of logs to a specific log stream | 
|  | Store \(archive\) | `TimedStorage-ByteHrs` | `HourlyStorageMetering` | Stores logs per hour and logs per byte in CloudWatch Logs | 
|  | Analyze \(Logs Insights queries\) | `DataScanned-Bytes` | `StartQuery` | Logs data scanned by CloudWatch Logs Insights queries | 
| Vended logs | Delivery \(CloudWatch Logs\) | `VendedLog-Bytes` | `PutLogEvents` | Uploads a batch of logs to a specific log stream | 
|  |  *Delivery \(Amazon S3\)*  |  `S3-Egress-ComprBytes` `S3-Egress-Bytes`  | `LogDelivery` | Sends vended logs \(CloudWatch, Amazon S3, or Kinesis Data Firehose\) | 
|  | *Delivery \(Kinesis Data Firehose\)* | `FH-Egress-Bytes` | `LogDelivery` | Sends vended logs \(CloudWatch, Amazon S3, or Kinesis Data Firehose\) | 

 To analyze costs, use AWS Cost and Usage Reports with Athena, so that you can identify which logs are generating costs and determine how the costs are generated\. 

 ** * Example: Athena query * ** 

 You can use the following query to track which logs generate costs by resource ID\. 

```
SELECT
bill_payer_account_id as Payer,
line_item_usage_account_id as LinkedAccount,
line_item_resource_id AS ResourceID,
line_item_usage_type AS UsageType,
SUM(CAST(line_item_unblended_cost AS decimal(16,8))) AS TotalSpend,
SUM(CAST(line_item_usage_amount AS double)) AS UsageQuantity
FROM
costandusagereport 
WHERE
product_product_name = 'AmazonCloudWatch'
AND year='2022'
AND month='4'
AND line_item_operation IN ('PutLogEvents','HourlyStorageMetering','StartQuery','LogDelivery')
AND line_item_line_item_type NOT IN ('Tax','Credit','Refund','EdpDiscount','Fee','RIFee')
GROUP BY
bill_payer_account_id,
line_item_usage_account_id,
line_item_usage_type,
line_item_resource_id,
line_item_operation
ORDER BY
TotalSpend DESC
```

 To make the most of costs that are generated by CloudWatch Logs, consider the following: 
+  Log only the events that bring your business value\. This helps you generate fewer costs for ingestion\. 
+  Change your log retention settings, so that you generate fewer costs for storage\. For more information, see [Change log data retention in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#SettingLogRetention) in the *Amazon CloudWatch Logs User Guide*\. 
+  Run queries that CloudWatch Logs Insights automatically saves in your history\. This way, you generate fewer costs for analysis\. For more information, see [View running queries or query history](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CloudWatchLogs-Insights-Query-History.html) in the *Amazon CloudWatch Logs User Guide*\. 
+  Use the CloudWatch agent to collect system and application logs and send them to CloudWatch\. This way, you can collect only the log events that meet your criteria\. For more information, see [Amazon CloudWatch Agent adds Support for Log Filter Expressions](https://aws.amazon.com/about-aws/whats-new/2022/02/amazon-cloudwatch-agent-log-filter-expressions/)\. 

 To reduce costs for vended logs, consider your use case, and then determine whether your logs should be sent to CloudWatch or Amazon S3\. For more information, see [Logs sent to Amazon S3](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AWS-logs-and-resource-policy.html#AWS-logs-infrastructure-S3) in the *Amazon CloudWatch Logs User Guide*\. 

**Tip**  
 If you want to use metric filters, subscription filters, CloudWatch Logs Insights, and Contributor Insights, send vended logs to CloudWatch\.   
 Alternatively, if you're working with VPC Flow Logs and using them for auditing and compliance purposes, send vended logs to Amazon S3\.   
 For information about how to track charges that are generated by publishing VPC Flow Logs to S3 buckets, see [Using AWS Cost and Usage Reports and cost allocation tags to understand VPC FLow Logs data ingestion in Amazon S3](https://aws.amazon.com/blogs/mt/using-aws-cost-usage-reports-cost-allocation-tags-to-understand-vpc-flow-logs-data-ingestion-costs-in-amazon-s3/)\. 

 For additional information about how to make the most of costs that are generated by CloudWatch Logs, see [Which log group is causing a sudden increase in my CloudWatch Logs bill?](https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-logs-bill-increase/)\. 