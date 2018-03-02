# CloudWatch Limits<a name="cloudwatch_limits"></a>

CloudWatch has the following limits:


| Resource | Default Limit | 
| --- | --- | 
|  Actions  |  5/alarm\. This limit cannot be changed\.   | 
|  Alarms  |  10/month/customer for free\. 5000 per region per account\.  | 
|  API requests  |  1,000,000/month/customer for free\.  | 
|  Custom metrics  |  No limit\.  | 
|  [DescribeAlarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarms.html)  |  9 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  Dimensions  |  10/metric\. This limit cannot be changed\.  | 
|  [GetMetricStatistics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html)  |  400 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  [ListMetrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_ListMetrics.html)  |  25 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  Metric data  |  15 months\. This limit cannot be changed\.  | 
|  [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html) items  |  20/[PutMetricData](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) request\. A [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html) object can contain a single value or a [StatisticSet](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_StatisticSet.html) object representing many values\. This limit cannot be changed\.  | 
|  Metrics  |  10/month/customer for free\.  | 
|  Period  |  Maximum value is one day \(86,400 seconds\)\. This limit cannot be changed\.  | 
|  [PutMetricAlarm](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html) request  |  3 transactions per second \(TPS\)\. The maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  [PutMetricData](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) request  |  40 KB for HTTP POST requests\. [PutMetricData](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricData.html) can handle 150 transactions per second \(TPS\), which is the maximum number of operation requests you can make per second without being throttled\. You can [request a limit increase](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-amazon-cloudwatch)\.  | 
|  Amazon SNS email notifications  |  1,000/month/customer for free\.  | 