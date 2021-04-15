# Send logs to CloudWatch Logs<a name="Container-Insights-EKS-logs"></a>

To send logs from your containers to Amazon CloudWatch Logs, you can use Fluent Bit or Fluentd\. For more information, see [Fluent Bit](https://fluentbit.io/) and [Fluentd](https://www.fluentd.org/)\.

If you are not already using Fluentd, we recommend that you use Fluent Bit for the following reasons:
+ Fluent Bit has a smaller resource footprint and is more resource\-efficient with memory and CPU usage than FluentD\. For a more detailed comparison, see [Fluent Bit and Fluentd performance comparison](#Container-Insights-EKS-logs-performance)\.
+ The Fluent Bit image is developed and maintained by AWS\. This gives AWS the ability to adopt new Fluent Bit image features and respond to issues much quicker\. 

**Topics**
+ [Fluent Bit and Fluentd performance comparison](#Container-Insights-EKS-logs-performance)
+ [Set Up Fluent Bit as a DaemonSet to Send Logs to CloudWatch Logs](Container-Insights-setup-logs-FluentBit.md)
+ [\(Optional\) Set Up FluentD as a DaemonSet to Send Logs to CloudWatch Logs](Container-Insights-setup-logs.md)
+ [\(Optional\) Set Up Amazon EKS Control Plane Logging](Container-Insights-setup-control-plane-logging.md)
+ [\(Optional\) Enable App Mesh Envoy Access Logs](ContainerInsights-Prometheus-Sample-Workloads-appmesh-envoy.md)

## Fluent Bit and Fluentd performance comparison<a name="Container-Insights-EKS-logs-performance"></a>

The following tables show the the performance advantage that Fluent Bit has over FluentD in memory and CPU usages\. The following numbers are just for reference and might change depending on the environment\.


| Logs per second | Fluentd CPU usage | Fluent Bit CPU usage with Fluentd\-compatible configuration | Fluent Bit CPU usage with optimized configuration | 
| --- | --- | --- | --- | 
| 100 | 0\.35 vCPU | 0\.02 vCPU | 0\.02 vCPU | 
| 1,000 | 0\.32 vCPU | 0\.14 vCPU | 0\.11 vCPU | 
| 5,000 | 0\.85 vCPU | 0\.48 vCPU | 0\.30 vCPU | 
| 10,000 | 0\.94 vCPU | 0\.60 vCPU | 0\.39 vCPU | 


| Logs per second | Fluentd memory usage | Fluent Bit memory usage with Fluentd\-compatible configuration | Fluent Bit memory usage with optimized configuration | 
| --- | --- | --- | --- | 
| 100 | 153 MB | 46 MB | 37 MB | 
| 1,000 | 270 MB | 45 MB | 40 MB | 
| 5,000 | 320 MB | 55 MB | 45 MB | 
| 10,000 | 375 MB | 92 MB | 75 MB | 