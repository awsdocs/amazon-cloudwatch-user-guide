# AWS Elastic Beanstalk Metrics and Dimensions<a name="aeb-metricscollected"></a>

AWS Elastic Beanstalk sends metrics to Amazon CloudWatch\. For more information, see [Publishing Amazon CloudWatch Custom Metrics for an Environment](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/health-enhanced-cloudwatch.html) in the *AWS Elastic Beanstalk Developer Guide*\.

## Elastic Beanstalk Metrics<a name="beanstalk-metrics"></a>

The `AWS/ElasticBeanstalk` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| `EnvironmentHealth` |  \[Environment\] The health status of the environment\. The possible values are 0 \(OK\), 1 \(Info\), 5 \(Unknown\), 10 \(No data\), 15 \(Warning\), 20 \(Degraded\) and 25 \(Severe\)\.  | 
| `InstancesOk` | \[Environment\] The number of instances with OK health status\. | 
| `InstancesPending` | \[Environment\] The number of instances with Pending health status\. | 
| `InstancesInfo` | \[Environment\] The number of instances with Info health status\. | 
| `InstancesUnknown` | \[Environment\] The number of instances with Unknown health status\. | 
| `InstancesNoData` | \[Environment\] The number of instances with no health status data\. | 
| `InstancesWarning` | \[Environment\] The number of instances with Warning health status\. | 
| `InstancesDegraded` | \[Environment\] The number of instances with Degraded health status\. | 
| `InstancesSevere` | \[Environment\] The number of instances with Severe health status\. | 
| `ApplicationRequestsTotal` | The number of requests completed by the instance or environment\. | 
| `ApplicationRequests2xx` | The number of requests that completed with a 2XX status code\. | 
| `ApplicationRequests3xx` | The number of requests that completed with a 3XX status code\. | 
| `ApplicationRequests4xx` | The number of requests that completed with a 4XX status code\. | 
| `ApplicationRequests5xx` | The number of requests that completed with a 5XX status code\. | 
| `ApplicationLatencyP10` | The average time to complete the fastest 10 percent of requests\. | 
| `ApplicationLatencyP50` | The average time to complete the fastest 50 percent of requests\. | 
| `ApplicationLatencyP75` | The average time to complete the fastest 75 percent of requests\. | 
| `ApplicationLatencyP85` | The average time to complete the fastest 85 percent of requests\. | 
| `ApplicationLatencyP90` | The average time to complete the fastest 90 percent of requests\. | 
| `ApplicationLatencyP95` | The average time to complete the fastest 95 percent of requests\. | 
| `ApplicationLatencyP99` | The average time to complete the fastest 99 percent of requests\. | 
| `ApplicationLatencyP99.9` | The average time to complete the fastest x percent of requests\. | 
| `LoadAverage1min` | \[Instance\] The average CPU load over the last minute\. | 
| `InstanceHealth` | \[Instance\] The health status of the instance\. | 
| `RootFilesystemUtil` | \[Instance\] The percentage of disk space in use\. | 
| `CPUIrq` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 
| `CPUUser` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 
| `CPUIdle` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 
| `CPUSystem` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 
| `CPUSoftirq` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 
| `CPUIowait` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 
| `CPUNice` | \[Instance\] The percentage of time the CPU was in this state in the last minute\. | 

## Dimensions for Elastic Beanstalk Metrics<a name="beanstalk-metric-dimensions"></a>

You can filter the data using the following dimensions\.


| Dimensions | Description | 
| --- | --- | 
| `EnvironmentName` |  Filter the data by environment\.  | 
| `InstanceId` |  Filter the data by instance\.  | 