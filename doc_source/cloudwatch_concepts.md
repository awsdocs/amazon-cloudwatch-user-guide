# Amazon CloudWatch concepts<a name="cloudwatch_concepts"></a>

The following terminology and concepts are central to your understanding and use of Amazon CloudWatch:
+ [Namespaces](#Namespace)
+ [Metrics](#Metric)
+ [Dimensions](#Dimension)
+ [Resolution](#Resolution_definition)
+ [Statistics](#Statistic)
+ [Percentiles](#Percentiles)
+ [Alarms](#CloudWatchAlarms)

 For information about the service quotas for CloudWatch metrics, alarms, API requests, and alarm email notifications, see [CloudWatch service quotas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_limits.html)\. 

## Namespaces<a name="Namespace"></a>

A *namespace* is a container for CloudWatch metrics\. Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics\.

There is no default namespace\. You must specify a namespace for each data point you publish to CloudWatch\. You can specify a namespace name when you create a metric\. These names must contain valid ASCII characters, and be 255 or fewer characters\. Possible characters are: alphanumeric characters \(0\-9A\-Za\-z\), period \(\.\), hyphen \(\-\), underscore \(\_\), forward slash \(/\), hash \(\#\), and colon \(:\)\. A namespace must contain at least one non\-whitespace character\.

The AWS namespaces typically use the following naming convention: `AWS/service`\. For example, Amazon EC2 uses the `AWS/EC2` namespace\. For the list of AWS namespaces, see [AWS services that publish CloudWatch metrics](aws-services-cloudwatch-metrics.md)\.

## Metrics<a name="Metric"></a>

*Metrics* are the fundamental concept in CloudWatch\. A metric represents a time\-ordered set of data points that are published to CloudWatch\. Think of a metric as a variable to monitor, and the data points as representing the values of that variable over time\. For example, the CPU usage of a particular EC2 instance is one metric provided by Amazon EC2\. The data points themselves can come from any application or business activity from which you collect data\.

By default, many AWS services provide metrics at no charge for resources \(such as Amazon EC2 instances, Amazon EBS volumes, and Amazon RDS DB instances\)\. For a charge, you can also enable detailed monitoring for some resources, such as your Amazon EC2 instances, or publish your own application metrics\. For custom metrics, you can add the data points in any order, and at any rate you choose\. You can retrieve statistics about those data points as an ordered set of time\-series data\.

Metrics exist only in the Region in which they are created\. Metrics cannot be deleted, but they automatically expire after 15 months if no new data is published to them\. Data points older than 15 months expire on a rolling basis; as new data points come in, data older than 15 months is dropped\.

Metrics are uniquely defined by a name, a namespace, and zero or more dimensions\. Each data point in a metric has a time stamp, and \(optionally\) a unit of measure\. You can retrieve statistics from CloudWatch for any metric\.

For more information, see [View available metrics](viewing_metrics_with_cloudwatch.md) and [Publish custom metrics](publishingMetrics.md)\.

### Time stamps<a name="about_timestamp"></a>

Each metric data point must be associated with a time stamp\. The time stamp can be up to two weeks in the past and up to two hours into the future\. If you do not provide a time stamp, CloudWatch creates a time stamp for you based on the time the data point was received\. 

Time stamps are `dateTime` objects, with the complete date plus hours, minutes, and seconds \(for example, 2016\-10\-31T23:59:59Z\)\. For more information, see [dateTime](http://www.w3.org/TR/xmlschema-2/#dateTime)\. Although it is not required, we recommend that you use Coordinated Universal Time \(UTC\)\. When you retrieve statistics from CloudWatch, all times are in UTC\.

CloudWatch alarms check metrics based on the current time in UTC\. Custom metrics sent to CloudWatch with time stamps other than the current UTC time can cause alarms to display the **Insufficient Data** state or result in delayed alarms\.

### Metrics retention<a name="metrics-retention"></a>

CloudWatch retains metric data as follows:
+ Data points with a period of less than 60 seconds are available for 3 hours\. These data points are high\-resolution custom metrics\. 
+ Data points with a period of 60 seconds \(1 minute\) are available for 15 days
+ Data points with a period of 300 seconds \(5 minute\) are available for 63 days
+ Data points with a period of 3600 seconds \(1 hour\) are available for 455 days \(15 months\)

Data points that are initially published with a shorter period are aggregated together for long\-term storage\. For example, if you collect data using a period of 1 minute, the data remains available for 15 days with 1\-minute resolution\. After 15 days this data is still available, but is aggregated and is retrievable only with a resolution of 5 minutes\. After 63 days, the data is further aggregated and is available with a resolution of 1 hour\. 

**Note**  
Metrics that have not had any new data points in the past two weeks do not appear in the console\. They also do not appear when you type their metric name or dimension names in the search box in the **All metrics** tab in the console, and they are not returned in the results of a [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command\. The best way to retrieve these metrics is with the [get\-metric\-data](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-data.html) or [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) commands in the AWS CLI\.

## Dimensions<a name="Dimension"></a>

A *dimension* is a name/value pair that is part of the identity of a metric\. You can assign up to 30 dimensions to a metric\.

Every metric has specific characteristics that describe it, and you can think of dimensions as categories for those characteristics\. Dimensions help you design a structure for your statistics plan\. Because dimensions are part of the unique identifier for a metric, whenever you add a unique name/value pair to one of your metrics, you are creating a new variation of that metric\.

AWS services that send data to CloudWatch attach dimensions to each metric\. You can use dimensions to filter the results that CloudWatch returns\. For example, you can get statistics for a specific EC2 instance by specifying the `InstanceId` dimension when you search for metrics\.

For metrics produced by certain AWS services, such as Amazon EC2, CloudWatch can aggregate data across dimensions\. For example, if you search for metrics in the `AWS/EC2` namespace but do not specify any dimensions, CloudWatch aggregates all data for the specified metric to create the statistic that you requested\. CloudWatch does not aggregate across dimensions for your custom metrics\.

### Dimension combinations<a name="dimension-combinations"></a>

CloudWatch treats each unique combination of dimensions as a separate metric, even if the metrics have the same metric name\. You can only retrieve statistics using combinations of dimensions that you specifically published\. When you retrieve statistics, specify the same values for the namespace, metric name, and dimension parameters that were used when the metrics were created\. You can also specify the start and end times for CloudWatch to use for aggregation\.

For example, suppose that you publish four distinct metrics named ServerStats in the DataCenterMetric namespace with the following properties:

```
Dimensions: Server=Prod, Domain=Frankfurt, Unit: Count, Timestamp: 2016-10-31T12:30:00Z, Value: 105
Dimensions: Server=Beta, Domain=Frankfurt, Unit: Count, Timestamp: 2016-10-31T12:31:00Z, Value: 115
Dimensions: Server=Prod, Domain=Rio,       Unit: Count, Timestamp: 2016-10-31T12:32:00Z, Value: 95
Dimensions: Server=Beta, Domain=Rio,       Unit: Count, Timestamp: 2016-10-31T12:33:00Z, Value: 97
```

If you publish only those four metrics, you can retrieve statistics for these combinations of dimensions:
+ `Server=Prod,Domain=Frankfurt`
+ `Server=Prod,Domain=Rio`
+ `Server=Beta,Domain=Frankfurt`
+ `Server=Beta,Domain=Rio`

You can't retrieve statistics for the following dimensions or if you specify no dimensions\. \(The exception is by using the metric math **SEARCH** function, which can retrieve statistics for multiple metrics\. For more information, see [Use search expressions in graphs](using-search-expressions.md)\.\)
+ `Server=Prod`
+ `Server=Beta`
+ `Domain=Frankfurt`
+ `Domain=Rio`

## Resolution<a name="Resolution_definition"></a>

Each metric is one of the following:
+ Standard resolution, with data having a one\-minute granularity
+ High resolution, with data at a granularity of one second

Metrics produced by AWS services are standard resolution by default\. When you publish a custom metric, you can define it as either standard resolution or high resolution\. When you publish a high\-resolution metric, CloudWatch stores it with a resolution of 1 second, and you can read and retrieve it with a period of 1 second, 5 seconds, 10 seconds, 30 seconds, or any multiple of 60 seconds\.

High\-resolution metrics can give you more immediate insight into your application's sub\-minute activity\. Keep in mind that every `PutMetricData` call for a custom metric is charged, so calling `PutMetricData` more often on a high\-resolution metric can lead to higher charges\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

If you set an alarm on a high\-resolution metric, you can specify a high\-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds\. There is a higher charge for high\-resolution alarms with a period of 10 or 30 seconds\.

## Statistics<a name="Statistic"></a>

*Statistics* are metric data aggregations over specified periods of time\. CloudWatch provides statistics based on the metric data points provided by your custom data or provided by other AWS services to CloudWatch\. Aggregations are made using the namespace, metric name, dimensions, and the data point unit of measure, within the time period you specify\.

For detailed definitions of the statistics supported by CloudWatch, see [CloudWatch statistics definitions](Statistics-definitions.md)\.

## Units<a name="Unit"></a>

Each statistic has a unit of measure\. Example units include `Bytes`, `Seconds`, `Count`, and `Percent`\. For the complete list of the units that CloudWatch supports, see the [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html) data type in the *Amazon CloudWatch API Reference*\.

You can specify a unit when you create a custom metric\. If you do not specify a unit, CloudWatch uses `None` as the unit\. Units help provide conceptual meaning to your data\. Though CloudWatch attaches no significance to a unit internally, other applications can derive semantic information based on the unit\.

Metric data points that specify a unit of measure are aggregated separately\. When you get statistics without specifying a unit, CloudWatch aggregates all data points of the same unit together\. If you have two otherwise identical metrics with different units, two separate data streams are returned, one for each unit\.

## Periods<a name="CloudWatchPeriods"></a>

A *period* is the length of time associated with a specific Amazon CloudWatch statistic\. Each statistic represents an aggregation of the metrics data collected for a specified period of time\. Periods are defined in numbers of seconds, and valid values for period are 1, 5, 10, 30, or any multiple of 60\. For example, to specify a period of six minutes, use 360 as the period value\. You can adjust how the data is aggregated by varying the length of the period\. A period can be as short as one second or as long as one day \(86,400 seconds\)\. The default value is 60 seconds\.

Only custom metrics that you define with a storage resolution of 1 second support sub\-minute periods\. Even though the option to set a period below 60 is always available in the console, you should select a period that aligns to how the metric is stored\. For more information about metrics that support sub\-minute periods, see [High\-resolution metrics](publishingMetrics.md#high-resolution-metrics)\.

When you retrieve statistics, you can specify a period, start time, and end time\. These parameters determine the overall length of time associated with the statistics\. The default values for the start time and end time get you the last hour's worth of statistics\. The values that you specify for the start time and end time determine how many periods CloudWatch returns\. For example, retrieving statistics using the default values for the period, start time, and end time returns an aggregated set of statistics for each minute of the previous hour\. If you prefer statistics aggregated in ten\-minute blocks, specify a period of 600\. For statistics aggregated over the entire hour, specify a period of 3600\.

When statistics are aggregated over a period of time, they are stamped with the time corresponding to the beginning of the period\. For example, data aggregated from 7:00pm to 8:00pm is stamped as 7:00pm\. Additionally, data aggregated between 7:00pm and 8:00pm begins to be visible at 7:00pm, then the values of that aggregated data may change as CloudWatch collects more samples during the period\.

Periods are also important for CloudWatch alarms\. When you create an alarm to monitor a specific metric, you are asking CloudWatch to compare that metric to the threshold value that you specified\. You have extensive control over how CloudWatch makes that comparison\. Not only can you specify the period over which the comparison is made, but you can also specify how many evaluation periods are used to arrive at a conclusion\. For example, if you specify three evaluation periods, CloudWatch compares a window of three data points\. CloudWatch only notifies you if the oldest data point is breaching and the others are breaching or missing\.

## Aggregation<a name="CloudWatchAggregation"></a>

Amazon CloudWatch aggregates statistics according to the period length that you specify when retrieving statistics\. You can publish as many data points as you want with the same or similar time stamps\. CloudWatch aggregates them according to the specified period length\. CloudWatch does not automatically aggregate data across Regions, but you can use metric math to aggregate metrics from different Regions\.

You can publish data points for a metric that share not only the same time stamp, but also the same namespace and dimensions\. CloudWatch returns aggregated statistics for those data points\. You can also publish multiple data points for the same or different metrics, with any time stamp\.

For large datasets, you can insert a pre\-aggregated dataset called a *statistic set*\. With statistic sets, you give CloudWatch the Min, Max, Sum, and SampleCount for a number of data points\. This is commonly used when you need to collect data many times in a minute\. For example, suppose you have a metric for the request latency of a webpage\. It doesn't make sense to publish data with every webpage hit\. We suggest that you collect the latency of all hits to that webpage, aggregate them once a minute, and send that statistic set to CloudWatch\.

Amazon CloudWatch doesn't differentiate the source of a metric\. If you publish a metric with the same namespace and dimensions from different sources, CloudWatch treats this as a single metric\. This can be useful for service metrics in a distributed, scaled system\. For example, all the hosts in a web server application could publish identical metrics representing the latency of requests they are processing\. CloudWatch treats these as a single metric, allowing you to get the statistics for minimum, maximum, average, and sum of all requests across your application\.

## Percentiles<a name="Percentiles"></a>

A *percentile* indicates the relative standing of a value in a dataset\. For example, the 95th percentile means that 95 percent of the data is lower than this value and 5 percent of the data is higher than this value\. Percentiles help you get a better understanding of the distribution of your metric data\.

Percentiles are often used to isolate anomalies\. In a normal distribution, 95 percent of the data is within two standard deviations from the mean and 99\.7 percent of the data is within three standard deviations from the mean\. Any data that falls outside three standard deviations is often considered to be an anomaly because it differs so greatly from the average value\. For example, suppose that you are monitoring the CPU utilization of your EC2 instances to ensure that your customers have a good experience\. If you monitor the average, this can hide anomalies\. If you monitor the maximum, a single anomaly can skew the results\. Using percentiles, you can monitor the 95th percentile of CPU utilization to check for instances with an unusually heavy load\.

Some CloudWatch metrics support percentiles as a statistic\. For these metrics, you can monitor your system and applications using percentiles as you would when using the other CloudWatch statistics \(Average, Minimum, Maximum, and Sum\)\. For example, when you create an alarm, you can use percentiles as the statistical function\. You can specify the percentile with up to ten decimal places \(for example, p95\.0123456789\)\.

Percentile statistics are available for custom metrics as long as you publish the raw, unsummarized data points for your custom metric\. Percentile statistics are not available for metrics when any of the metric values are negative numbers\.

CloudWatch needs raw data points to calculate percentiles\. If you publish data using a statistic set instead, you can only retrieve percentile statistics for this data if one of the following conditions is true:
+ The SampleCount value of the statistic set is 1 and Min, Max, and Sum are all equal\.
+ The Min and Max are equal, and Sum is equal to Min multiplied by SampleCount\.

The following AWS services include metrics that support percentile statistics\.
+ API Gateway
+ Application Load Balancer
+ Amazon EC2
+ Elastic Load Balancing
+ Kinesis
+ Amazon RDS

CloudWatch also supports trimmed mean and other performance statistics, which can have a similar use as percentiles\. For more information, see [CloudWatch statistics definitions](Statistics-definitions.md)\.

## Alarms<a name="CloudWatchAlarms"></a>

You can use an *alarm* to automatically initiate actions on your behalf\. An alarm watches a single metric over a specified time period, and performs one or more specified actions, based on the value of the metric relative to a threshold over time\. The action is a notification sent to an Amazon SNS topic or an Auto Scaling policy\. You can also add alarms to dashboards\.

Alarms invoke actions for sustained state changes only\. CloudWatch alarms do not invoke actions simply because they are in a particular state\. The state must have changed and been maintained for a specified number of periods\.

When creating an alarm, select an alarm monitoring period that is greater than or equal to the metric's resolution\. For example, basic monitoring for Amazon EC2 provides metrics for your instances every 5 minutes\. When setting an alarm on a basic monitoring metric, select a period of at least 300 seconds \(5 minutes\)\. Detailed monitoring for Amazon EC2 provides metrics for your instances with a resolution of 1 minute\. When setting an alarm on a detailed monitoring metric, select a period of at least 60 seconds \(1 minute\)\.

 If you set an alarm on a high\-resolution metric, you can specify a high\-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds\. There is a higher charge for high\-resolution alarms\. For more information about high\-resolution metrics, see [Publish custom metrics](publishingMetrics.md)\.

For more information, see [ Using Amazon CloudWatch alarms](AlarmThatSendsEmail.md) and [Create an alarm from a metric on a graph](create_alarm_metric_graph.md)\.