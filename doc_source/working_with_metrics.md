# Using Amazon CloudWatch metrics<a name="working_with_metrics"></a>

Metrics are data about the performance of your systems\. By default, many services provide free metrics for resources \(such as Amazon EC2 instances, Amazon EBS volumes, and Amazon RDS DB instances\)\. You can also enable detailed monitoring for some resources, such as your Amazon EC2 instances, or publish your own application metrics\. Amazon CloudWatch can load all the metrics in your account \(both AWS resource metrics and application metrics that you provide\) for search, graphing, and alarms\.

Metric data is kept for 15 months, enabling you to view both up\-to\-the\-minute data and historical data\.

To graph metrics in the console, you can use CloudWatch Metrics Insights, a high\-performance SQL query engine that you can use to identify trends and patterns within all your metrics in real time\. 

**Topics**
+ [Query your metrics with CloudWatch Metrics Insights](query_with_cloudwatch-metrics-insights.md)
+ [Viewing available metrics](viewing_metrics_with_cloudwatch.md)
+ [Searching for available metrics](finding_metrics_with_cloudwatch.md)
+ [Getting statistics for a metric](getting-metric-statistics.md)
+ [Graphing metrics](graph_metrics.md)
+ [Publishing custom metrics](publishingMetrics.md)
+ [Using metric math](using-metric-math.md)
+ [Using search expressions in graphs](using-search-expressions.md)