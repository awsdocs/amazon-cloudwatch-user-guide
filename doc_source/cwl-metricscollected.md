# Amazon CloudWatch Logs Metrics and Dimensions<a name="cwl-metricscollected"></a>

CloudWatch Logs sends metrics to CloudWatch every minute\.

## CloudWatch Logs Metrics<a name="cwl-metrics"></a>

The `AWS/Logs` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `IncomingBytes`  |  The volume of log events in uncompressed bytes uploaded to CloudWatch Logs\. When used with the `LogGroupName` dimension, this is the volume of log events in uncompressed bytes uploaded to the log group\. Valid Dimensions: LogGroupName Valid Statistic: Sum Units: Bytes  | 
|  `IncomingLogEvents`  |  The number of log events uploaded to CloudWatch Logs\. When used with the `LogGroupName` dimension, this is the number of log events uploaded to the log group\.  Valid Dimensions: LogGroupName Valid Statistic: Sum Units: None  | 
|  `ForwardedBytes`  |  The volume of log events in compressed bytes forwarded to the subscription destination\. Valid Dimensions: LogGroupName, DestinationType, FilterName  Valid Statistic: Sum Units: Bytes  | 
|  `ForwardedLogEvents`  |  The number of log events forwarded to the subscription destination\. Valid Dimensions: LogGroupName, DestinationType, FilterName Valid Statistic: Sum Units: None  | 
|  `DeliveryErrors`  |  The number of log events for which CloudWatch Logs received an error when forwarding data to the subscription destination\. Valid Dimensions: LogGroupName, DestinationType, FilterName Valid Statistic: Sum Units: None  | 
|  `DeliveryThrottling`  |  The number of log events for which CloudWatch Logs was throttled when forwarding data to the subscription destination\. Valid Dimensions: LogGroupName, DestinationType, FilterName Valid Statistic: Sum Units: None  | 

## Dimensions for CloudWatch Logs Metrics<a name="cwl-metric-dimensions"></a>

The dimensions that you can use with CloudWatch Logs metrics are listed below\.


|  Dimension  |  Description  | 
| --- | --- | 
|  LogGroupName  |  The name of the CloudWatch Logs log group for which to display metrics\.  | 
|  DestinationType  |  The subscription destination for the CloudWatch Logs data, which can be AWS Lambda, Amazon Kinesis Data Streams, or Amazon Kinesis Data Firehose\.  | 
|  FilterName  |  The name of the subscription filter that is forwarding data from the log group to the destination\. The subscription filter name is automatically converted by CloudWatch to ASCII and any unsupported characters get replaced with a question mark \(?\)\.  | 