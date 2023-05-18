# CloudWatch metrics that you can collect with CloudWatch RUM<a name="CloudWatch-RUM-metrics"></a>

The table in this section lists the metrics that you automatically collect with CloudWatch RUM\. You can see these metrics in the CloudWatch console\. For more information, see [View available metrics](viewing_metrics_with_cloudwatch.md)\.

You can also optionally send extended metrics to CloudWatch or CloudWatch Evidently\. For more information, see [Extended metrics](CloudWatch-RUM-custom-and-extended-metrics.md#CloudWatch-RUM-vended-metrics)\. 

These metrics are published in the metric namespace named `AWS/RUM`\. All of the following metrics are published with an `application_name` dimension\. The value of this dimension is the name of the app monitor\. Some metrics are also published with additional dimensions, as listed in the table\.


| Metric | Unit | Description | 
| --- | --- | --- | 
|  `HttpStatusCodeCount` |  Count  |  The count of HTTP responses in the application, by their response status code\. Additional dimensions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-RUM-metrics.html)  | 
|  `JsErrorCount` |  Count  |  The count of JavaScript error events ingested\.   | 
|  `NavigationFrustratedCount` |  Count  |  The count of navigation events with a `duration` higher than the frustrating threshold, which is 8000ms\. The duration of navigation events is tracked in the `PerformanceNavigationDuration` metric\.  | 
|  `NavigationSatisfiedCount` |  Count  |  The count of navigation events with a `duration` that is less than the Apdex objective, which is 2000ms\. The duration of navigation events is tracked in the `PerformanceNavigationDuration` metric\.  | 
|  `NavigationToleratedCount` |  Count  |  The count of navigation events with a `duration` between 2000ms and 8000ms\. The duration of navigation events is tracked in the `PerformanceNavigationDuration` metric\.  | 
|  `PerformanceResourceDuration` |  Milliseconds  |  The `duration` of a resource event\. Additional dimensions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-RUM-metrics.html)  | 
|  `PerformanceNavigationDuration` |  Milliseconds  |  The `duration` of a navigation event\.  | 
|  `RumEventPayloadSize` |  Bytes  |  The size of every event ingested by CloudWatch RUM\. You can also use the `SampleCount` statistic for this metric to monitor the number of events that an app monitor is ingesting\.  | 
|  `SessionCount` |  Count  |  The count of session start events ingested by the app monitor\. In other words, the number of new sessions started\.  | 
|  `WebVitalsCumulativeLayoutShift` |  None  |  Tracks the value of the cumulative layout shift events\.  | 
|  `WebVitalsFirstInputDelay` |  Milliseconds  |  Tracks the value of the first input delay events\.  | 
|  `WebVitalsLargestContentfulPaint` |  Milliseconds  |  Tracks the value of the largest contentful paint events\.  | 