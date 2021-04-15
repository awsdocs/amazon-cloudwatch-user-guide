# Amazon ECS Container Insights Metrics<a name="Container-Insights-metrics-ECS"></a>

The following table lists the metrics and dimensions that Container Insights collects for Amazon ECS\. These metrics are in the `ECS/ContainerInsights` namespace\. For more information, see [Metrics](cloudwatch_concepts.md#Metric)\.

If you do not see any Container Insights metrics in your console, be sure that you have completed the setup of Container Insights\. Metrics do not appear before Container Insights has been set up completely\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.

When you use Container Insights to collect the following metrics, the metrics are charged as custom metrics\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. Amazon ECS also automatically sends several free metrics to CloudWatch\. For more information, see [ Available metrics and dimensions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cloudwatch-metrics.html#available_cloudwatch_metrics)\.

The following metrics are available when you complete the steps in [Setting Up Container Insights on Amazon ECS for Cluster\- and Service\-Level Metrics](deploy-container-insights-ECS-cluster.md)


| Metric Name | Dimensions | Description | 
| --- | --- | --- | 
|  `ContainerInstanceCount` |  ClusterName  |  The number of EC2 instances running the Amazon ECS agent that are registered with a cluster\. Unit: Count  | 
|  `CpuUtilized` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The CPU units used by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined CPU reservation in their container definition\.   | 
|  `CpuReserved` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The CPU units reserved by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined CPU reservation in their task definition\.   | 
|  `DeploymentCount` |  ServiceName, ClusterName  |  The number of deployments in an Amazon ECS service\.  | 
|  `DesiredTaskCount` |  ServiceName, ClusterName  |  The desired number of tasks for an Amazon ECS service\. Unit: Count  | 
|  `MemoryUtilized` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The memory being used by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined memory reservation in their task definition\.   | 
|  `MemoryReserved` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The memory that is reserved by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined memory reservation in their task definition\.   | 
|  `NetworkRxBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes received by the resource that is specified by the dimensions that you're using\. This metric is available only for containers in tasks using the `awsvpc` or `bridge` network modes\. Unit: Bytes/Second  | 
|  `NetworkTxBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes transmitted by the resource that is specified by the dimensions that you're using\. This metric is available only for containers in tasks using the `awsvpc` or `bridge` network modes\. Unit: Bytes/Second  | 
|  `PendingTaskCount` |  ServiceName, ClusterName  |  The number of tasks currently in the `PENDING` state\. Unit: Count  | 
|  `RunningTaskCount` |  ServiceName, ClusterName  |  The number of tasks currently in the `RUNNING` state\. Unit: Count  | 
|  `ServiceCount` |  ClusterName  |  The number of services in the cluster\. Unit: Count  | 
|  `StorageReadBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes read from storage in the resource that is specified by the dimensions that you're using\. Unit: Bytes  | 
|  `StorageWriteBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes written to storage in the resource that is specified by the dimensions that you're using\. Unit: Bytes  | 
|  `TaskCount` |  ClusterName  |  The number of tasks running in the cluster\. Unit: Count  | 
|  `TaskSetCount` |  ServiceName, ClusterName  |  The number of task sets in the service\. Unit: Count  | 

The following metrics are available when you complete the steps in [Deploying the CloudWatch Agent to Collect EC2 Instance\-Level Metrics on Amazon ECS](deploy-container-insights-ECS-instancelevel.md)


| Metric Name | Dimensions | Description | 
| --- | --- | --- | 
|  `instance_cpu_limit` |  ClusterName  |  The maximum number of CPU units that can be assigned to a single EC2 Instance in the cluster\.  | 
|  `instance_cpu_reserved_capacity` |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The percentage of CPU currently being reserved on a single EC2 instance in the cluster\.  | 
|  `instance_cpu_usage_total` |  ClusterName  |  The number of CPU units being used on a Single EC2 instance in the cluster\.  | 
|  `instance_cpu_utilization` |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The total percentage of CPU units being used on a single EC2 instance in the cluster\.   | 
|  `instance_filesystem_utilization` |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The total percentage of file system capacity being used on a single EC2 instance in the cluster\.   | 
|  `instance_memory_limit` |  ClusterName  |  The maximum amount of memory, in bytes, that can be assigned to a single EC2 Instance in this cluster\.   | 
|  `instance_memory_reserved_capacity` |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The percentage of Memory currently being reserved on a single EC2 Instance in the cluster\.  | 
|  `instance_memory_utilization` |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The total percentage of memory being used on a single EC2 Instance in the cluster\.  | 
|  `instance_memory_working_set` |  ClusterName  |  The amount of memory, in bytes, being used on a single EC2 Instance in the cluster\.  | 
|  `instance_network_total_bytes` |  ClusterName  |  The total number of bytes per second transmitted and received over the network on a single EC2 Instance in the cluster\.  | 
|  `instance_number_of_running_tasks` |  ClusterName  |  The number of running tasks on a single EC2 Instance in the cluster\.  | 