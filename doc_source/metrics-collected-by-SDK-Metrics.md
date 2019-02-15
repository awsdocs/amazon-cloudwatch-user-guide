# Metrics and Data Collected by AWS SDK Metrics for Enterprise Support<a name="metrics-collected-by-SDK-Metrics"></a>

SDK Metrics collects data from your applications and uses it to emit metrics to CloudWatch\. The following table lists the data collected by SDK Metrics\.


| Data | Type | 
| --- | --- | 
|  Message Version |  String  | 
|  Message ID |  String  | 
|  Service Endpoint |  String  | 
|  Normalized Service ID |  String  | 
|  API Operation name |  String  | 
|  Availability \(from SDK customer point of view\) |  Integer \(0 or 1\) with number of samples  | 
|  Latency \(from SDK customer point of view\) |  Distribution  | 
|  SDK Version |  String  | 
|  Client Language Runtime Version |  String  | 
|  Client Operating System |  String  | 
|  Service Response Codes |  Key/value pairs  | 
|  Client Language Runtime Version |  String  | 
|  Sample Request IDs |  List  | 
|  Retries |  Distribution  | 
|  Throttled Requests |  Distribution  | 
|  AccountID |  String  | 
|  Availability Zone |  String  | 
|  Instance ID |  String  | 
|  Runtime Environment \(Lambda/ECS\) |  String  | 
|  Network Error Messages |  String/map  | 
|  Source IP Address |  String  | 
|  Destination IP Address |  String  | 

The following table lists the metrics that Enterprise Support customers can collect using AWS SDK Metrics for Enterprise Support\. These metrics are in the `AWS/SDKMetrics` namespace\.

AWS Support resources and your technical account manager should have access to SDK Metrics data to help you resolve cases\. If you discover data that is confusing or unexpected but doesn’t negatively impact your application performance, we recommend that you wait and review that data during scheduled business reviews with your technical account manager\.


| Metric | Description | 
| --- | --- | 
|  **CallCount** |  Total number of successful or failed API calls from your code to AWS services\. Use `CallCount` as a baseline to correlate with other metrics such as `ServerErrorCount` and `ThrottleCount`\. Unit: Count  | 
|  **ClientErrorCount** |  The number of API calls that failed with client errors \(4xx HTTP response codes\)\. These can include throttling errors, *access denied*, *S3 bucket does not exist*, and *invalid parameter value*\. A high value for this metric usually indicates that something in your application needs to be fixed, unless the high value is a result of throttling caused by an AWS service limit\. In this case, you should increase your service limit\. Unit: Count  | 
|  **EndToEndLatency** |  The total time for your application to make a call using the AWS SDK, inclusive of retries\. Use `EndToEndLatency` to determine how AWS API calls contribute to your application’s overall latency\. Higher than expected latency might be caused by issues with your network, firewall, or other configuration settings\. Latency can also result from SDK retries\. Unit: Milliseconds  | 
|  **ConnectionErrorCount** |  The number of API calls that fail because of errors connecting to the service\. These can be caused by network issues between your application and AWS services, including load balancer issues, DNS failures, and transit provider issues\. In some cases, AWS issues might result in this error\. Use this metric to determine whether problems are specific to your application or are caused by your infrastructure or network\. A high value could also indicate short timeout values for API calls\. Unit: Count  | 
|  **ServerErrorCount** |  The number of API calls that fail because of server errors \(5xx HTTP response codes\) from AWS services\. These are typically caused by AWS services\. Use this metric to determine the cause of SDK retries or latency\. This metric doesn't always indicate that AWS services are at fault because some AWS teams classify latency as an HTTP 503 response\.  Unit: Count  | 
|  **ThrottleCount** |  The number of API calls that fail because of throttling by AWS services\. Use this metric to assess if your application has reached throttle limits, as well as to determine the cause of retries and application latency\. If you see high values, consider distributing calls over a window instead of batching your calls\. Unit: Count  | 

You can use the following dimensions with SDK Metrics\.


| Dimension | Description | 
| --- | --- | 
|  DestinationRegion |  The AWS Region that is the destination of the call\.  | 
|  Service |  The AWS service being called by the application\.  | 