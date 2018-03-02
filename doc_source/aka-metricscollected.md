# Amazon Kinesis Data Analytics Metrics<a name="aka-metricscollected"></a>

Kinesis Data Analytics sends metrics to CloudWatch\. For more information, see [Monitoring with Amazon CloudWatch Metrics](http://docs.aws.amazon.com/kinesisanalytics/latest/dev/monitoring-with-cloudwatch-metrics.html) in the *Amazon Kinesis Data Analytics Developer Guide*\.

## Metrics<a name="kinesis-analytics-metrics"></a>

The `AWS/KinesisAnalytics` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `Bytes` | The number of bytes read \(per input stream\) or written \(per output stream\)\.  Levels: Per input stream and per output stream  | 
|  `MillisBehindLatest` |  Indicates how far behind from the current time an application is reading from the streaming source\. Levels: Application\-level  | 
|  `Records` | The number of records read \(per input stream\) or written \(per output stream\)\. Levels: Per input stream and per output stream  | 
|  `Success` | 1 for each successful delivery attempt to the destination configured for your application; 0 for every failed delivery attempt\. The average value of this metric indicates how many successful deliveries are performed\. Levels: Per destination\.  | 
|  `InputProcessing.Duration` |  The time taken for each Lambda function invocation performed by Kinesis Data Analytics\. Levels: Per input stream  | 
|  `InputProcessing.OkRecords ` |  The number of records returned by a Lambda function that were marked with Ok status\.  Levels: Per input stream  | 
|  `InputProcessing.OkBytes ` |  The sum of bytes of the records returned by a Lambda function that were marked with Ok status\. Levels: Per input stream  | 
|  `InputProcessing.DroppedRecords ` |  The number of records returned by a Lambda function that were marked with Dropped status\.  Levels: Per input stream  | 
|  `InputProcessing.ProcessingFailedRecords ` |  The number of records returned by a Lambda function that were marked with ProcessingFailed status\.  Levels: Per input stream  | 
|  `InputProcessing.Success` |  The number of successful Lambda invocations by Kinesis Data Analytics\. Levels: Per input stream  | 
|  `LambdaDelivery.OkRecords` |  The number of records returned by a Lambda function that were marked with Ok status\.  Levels: Per Lambda destination | 
|  `LambdaDelivery.DeliveryFailedRecords` |  The number of records returned by a Lambda function that were marked with DeliveryFailed status\.  Levels: Per Lambda destination | 
|  `LambdaDelivery.Duration` |  The time taken for each Lambda function invocation performed by Kinesis Data Analytics\. Levels: Per Lambda destination | 

## Dimensions for Metrics<a name="kinesis-analytics-metricdimensions"></a>

Amazon Kinesis Data Analytics provides metrics for the following dimensions\.


| Dimension | Description | 
| --- | --- | 
|  `Flow`  |  Per input stream: Input  Per output stream: Output  | 
|  `Id`  |  Per input stream: Input Id Per output stream: Output Id  | 