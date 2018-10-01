# Amazon Translate Metrics<a name="translate-metricscollected"></a>

The following table describes the Amazon Translate metrics\.

## CloudWatch Metrics for Amazon Translate<a name="translate-cloudwatch-metrics"></a>


| Metric | Description | 
| --- | --- | 
| CharacterCount | The number of billable characters in requests\. Valid dimensions: Language pair, Operation Valid statistics: Average, Maximum, Minimum, Sum Unit: Count  | 
| ResponseTime | The time that it took to respond to a request\. Valid dimensions: Language pair, Operation Valid statistics: Data samples, Average Unit: For Data samples, count\. For Average statistics, milliseconds\.  | 
| ServerErrorCount | The number of server errors\. The HTTP response code range for a server error is 500 to 599\. Valid dimension: Operation Valid statistics: Average, Sum Unit: Count | 
| SuccessfulRequestCount | The number of successful translation requests\. The response code for a successful request is 200 to 299\. Valid dimension: Operation Valid statistics: Average, Sum Unit: Count | 
| ThrottledCount | The number of requests subject to throttling\. Use `ThrottledCount` to determine if your application is sending requests to Amazon Translate faster than your account is configured to accept them\. For more information, see [Amazon Translate Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_amazon_translate) in the *Amazon Web Services General Reference*\.   Valid dimension: Operation Valid statistics: Average, Sum Unit: Count | 
| UserErrorCount | The number of user errors that occurred\. The HTTP response code range for a user error is 400 to 499\.  Valid dimension: Operation Valid statistics: Average, Sum Unit: Count | 

## CloudWatch Dimensions for Amazon Translate<a name="translate-dimensions"></a>

Use the following dimensions to filter Amazon Translate metrics\. Metrics are grouped by the source language and the target language\.


| Dimension | Description | 
| --- | --- | 
| LanguagePair | Restricts the metrics to only those that contain the specified languages\. | 
| Operation | Restricts the metrics to only those with the specified operation\. | 