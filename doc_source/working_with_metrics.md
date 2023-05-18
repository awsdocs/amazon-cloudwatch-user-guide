# Use Amazon CloudWatch metrics<a name="working_with_metrics"></a>

Metrics are data about the performance of your systems\. By default, many services provide free metrics for resources \(such as Amazon EC2 instances, Amazon EBS volumes, and Amazon RDS DB instances\)\. You can also enable detailed monitoring for some resources, such as your Amazon EC2 instances, or publish your own application metrics\. Amazon CloudWatch can load all the metrics in your account \(both AWS resource metrics and application metrics that you provide\) for search, graphing, and alarms\.

Metric data is kept for 15 months, enabling you to view both up\-to\-the\-minute data and historical data\.

To graph metrics in the console, you can use CloudWatch Metrics Insights, a high\-performance SQL query engine that you can use to identify trends and patterns within all your metrics in real time\. 

**Topics**
+ [Basic monitoring and detailed monitoring](cloudwatch-metrics-basic-detailed.md)
+ [Query your metrics with CloudWatch Metrics Insights](query_with_cloudwatch-metrics-insights.md)
+ [Use metrics explorer to monitor resources by their tags and properties](CloudWatch-Metrics-Explorer.md)
+ [Use metric streams](CloudWatch-Metric-Streams.md)
+ [View available metrics](viewing_metrics_with_cloudwatch.md)
+ [Graphing metrics](graph_metrics.md)
+ [Using CloudWatch anomaly detection](CloudWatch_Anomaly_Detection.md)
+ [Use metric math](using-metric-math.md)
+ [Use search expressions in graphs](using-search-expressions.md)
+ [Get statistics for a metric](getting-metric-statistics.md)
+ [Publish custom metrics](publishingMetrics.md)