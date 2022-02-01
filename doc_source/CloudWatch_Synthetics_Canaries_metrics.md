# CloudWatch metrics published by canaries<a name="CloudWatch_Synthetics_Canaries_metrics"></a>

Canaries publish the following metrics to CloudWatch in the `CloudWatchSynthetics` namespace\. For more information about viewing CloudWatch metrics, see [Viewing available metrics](viewing_metrics_with_cloudwatch.md)\.


| Metric | Description | 
| --- | --- | 
|  `SuccessPercent`  |  The percentage of the runs of this canary that succeed and find no failures\. Valid Dimensions: CanaryName Valid Statistic: Average Units: Percent  | 
|  `Duration`  |  The duration in milliseconds of the canary run\. Valid Dimensions: CanaryName Valid Statistic: Average Units: Milliseconds  | 
|  `2xx`  |  The number of network requests performed by the canary that returned OK responses, with response codes between 200 and 299\. This metric is reported for UI canaries that use runtime version `syn-nodejs-2.0` or later, and is reported for API canaries that use runtime version `syn-nodejs-2.2` or later\. Valid Dimensions: CanaryName Valid Statistic: Sum Units: Count  | 
|  `4xx`  |  The number of network requests performed by the canary that returned Error responses, with response codes between 400 and 499\. This metric is reported for UI canaries that use runtime version `syn-nodejs-2.0` or later, and is reported for API canaries that use runtime version `syn-nodejs-2.2` or later\. Valid Dimensions: CanaryName Valid Statistic: Sum Units: Count  | 
|  `5xx`  |  The number of network requests performed by the canary that returned Fault responses, with response codes between 500 and 599\. This metric is reported for UI canaries that use runtime version `syn-nodejs-2.0` or later, and is reported for API canaries that use runtime version `syn-nodejs-2.2` or later\. Valid Dimensions: CanaryName Valid Statistic: Sum Units: Count  | 
|  `Failed`  |  The number of canary runs that failed to execute\. These failures are related to the canary itself\. Valid Dimensions: CanaryName Valid Statistic: Sum Units: Count  | 
|  `Failed requests`  |  The number of HTTP requests executed by the canary on the target website that failed with no response\. Valid Dimensions: CanaryName Valid Statistic: Sum Units: Count  | 
|  `VisualMonitoringSuccessPercent`  |  The percentage of visual comparisons that successfully matched the baseline screenshots during a canary run\. Valid Dimensions: CanaryName Valid Statistic: Average Units: Percent  | 
|  `VisualMonitoringTotalComparisons`  |  The total number of visual comparisons that happened during a canary run\. Valid Dimensions: CanaryName Units: Count  | 

**Note**  
Canaries that use either the `executeStep()` or `executeHttpStep()` methods from the Synthetics library also publish `SuccessPercent` and `Duration` metrics with the dimensions `CanaryName` and `StepName` for each step\.

 