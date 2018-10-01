# Route 53 Metrics and Dimensions<a name="r53-metricscollected_shared"></a>

Route 53 sends metrics to CloudWatch\. CloudWatch provides detailed monitoring of Route 53 by default\. Route 53 sends one\-minute metrics to CloudWatch\. For more information, see [Monitoring Health Checks Using Amazon CloudWatch](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/health-checks-monitor-view-status.html#monitoring-health-checks) in the *Amazon Route 53 Developer Guide*\.

**Note**  
To get Route 53 metrics using CloudWatch, you must choose US East \(N\. Virginia\) as the region\. Route 53 metrics are not available if you select any other region\. You can also optionally specify a `Region` dimension\. For more information, see [Dimensions for Route 53 Metrics](#route-53-metrics-dimensions)\.

## Route 53 Metrics<a name="route-53-metrics-list"></a>

The `AWS/Route53` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `ChildHealthCheckHealthyCount`  |  For a calculated health check, the number of health checks that are healthy among the health checks that Route 53 is monitoring\. Valid statistics: Average \(recommended\), Minimum, Maximum Units: Healthy health checks  | 
|  `ConnectionTime`  |  The average time, in milliseconds, that it took Route 53 health checkers to establish a TCP connection with the endpoint\. You can view `ConnectionTime` for a health check either across all regions or for a selected geographic region\. Valid statistics: Average \(recommended\), Minimum, Maximum Units: Milliseconds  | 
|  `HealthCheckPercentageHealthy`  |  The percentage of Route 53 health checkers that consider the selected endpoint to be healthy\. You can view `HealthCheckPercentageHealthy` only across all regions; data is not available for a selected region\. Valid statistics: Average, Minimum, Maximum Units: Percent  | 
|  `HealthCheckStatus`  |  The status of the health check endpoint that CloudWatch is checking\. **1** indicates healthy, and **0** indicates unhealthy\. You can view `HealthCheckStatus` only across all regions; data is not available for a selected region\. Valid statistics: Minimum Units: none  | 
|  `SSLHandshakeTime`  |  The average time, in milliseconds, that it took Route 53 health checkers to complete the SSL handshake\. You can view `SSLHandshakeTime` for a health check either across all regions or for a selected geographic region\. Valid statistics: Average \(recommended\), Minimum, Maximum Units: Milliseconds  | 
|  `TimeToFirstByte`  |  The average time, in milliseconds, that it took Route 53 health checkers to receive the first byte of the response to an HTTP or HTTPS request\. You can view `TimeToFirstByte` for a health check either across all regions or for a selected geographic region\. Valid statistics: Average \(recommended\), Minimum, Maximum Units: Milliseconds  | 

## Dimensions for Route 53 Metrics<a name="route-53-metrics-dimensions"></a>

Route 53 metrics use the `AWS/Route53` namespace and provide metrics for `HealthCheckId`\. When retrieving metrics, you must supply the `HealthCheckId` dimension\.

In addition, for `ConnectionTime`, `SSLHandshakeTime`, and `TimeToFirstByte`, you can optionally specify `Region`\. If you omit `Region`, CloudWatch returns metrics across all regions\. If you include `Region`, CloudWatch returns metrics only for the specified region\.

For more information, see [Monitoring Health Checks Using CloudWatch](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/monitoring-health-checks.html) in the *Amazon Route 53 Developer Guide*\.