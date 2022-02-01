# CloudWatch usage metrics<a name="CloudWatch-Usage-Metrics"></a>

CloudWatch collects metrics that track the usage of some AWS resources\. These metrics correspond to AWS service quotas\. Tracking these metrics can help you proactively manage your quotas\. For more information, see [Visualizing your service quotas and setting alarms](CloudWatch-Quotas-Visualize-Alarms.md)\.

Service quota usage metrics are in the `AWS/Usage` namespace and are collected every minute\.

Currently, the only metric name in this namespace that CloudWatch publishes is `CallCount`\. This metric is published with the dimensions `Resource`, `Service`, and `Type`\. The `Resource` dimension specifies the name of the API operation being tracked\. For example, the `CallCount` metric with the dimensions `"Service": "CloudWatch"`, `"Type": "API"` and `"Resource": "PutMetricData"` indicates the number of times the CloudWatch `PutMetricData` API operation has been called in your account\.

The `CallCount` metric does not have a specified unit\. The most useful statistic for the metric is `SUM`, which represents the total operation count for the 1\-minute period\.

**Metrics**


| Metric | Description | 
| --- | --- | 
|  `CallCount`  |  The number of specified operations performed in your account\.  | 

**Dimensions**


| Dimension | Description | 
| --- | --- | 
|  `Service`  |  The name of the AWS service containing the resource\. For CloudWatch usage metrics, the value for this dimension is `CloudWatch`\.  | 
|  `Class`  |  The class of resource being tracked\. CloudWatch API usage metrics use this dimension with a value of `None`\.  | 
|  `Type`  |  The type of resource being tracked\. Currently, when the `Service` dimension is `CloudWatch`, the only valid value for `Type` is `API`\.  | 
|  `Resource`  |  The name of the API operation\. Valid values include the following: DeleteAlarms, DeleteDashboards, DescribeAlarmHistory, DescribeAlarms, GetDashboard, GetMetricData, GetMetricStatistics, ListMetrics, PutDashboard, and PutMetricData  | 