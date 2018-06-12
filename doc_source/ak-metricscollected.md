# Amazon Kinesis Data Streams Metrics and Dimensions<a name="ak-metricscollected"></a>

Kinesis Data Streams sends metrics to CloudWatch at two levels; the stream level and, optionally, the shard level\. Stream\-level metrics are for most common monitoring use cases in normal conditions\. Shard\-level metrics are for specific monitoring tasks, usually related to troubleshooting\. For more information, see [Monitoring Amazon Kinesis with Amazon CloudWatch](http://docs.aws.amazon.com/kinesis/latest/dev/monitoring-with-cloudwatch.html) in the *Amazon Kinesis Developer Guide*\.

**Topics**
+ [Basic Stream\-level Metrics](#kinesis-metrics-stream)
+ [Enhanced Shard\-level Metrics](#kinesis-metrics-shard)
+ [Dimensions for Amazon Kinesis Metrics](#ak-metric-dimensions)

## Basic Stream\-level Metrics<a name="kinesis-metrics-stream"></a>

The `AWS/Kinesis` namespace includes the following stream\-level metrics\.

Kinesis Data Streams sends these stream\-level metrics to CloudWatch every minute\. These metrics are always available\.


| Metric | Description | 
| --- | --- | 
| GetRecords\.Bytes |  The number of bytes retrieved from the Kinesis stream, measured over the specified time period\. Minimum, Maximum, and Average statistics represent the bytes in a single `GetRecords` operation for the stream in the specified time period\. Shard\-level metric name: `OutgoingBytes` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Bytes  | 
| GetRecords\.IteratorAge |  This metric is deprecated\. Use `GetRecords.IteratorAgeMilliseconds`\.  | 
| GetRecords\.IteratorAgeMilliseconds |  The age of the last record in all `GetRecords` calls made against an Kinesis stream, measured over the specified time period\. Age is the difference between the current time and when the last record of the `GetRecords` call was written to the stream\. The Minimum and Maximum statistics can be used to track the progress of Kinesis consumer applications\. A value of zero indicates that the records being read are completely caught up with the stream\. Shard\-level metric name: `IteratorAgeMilliseconds` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Samples Units: Milliseconds  | 
| GetRecords\.Latency |  The time taken per `GetRecords` operation, measured over the specified time period\. Dimensions: StreamName Statistics: Minimum, Maximum, Average Units: Milliseconds  | 
| GetRecords\.Records |  The number of records retrieved from the shard, measured over the specified time period\. Minimum, Maximum, and Average statistics represent the records in a single `GetRecords` operation for the stream in the specified time period\. Shard\-level metric name: `OutgoingRecords` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| GetRecords\.Success |  The number of successful `GetRecords` operations per stream, measured over the specified time period\. Dimensions: StreamName Statistics: Average, Sum, Samples Units: Count  | 
| IncomingBytes |  The number of bytes successfully put to the Kinesis stream over the specified time period\. This metric includes bytes from `PutRecord` and `PutRecords` operations\. Minimum, Maximum, and Average statistics represent the bytes in a single put operation for the stream in the specified time period\. Shard\-level metric name: `IncomingBytes` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Bytes  | 
| IncomingRecords |  The number of records successfully put to the Kinesis stream over the specified time period\. This metric includes record counts from `PutRecord` and `PutRecords` operations\. Minimum, Maximum, and Average statistics represent the records in a single put operation for the stream in the specified time period\. Shard\-level metric name: `IncomingRecords` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| PutRecord\.Bytes |  The number of bytes put to the Kinesis stream using the `PutRecord` operation over the specified time period\. Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Bytes  | 
| PutRecord\.Latency |  The time taken per `PutRecord` operation, measured over the specified time period\. Dimensions: StreamName Statistics: Minimum, Maximum, Average Units: Milliseconds  | 
| PutRecord\.Success |  The number of successful `PutRecord` operations per Kinesis stream, measured over the specified time period\. Average reflects the percentage of successful writes to a stream\. Dimensions: StreamName Statistics: Average, Sum, Samples Units: Count  | 
| PutRecords\.Bytes |  The number of bytes put to the Kinesis stream using the `PutRecords` operation over the specified time period\. Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Bytes  | 
| PutRecords\.Latency |  The time taken per `PutRecords` operation, measured over the specified time period\. Dimensions: StreamName Statistics: Minimum, Maximum, Average Units: Milliseconds  | 
| PutRecords\.Records |  The number of successful records in a `PutRecords` operation per Kinesis stream, measured over the specified time period\. Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| PutRecords\.Success |  The number of `PutRecords` operations where at least one record succeeded, per Kinesis stream, measured over the specified time period\. Dimensions: StreamName Statistics: Average, Sum, Samples Units: Count  | 
| ReadProvisionedThroughputExceeded |  The number of `GetRecords` calls throttled for the stream over the specified time period\. The most commonly used statistic for this metric is Average\. When the Minimum statistic has a value of 1, all records were throttled for the stream during the specified time period\.  When the Maximum statistic has a value of 0 \(zero\), no records were throttled for the stream during the specified time period\. Shard\-level metric name: `ReadProvisionedThroughputExceeded` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| WriteProvisionedThroughputExceeded |  The number of records rejected due to throttling for the stream over the specified time period\. This metric includes throttling from `PutRecord` and `PutRecords` operations\. The most commonly used statistic for this metric is Average\. When the Minimum statistic has a non\-zero value, records were being throttled for the stream during the specified time period\.  When the Maximum statistic has a value of 0 \(zero\), no records were being throttled for the stream during the specified time period\. Shard\-level metric name: `WriteProvisionedThroughputExceeded` Dimensions: StreamName Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 

## Enhanced Shard\-level Metrics<a name="kinesis-metrics-shard"></a>

The `AWS/Kinesis` namespace includes the following shard\-level metrics\.

Kinesis sends the following shard\-level metrics to CloudWatch every minute\. These metrics are not enabled by default\. There is a charge for enhanced metrics emitted from Kinesis\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/) under the heading *Amazon CloudWatch Custom Metrics*\. The charges are given per shard per metric per month\.


| Metric | Description | 
| --- | --- | 
| IncomingBytes |  The number of bytes successfully put to the shard over the specified time period\. This metric includes bytes from `PutRecord` and `PutRecords` operations\. Minimum, Maximum, and Average statistics represent the bytes in a single put operation for the shard in the specified time period\. Stream\-level metric name: `IncomingBytes` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Sum, Samples Units: Bytes  | 
| IncomingRecords |  The number of records successfully put to the shard over the specified time period\. This metric includes record counts from `PutRecord` and `PutRecords` operations\. Minimum, Maximum, and Average statistics represent the records in a single put operation for the shard in the specified time period\. Stream\-level metric name: `IncomingRecords` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| IteratorAgeMilliseconds |  The age of the last record in all `GetRecords` calls made against a shard, measured over the specified time period\. Age is the difference between the current time and when the last record of the `GetRecords` call was written to the stream\. The Minimum and Maximum statistics can be used to track the progress of Kinesis consumer applications\. A value of 0 \(zero\) indicates that the records being read are completely caught up with the stream\. Stream\-level metric name: `GetRecords.IteratorAgeMilliseconds` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Samples Units: Milliseconds  | 
| OutgoingBytes |  The number of bytes retrieved from the shard, measured over the specified time period\. Minimum, Maximum, and Average statistics represent the bytes in a single `GetRecords` operation for the shard in the specified time period\. Stream\-level metric name: `GetRecords.Bytes` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Sum, Samples Units: Bytes  | 
| OutgoingRecords |  The number of records retrieved from the shard, measured over the specified time period\. Minimum, Maximum, and Average statistics represent the records in a single `GetRecords` operation for the shard in the specified time period\. Stream\-level metric name: `GetRecords.Records` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| ReadProvisionedThroughputExceeded |  The number of `GetRecords` calls throttled for the shard over the specified time period\. This exception count covers all dimensions of the following limits: 5 reads per shard per second or 2 MB per second per shard\. The most commonly used statistic for this metric is Average\. When the Minimum statistic has a value of 1, all records were throttled for the shard during the specified time period\.  When the Maximum statistic has a value of 0 \(zero\), no records were throttled for the shard during the specified time period\. Stream\-level metric name: `ReadProvisionedThroughputExceeded` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 
| WriteProvisionedThroughputExceeded |  The number of records rejected due to throttling for the shard over the specified time period\. This metric includes throttling from `PutRecord` and `PutRecords` operations and covers all dimensions of the following limits: 1,000 records per second per shard or 1 MB per second per shard\. The most commonly used statistic for this metric is Average\. When the Minimum statistic has a non\-zero value, records were being throttled for the shard during the specified time period\.  When the Maximum statistic has a value of 0 \(zero\), no records were being throttled for the shard during the specified time period\. Stream\-level metric name: `WriteProvisionedThroughputExceeded` Dimensions: StreamName, ShardId Statistics: Minimum, Maximum, Average, Sum, Samples Units: Count  | 

## Dimensions for Amazon Kinesis Metrics<a name="ak-metric-dimensions"></a>

You can use the following dimensions to filter the metrics for Amazon Kinesis Data Streams\.


| Dimension | Description | 
| --- | --- | 
|  `StreamName`  |  The name of the Kinesis stream\.  | 
|  `ShardId`  |  The shard ID within the Kinesis stream\.  | 