# Statistics that can be streamed<a name="CloudWatch-metric-streams-statistics"></a>

Metric streams always include the following statistics: `Minimum`, `Maximum`, `Sample count`, and `Sum`\. You can also choose to include the following additional statistics in a metric stream\. This choice is on a per\-metric basis\. For more information about these statistics, see [CloudWatch statistics definitions](Statistics-definitions.md)\.
+ Percentile values such as p95 or p99 \(For streams with either JSON or OpenTelemetry format\) 
+ Trimmed mean \(Only for streams with the JSON format\)
+ Winsorized mean \(Only for streams with the JSON format\)
+ Trimmed count \(Only for streams with the JSON format\)
+ Trimmed sum \(Only for streams with the JSON format\)
+ Percentile rank \(Only for streams with the JSON format\)
+ Interquartile mean \(Only for streams with the JSON format\)

Streaming additional statistics incurs additional charges\. Streaming between one and five of these additional statistics for a particular metric is billed as an additional metric update\. Thereafter, each additional set of up to five of these statistics is billed as another metric update\. 

 For example, suppose that for one metric you are streaming the following six additional statistics: p95, p99, p99\.9, Trimmed mean, Winsorized mean, and Trimmed sum\. Each update of this metric is billed as three metric updates: one for the metric update which includes the default statistics, one for the first five additional statistics, and one for the sixth additional statistic\. Adding up to four more additional statistics for a total of ten would not increase the billing, but an eleventh additional statistic would do so\.

When you specify a metric name and namespace combination to stream additional statistics, all dimension combinations of that metric name and namespace are streamed with the additional statistics\. 

CloudWatch metric streams publishes a new metric, `TotalMetricUpdate`, which reflects the base number of metric updates plus extra metric updates incurred by streaming additional statistics\. For more information, see [Monitor your metric streams with CloudWatch metrics](CloudWatch-metric-streams-monitoring.md)\.

For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

**Note**  
Some metrics do not support percentiles\. Percentile statistics for these metrics are excluded from the stream and do not incur metric stream charges\. An example of these statistics that do not support percentiles are some metrics in the `AWS/ECS` namespace\.

The additional statistics that you configure are streamed only if they match the filters for the stream\. For example, if you create a stream that has only `EC2` and `RDS` in the include filters, and then your statistics configuration lists `EC2` and `Lambda`, then the stream includes `EC2` metrics with additional statistics, `RDS` metrics with only the default statistics, and doesn't include `Lambda` statistics at all\.