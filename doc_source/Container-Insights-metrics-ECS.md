# Amazon ECS Container Insights metrics<a name="Container-Insights-metrics-ECS"></a>

The following table lists the metrics and dimensions that Container Insights collects for Amazon ECS\. These metrics are in the `ECS/ContainerInsights` namespace\. For more information, see [Metrics](cloudwatch_concepts.md#Metric)\.

If you do not see any Container Insights metrics in your console, be sure that you have completed the setup of Container Insights\. Metrics do not appear before Container Insights has been set up completely\. For more information, see [Setting up Container Insights](deploy-container-insights.md)\.

When you use Container Insights to collect the following metrics, the metrics are charged as custom metrics\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. Amazon ECS also automatically sends several free metrics to CloudWatch\. For more information, see [ Available metrics and dimensions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cloudwatch-metrics.html#available_cloudwatch_metrics)\.

The following metrics are available when you complete the steps in [Setting up Container Insights on Amazon ECS for cluster\- and service\-level metrics](deploy-container-insights-ECS-cluster.md)


| Metric name | Dimensions | Description | 
| --- | --- | --- | 
|  `ContainerInstanceCount`  |  ClusterName  |  The number of EC2 instances running the Amazon ECS agent that are registered with a cluster\. Unit: Count  | 
|  `CpuUtilized`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The CPU units used by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined CPU reservation in their task definition\. Unit: None   | 
|  `CpuReserved`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The CPU units reserved by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined CPU reservation in their task definition\. Unit: None   | 
|  `DeploymentCount`  |  ServiceName, ClusterName  |  The number of deployments in an Amazon ECS service\. Unit: Count  | 
|  `DesiredTaskCount`  |  ServiceName, ClusterName  |  The desired number of tasks for an Amazon ECS service\. Unit: Count  | 
|  EphemeralStorageReserved [1](#ci-metrics-ecs-storage-fargate-note)  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes reserved from ephemeral storage in the resource that is specified by the dimensions that you're using\. Ephemeral storage is used for the container root filesystem and any bind mount host volumes defined in the container image and task definition\. The amount of ephemeral storage can’t be changed in a running task\. This metric is only available for tasks that run on Fargate Linux platform version 1\.4\.0 or later\. Unit: Gibibytes \(GiB\)  | 
|  EphemeralStorageUtilized [1](#ci-metrics-ecs-storage-fargate-note)  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes used from ephemeral storage in the resource that is specified by the dimensions that you're using\. Ephemeral storage is used for the container root filesystem and any bind mount host volumes defined in the container image and task definition\. The amount of ephemeral storage can’t be changed in a running task\. This metric is only available for tasks that run on Fargate Linux platform version 1\.4\.0 or later\. Unit: Gibibytes \(GiB\)  | 
|  `MemoryUtilized`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The memory being used by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined memory reservation in their task definition\. Unit: Megabytes   | 
|  `MemoryReserved`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The memory that is reserved by tasks in the resource that is specified by the dimension set that you're using\. This metric is collected only for tasks that have a defined memory reservation in their task definition\. Unit: Megabytes   | 
|  `NetworkRxBytes`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes received by the resource that is specified by the dimensions that you're using\. This metric is available only for containers in tasks using the `awsvpc` or `bridge` network modes\. Unit: Bytes/Second  | 
|  `NetworkTxBytes`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes transmitted by the resource that is specified by the dimensions that you're using\. This metric is available only for containers in tasks using the `awsvpc` or `bridge` network modes\. Unit: Bytes/Second  | 
|  `PendingTaskCount`  |  ServiceName, ClusterName  |  The number of tasks currently in the `PENDING` state\. Unit: Count  | 
|  `RunningTaskCount`  |  ServiceName, ClusterName  |  The number of tasks currently in the `RUNNING` state\. Unit: Count  | 
|  `ServiceCount`  |  ClusterName  |  The number of services in the cluster\. Unit: Count  | 
|  `StorageReadBytes`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes read from storage in the resource that is specified by the dimensions that you're using\. Unit: Bytes  | 
|  `StorageWriteBytes`  |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes written to storage in the resource that is specified by the dimensions that you're using\. Unit: Bytes  | 
|  `TaskCount`  |  ClusterName  |  The number of tasks running in the cluster\. Unit: Count  | 
|  `TaskSetCount`  |  ServiceName, ClusterName  |  The number of task sets in the service\. Unit: Count  | 

**Note**  
The `EphemeralStorageReserved` and `EphemeralStorageUtilized` metrics are only available for tasks that run on Fargate Linux platform version 1\.4\.0 or later\.  
Fargate reserves space on disk\. It is only used by Fargate\. You aren't billed for it\. It isn't shown in these metrics\. However, you can see this additional storage in other tools such as `df`\.

The following metrics are available when you complete the steps in [Deploying the CloudWatch agent to collect EC2 instance\-level metrics on Amazon ECS](deploy-container-insights-ECS-instancelevel.md)


| Metric name | Dimensions | Description | 
| --- | --- | --- | 
|  `instance_cpu_limit`  |  ClusterName  |  The maximum number of CPU units that can be assigned to a single EC2 Instance in the cluster\. Unit: None  | 
|  `instance_cpu_reserved_capacity`  |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The percentage of CPU currently being reserved on a single EC2 instance in the cluster\. Unit: Percent  | 
|  `instance_cpu_usage_total`  |  ClusterName  |  The number of CPU units being used on a Single EC2 instance in the cluster\. Unit: None  | 
|  `instance_cpu_utilization`  |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The total percentage of CPU units being used on a single EC2 instance in the cluster\.  Unit: Percent  | 
|  `instance_filesystem_utilization`  |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The total percentage of file system capacity being used on a single EC2 instance in the cluster\.  Unit: Percent  | 
|  `instance_memory_limit`  |  ClusterName  |  The maximum amount of memory, in bytes, that can be assigned to a single EC2 Instance in this cluster\.  Unit: Bytes  | 
|  `instance_memory_reserved_capacity`  |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The percentage of Memory currently being reserved on a single EC2 Instance in the cluster\. Unit: Percent  | 
|  `instance_memory_utilization`  |  ClusterName InstanceId, ContainerInstanceId,ClusterName  |  The total percentage of memory being used on a single EC2 Instance in the cluster\. Unit: Percent  | 
|  `instance_memory_working_set`  |  ClusterName  |  The amount of memory, in bytes, being used on a single EC2 Instance in the cluster\. Unit: Bytes  | 
|  `instance_network_total_bytes`  |  ClusterName  |  The total number of bytes per second transmitted and received over the network on a single EC2 Instance in the cluster\. Unit: Bytes/second  | 
|  `instance_number_of_running_tasks`  |  ClusterName  |  The number of running tasks on a single EC2 Instance in the cluster\. Unit: Count  | 