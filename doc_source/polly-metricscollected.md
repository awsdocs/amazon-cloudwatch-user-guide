# Amazon Polly Metrics<a name="polly-metricscollected"></a>

Amazon Polly sends metrics to CloudWatch\. For more information, see the *Amazon Polly Developer Guide*\.

## Amazon Polly Metrics<a name="polly-metrics"></a>

Amazon Polly produces the following metrics for each request\. These metrics are aggregated and in one minute intervals sent to CloudWatch where they are available in the `AWS/Polly` namespace\.


| Metric | Description | 
| --- | --- | 
|  `RequestCharacters` |  The number of characters in the request\. This is billable characters only and does not include SSML tags\. Valid Dimension: Operation Valid Statistics: Minimum, Maximum, Average, SampleCount, Sum Unit: Count  | 
|  `ResponseLatency` |  The latency between when the request was made and the start of the streaming response\. Valid Dimensions: Operation Valid Statistics: Minimum, Maximum, Average, SampleCount Unit: milliseconds  | 
|  `2XXCount` |  HTTP 200 level code returned upon a successful response\. Valid Dimensions: Operation Valid Statistics: Average, SampleCount, Sum Unit: Count  | 
|  `4XXCount` |  HTTP 400 level error code returned upon an error\. For each successful response, a zero \(0\) is emitted\. Valid Dimensions: Operation Valid Statistics: Average, SampleCount, Sum Unit: Count  | 
|  `5XXCount` |  HTTP 500 level error code returned upon an error\. For each successful response, a zero \(0\) is emitted\. Valid Dimensions: Operation Valid Statistics: Average, SampleCount, Sum Unit: Count  | 

## Dimensions for Amazon Polly Metrics<a name="polly-metricdimensions"></a>

Amazon Polly provides metrics for the following dimension\.


| Dimension | Description | 
| --- | --- | 
|  `Operation`  |  Metrics are grouped by the API method they refer to\. Possible values are `SynthesizeSpeech`, `PutLexicon`, `DescribeVoices`, etc\.  | 