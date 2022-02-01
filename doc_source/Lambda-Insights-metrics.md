# Metrics collected by Lambda Insights<a name="Lambda-Insights-metrics"></a>

Lambda Insights collects several metrics from the Lambda functions where it is installed\. Some of these metrics are available as time series aggregated data in CloudWatch Metrics\. Other metrics are not aggregated into time series data but can be found in the embedded metric format log entries by using CloudWatch Logs Insights\.

The following metrics are available as time series aggregated data in CloudWatch Metrics in the `LambdaInsights` namespace\.


| Metric name | Dimensions | Description | 
| --- | --- | --- | 
|  `cpu_total_time` |  function\_name function\_name, version  |  Sum of `cpu_system_time` and `cpu_user_time`\. Unit: Milliseconds  | 
|  `init_duration` |  function\_name function\_name, version  |  The amount of time spent in the `init` phase of the Lambda execution environment lifecycle\. Unit: Milliseconds  | 
|  `memory _utilization` |  function\_name function\_name, version  |  The maximum memory measured as a percentage of the memory allocated to the function\. Unit: Percent  | 
|  `rx_bytes` |  function\_name function\_name, version  |  The number of bytes received by the function\. Unit: Bytes  | 
|  `tx_bytes` |  function\_name function\_name, version  |  The number of bytes sent by the function\. Unit: Bytes  | 
|  `total_memory` |  function\_name function\_name, version  |  The amount of memory allocated to your Lambda function\. This is the same as your function’s memory size\. Unit: Megabytes  | 
|  `total_network` |  function\_name function\_name, version  |  Sum of `rx_bytes` and `tx_bytes`\. Even for functions that don't perform I/O tasks, this value is usually greater than zero because of network calls made by the Lambda runtime\. Unit: Bytes  | 
|  `used_memory_max` |  function\_name function\_name, version  |  The measured memory of the function sandbox\. Unit: Megabytes  | 

The following metrics can be found in the embedded metric format log entries by using CloudWatch Logs Insights\. For more information about CloudWatch Logs Insights, see [ Analyzing Log Data with CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\.

For more information about embedded metric format, see [Ingesting high\-cardinality logs and generating metrics with CloudWatch embedded metric format](CloudWatch_Embedded_Metric_Format.md)\.


| Metric name | Description | 
| --- | --- | 
|  `cpu_system_time` |  The amount of time the CPU spent executing kernel code\. Unit: Milliseconds  | 
|  `cpu_total_time` |  Sum of `cpu_system_time` and `cpu_user_time`\. Unit: Milliseconds  | 
|  `cpu_user_time` |  The amount of time the CPU spent executing user code\. Unit: Milliseconds  | 
|  `fd_max` |  The maximum number of file descriptors available\. Unit: Count  | 
|  `fd_use` |  The maximum number of file descriptors in use\. Unit: Count  | 
|  `memory _utilization` |  The maximum memory measured as a percentage of the memory allocated to the function\. Unit: Percent  | 
|  `rx_bytes` |  The number of bytes received by the function\. Unit: Bytes  | 
|  `tx_bytes` |  The number of bytes sent by the function\. Unit: Bytes  | 
|  `threads_max` |  The number of threads in use by the function process\. As a function author, you don't control the initial number of threads created by the runtime\. Unit: Count  | 
|  `tmp_max` |  The amount of space available in the `/tmp` directory\. Unit: Bytes  | 
|  `tmp_used` |  The amount of space used in the `/tmp` directory\. Unit: Bytes  | 
|  `total_memory` |  The amount of memory allocated to your Lambda function\. This is the same as your function’s memory size\. Unit: Megabytes  | 
|  `total_network` |  Sum of `rx_bytes` and `tx_bytes`\. Even for functions that don't perform I/O tasks, this value is usually greater than zero because of network calls made by the Lambda runtime\. Unit: Bytes  | 
|  `used_memory_max` |  The measured memory of the function sandbox\. Unit: Bytes  | 