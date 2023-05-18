# Use metric streams<a name="CloudWatch-Metric-Streams"></a>

You can use *metric streams* to continually stream CloudWatch metrics to a destination of your choice, with near\-real\-time delivery and low latency\. Supported destinations include AWS destinations such as Amazon Simple Storage Service and several third\-party service provider destinations\.

There are two main usage scenarios for CloudWatch metric streams:
+ **Data lake**— Create a metric stream and direct it to an Amazon Kinesis Data Firehose delivery stream that delivers your CloudWatch metrics to a data lake such as Amazon S3\. This enables you to continually update monitoring data, or to combine this CloudWatch metric data with billing and performance data to create rich datasets\. You can then use tools such as Amazon Athena to get insight into cost optimization, resource performance, and resource utilization\.
+ **Third\-party providers**— Use third\-party service providers to monitor, troubleshoot, and analyze your applications using the streamed CloudWatch data\.

You can stream all of your CloudWatch metrics, or use filters to stream only specified metrics\. Each metric stream can include up to 1000 filters that either include or exclude metric namespaces or specific metrics\. A single metric stream can have only include or exclude filters, but not both\.

After a metric stream is created, if new metrics are created that match the filters in place, the new metrics are automatically included in the stream\.

There is no limit on the number of metric streams per account or per Region, and no limit on the number of metric updates being streamed\.

Each stream can use either JSON format or OpenTelemetry 0\.7\.0 format\.

For metric streams in monitoring accounts, you can choose whether to include metrics from the source accounts linked to that monitoring account\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\.

Metric streams always include the `Minimum`, `Maximum`, `Sample count`, and `Sum` statistics\. You can also choose to include additional statistics at an additional charge\. For more information, see [Statistics that can be streamed](CloudWatch-metric-streams-statistics.md)\. 

Metric streams pricing is based on the number of metric updates\. You also incur charges from Kinesis Data Firehose for the delivery stream used for the metric stream\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

**Topics**
+ [Set up a metric stream](CloudWatch-metric-streams-setup.md)
+ [Statistics that can be streamed](CloudWatch-metric-streams-statistics.md)
+ [Metric stream operation and maintenance](CloudWatch-metric-streams-operation.md)
+ [Monitor your metric streams with CloudWatch metrics](CloudWatch-metric-streams-monitoring.md)
+ [Trust between CloudWatch and Kinesis Data Firehose](CloudWatch-metric-streams-trustpolicy.md)
+ [Metric streams output formats](CloudWatch-metric-streams-formats.md)
+ [Troubleshooting](CloudWatch-metric-streams-troubleshoot.md)