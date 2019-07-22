# Amazon ECS Container Insights Metrics<a name="Container-Insights-metrics-ECS"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

The following table lists the metrics and dimensions that Container Insights collects for Amazon ECS\. These metrics are in the `ECS/ContainerInsights` namespace\. For more information, see [Metrics](cloudwatch_concepts.md#Metric)\.

If you do not see any Container Insights metrics in your console, be sure that you have completed the setup of Container Insights\. Metrics will not appear before Container Insights has been set up completely\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.


| Metric Name | Dimensions | Description | 
| --- | --- | --- | 
|  `ContainerInstanceCount` |  ClusterName  |  The number of EC2 instances running the Amazon ECS agent that are registered with a cluster\.  | 
|  `CpuUtilized` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The CPU units utilized by tasks in the resource that is specified by the dimension set that you're using\.   | 
|  `CpuReserved` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The CPU units reserved by tasks in the resource that is specified by the dimension set that you're using\.   | 
|  `DeploymentCount` |  ServiceName, ClusterName  |  The number of deployments in an Amazon ECS service\.  | 
|  `DesiredTaskCount` |  ServiceName, ClusterName  |  The desired number of tasks for an Amazon ECS service\.  | 
|  `MemoryUtilized` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The memory being utilized by tasks in the resource that is specified by the dimension set that you're using\.   | 
|  `MemoryReserved` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The memory that is reserved by tasks in the resource that is specified by the dimension set that you're using\.   | 
|  `NetworkRxBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes received by the resource that is specified by the dimensions that you're using\.  | 
|  `NetworkTxBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes transmitted by the resource that is specified by the dimensions that you're using\.  | 
|  `PendingTaskCount` |  ServiceName, ClusterName  |  The number of tasks currently in the `PENDING` state\.  | 
|  `RunningTaskCount` |  ServiceName, ClusterName  |  The number of tasks currently in the `RUNNING` state\.  | 
|  `ServiceCount` |  ServiceName  |  The number of services in the cluster\.  | 
|  `StorageReadBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes read from storage in the resource that is specified by the dimensions that you're using\.  | 
|  `StorageWriteBytes` |  TaskDefinitionFamily, ClusterName ServiceName, ClusterName ClusterName  |  The number of bytes written to storage in the resource that is specified by the dimensions that you're using\.  | 
|  `TaskCount` |  ServiceName  |  The number of tasks running in the service\.  | 
|  `TaskSetCount` |  ServiceName, ClusterName  |  The number of task sets in the service\.  | 