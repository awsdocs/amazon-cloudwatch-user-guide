# CloudWatch Limits<a name="cloudwatch_limits"></a>

CloudWatch has the following limits:


| Resource | Default Limit | 
| --- | --- | 
|  Actions  |  5/alarm\. This limit cannot be changed\.   | 
|  Alarms  |  10/month/customer for free\. 5000 per region per account\.  | 
|  API requests  |  1,000,000/month/customer for free\.  | 
|  Custom metrics  |  No limit\.  | 
|  Dashboards  |  Up to 1000 dashboards per account\. Up to 100 metrics per dashboard widget\. Up to 500 metrics per dashboard, across all widgets\. These limits cannot be changed\.  | 
|  [DescribeAlarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarms.html)  |  9 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  Dimensions  |  10/metric\. This limit cannot be changed\.  | 
|  [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)  |  50 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\.  180,000 Datapoints Per Second \(DPS\) if the `StartTime` used in the API request is less than or equal to three hours from current time\. 90,000 DPS if the `StartTime` is more than three hours from current time\. This is the maximum number of datapoints you can request per second using one or more API calls without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch) for both of these limits\.  | 
|  [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)  |  A single `GetMetricData` call can include as many as 100 `MetricDataQuery` structures\. This limit cannot be changed\.  | 
|  [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html)  |  400 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  [ListMetrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_ListMetrics.html)  |  25 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  Metric data  |  15 months\. This limit cannot be changed\.  | 
|  [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html) items  |  20/[PutMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) request\. A [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html) object can contain a single value or a [StatisticSet](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_StatisticSet.html) object representing many values\. This limit cannot be changed\.  | 
|  Metrics  |  10/month/customer for free\.  | 
|  Period  |  Maximum value is one day \(86,400 seconds\)\. This limit cannot be changed\.  | 
|  [PutMetricAlarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html) request  |  3 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  [PutMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) request  |  40 KB for HTTP POST requests\. [PutMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) can handle 150 transactions per second \(TPS\), which is the maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  Amazon SNS email notifications  |  1,000/month/customer for free\.  | 