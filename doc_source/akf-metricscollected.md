# Amazon Kinesis Data Firehose Metrics<a name="akf-metricscollected"></a>

Kinesis Data Firehose sends metrics to CloudWatch\. For more information, see [Monitoring with Amazon CloudWatch Metrics](https://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudwatch-metrics.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

## Service\-level CloudWatch Metrics<a name="fh-metrics-cw"></a>

The `AWS/Firehose` namespace includes the following service\-level metrics\.


| Metric | Description | 
| --- | --- | 
| BackupToS3\.Bytes |  The number of bytes delivered to Amazon S3 for backup over the specified time period\. Kinesis Data Firehose emits this metric when data transformation is enabled for Amazon S3 or Amazon Redshift destinations\. Units: Bytes  | 
| BackupToS3\.DataFreshness |  Age \(from getting into Kinesis Data Firehose to now\) of the oldest record in Kinesis Data Firehose\. Any record older than this age has been delivered to the Amazon S3 bucket for backup\. Kinesis Data Firehose emits this metric when data transformation is enabled for Amazon S3 or Amazon Redshift destinations\. Units: Seconds  | 
| BackupToS3\.Records |  The number of records delivered to Amazon S3 for backup over the specified time period\. Kinesis Data Firehose emits this metric when data transformation is enabled for Amazon S3 or Amazon Redshift destinations\. Units: Count  | 
| BackupToS3\.Success |  Sum of successful Amazon S3 put commands for backup over sum of all Amazon S3 backup put commands\. Kinesis Data Firehose emits this metric when data transformation is enabled for Amazon S3 or Amazon Redshift destinations\.  | 
| DataReadFromKinesisStream\.Bytes |  When the data source is a Kinesis data stream, this metric indicates the number of bytes read from that data stream\. This number includes rereads due to failovers\. Units: Bytes  | 
| DataReadFromKinesisStream\.Records |  When the data source is a Kinesis data stream, this metric indicates the number of records read from that data stream\. This number includes rereads due to failovers\. Units: Count  | 
| DeliveryToElasticsearch\.Bytes |  The number of bytes indexed to Amazon ES over the specified time period\. Units: Bytes  | 
| DeliveryToElasticsearch\.Records |  The number of records indexed to Amazon ES over the specified time period\. Units: Count  | 
| DeliveryToElasticsearch\.Success |  The sum of the successfully indexed records over the sum of records that were attempted\.  | 
| DeliveryToRedshift\.Bytes |  The number of bytes copied to Amazon Redshift over the specified time period\. Units: Bytes  | 
| DeliveryToRedshift\.Records |  The number of records copied to Amazon Redshift over the specified time period\. Units: Count  | 
| DeliveryToRedshift\.Success |  The sum of successful Amazon Redshift COPY commands over the sum of all Amazon Redshift COPY commands\.  | 
| DeliveryToS3\.Bytes |  The number of bytes delivered to Amazon S3 over the specified time period\. Units: Bytes  | 
| DeliveryToS3\.DataFreshness |  The age \(from getting into Kinesis Data Firehose to now\) of the oldest record in Kinesis Data Firehose\. Any record older than this age has been delivered to the S3 bucket\. Units: Seconds  | 
| DeliveryToS3\.Records |  The number of records delivered to Amazon S3 over the specified time period\. Units: Count  | 
| DeliveryToS3\.Success |  The sum of successful Amazon S3 put commands over the sum of all Amazon S3 put commands\.  | 
| DeliveryToSplunk\.Bytes |  The number of bytes delivered to Splunk over the specified time period\. Units: Bytes  | 
| DeliveryToSplunk\.DataAckLatency |  The approximate duration it takes to receive an acknowledgement from Splunk after Kinesis Data Firehose sends it data\. The increasing or decreasing trend for this metric is more useful than the absolute approximate value\. Increasing trends can indicate slower indexing and acknowledgement rates from Splunk indexers\. Units: Seconds  | 
| DeliveryToSplunk\.DataFreshness |  Age \(from getting into Kinesis Data Firehose to now\) of the oldest record in Kinesis Data Firehose\. Any record older than this age has been delivered to Splunk\. Units: Seconds  | 
| DeliveryToSplunk\.Records |  The number of records delivered to Splunk over the specified time period\. Units: Count  | 
| DeliveryToSplunk\.Success |  The sum of the successfully indexed records over the sum of records that were attempted\.  | 
| IncomingBytes |  The number of bytes ingested into the Kinesis Data Firehose stream over the specified time period\. Units: Bytes  | 
| IncomingRecords |  The number of records ingested into the Kinesis Data Firehose stream over the specified time period\. Units: Count  | 
| KinesisMillisBehindLatest |  When the data source is a Kinesis data stream, this metric indicates the number of milliseconds that the last read record is behind the newest record in the Kinesis data stream\. Units: Millisecond  | 

## API\-Level CloudWatch Metrics<a name="fh-metrics-api-cw"></a>

The `AWS/Firehose` namespace includes the following API\-level metrics\.


| Metric | Description | 
| --- | --- | 
| DescribeDeliveryStream\.Latency |  The time taken per `DescribeDeliveryStream` operation, measured over the specified time period\. Units: Milliseconds  | 
| DescribeDeliveryStream\.Requests |  The total number of `DescribeDeliveryStream` requests\. Units: Count  | 
| ListDeliveryStreams\.Latency |  The time taken per `ListDeliveryStream` operation, measured over the specified time period\. Units: Milliseconds  | 
| ListDeliveryStreams\.Requests |  The total number of `ListFirehose` requests\. Units: Count  | 
| PutRecord\.Bytes |  The number of bytes put to the Kinesis Data Firehose delivery stream using `PutRecord` over the specified time period\. Units: Bytes  | 
| PutRecord\.Latency |  The time taken per `PutRecord` operation, measured over the specified time period\. Units: Milliseconds  | 
| PutRecord\.Requests |  The total number of `PutRecord` requests, which is equal to total number of records from `PutRecord` operations\. Units: Count  | 
| PutRecordBatch\.Bytes |  The number of bytes put to the Kinesis Data Firehose delivery stream using `PutRecordBatch` over the specified time period\. Units: Bytes  | 
| PutRecordBatch\.Latency |  The time taken per `PutRecordBatch` operation, measured over the specified time period\. Units: Milliseconds  | 
| PutRecordBatch\.Records |  The total number of records from `PutRecordBatch` operations\. Units: Count  | 
| PutRecordBatch\.Requests |  The total number of `PutRecordBatch` requests\. Units: Count  | 
| ThrottledDescribeStream |  The total number of times the `DescribeStream` operation is throttled when the data source is a Kinesis data stream\. Units: Count  | 
| ThrottledGetRecords |  The total number of times the `GetRecords` operation is throttled when the data source is a Kinesis data stream\. Units: Count  | 
| ThrottledGetShardIterator |  The total number of times the `GetShardIterator` operation is throttled when the data source is a Kinesis data stream\. Units: Count  | 
| UpdateDeliveryStream\.Latency |  The time taken per `UpdateDeliveryStream` operation, measured over the specified time period\. Units: Milliseconds  | 
| UpdateDeliveryStream\.Requests |  The total number of `UpdateDeliveryStream` requests\. Units: Count  | 

## Data Transformation CloudWatch Metrics<a name="fh-metrics-data-transformation"></a>

If data transformation with Lambda is enabled, the `AWS/Firehose` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| ExecuteProcessing\.Duration |  The time it takes for each Lambda function invocation performed by Kinesis Data Firehose\. Units: Milliseconds  | 
| ExecuteProcessing\.Success |  The sum of the successful Lambda function invocations over the sum of the total Lambda function invocations\.  | 
| SucceedProcessing\.Records |  The number of successfully processed records over the specified time period\. Units: Count  | 
| SucceedProcessing\.Bytes |  The number of successfully processed bytes over the specified time period\. Units: Bytes  | 