# Amazon EKS and Kubernetes Container Insights Metrics<a name="Container-Insights-metrics-EKS"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

The following table lists the metrics and dimensions that Container Insights collects for Amazon EKS and Kubernetes\. These metrics are in the `ContainerInsights` namespace\. For more information, see [Metrics](cloudwatch_concepts.md#Metric)\.

If you do not see any Container Insights metrics in your console, be sure that you have completed the setup of Container Insights\. Metrics will not appear before Container Insights has been set up completely\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.


| Metric Name | Dimensions | Description | 
| --- | --- | --- | 
|  `cluster_failed_node_count` |  ClusterName  |  The number of failed worker nodes in the cluster\.  | 
|  `cluster_node_count` |  ClusterName  |  The total number of worker nodes in the cluster\.  | 
|  `namespace_number_of_running_pods` |  NameSpace, ClusterName ClusterName  |  The number of pods running per namespace in the resource that is specified by the dimensions that you're using\.  | 
|  `node_cpu_limit` |  ClusterName  |  The maximum number of CPU units that can be assigned to a single node in this cluster\.  | 
|  `node_cpu_reserved_capacity` |  NodeName, ClusterName ClusterName  |  The number of CPU units that are reserved for node components, such as kubelet, kube\-proxy, and Docker\.  | 
|  `node_cpu_usage_total` |  ClusterName  |  The number of CPU units being used on the nodes in the cluster\.  | 
|  `node_cpu_utilization` |  NodeName, ClusterName ClusterName  |  The total percentage of CPU units being used on the nodes in the cluster\.  | 
|  `node_filesystem_utilization` |  NodeName, ClusterName ClusterName  |  The total percentage of file system capacity being used on nodes in the cluster\.  | 
|  `node_memory_limit` |  ClusterName  |  The maximum amount of memory, in bytes, that can be assigned to a single node in this cluster\.  | 
|  `node_memory_reserved_capacity` |  NodeName, ClusterName ClusterName  |  The amount of memory, in bytes, currently being used on the nodes in the cluster\.  | 
|  `node_memory_utilization` |  NodeName, ClusterName ClusterName  |  The amount of memory, in bytes, currently being used by the node or nodes\.  | 
|  `node_memory_working_set` |  ClusterName  |  The amount of memory, in bytes, being used in the working set of the nodes in the cluster\.  | 
|  `node_network_total_bytes` |  NodeName, ClusterName ClusterName  |  The total number of bytes transmitted and received over the network per node in a cluster\.  | 
|  `node_number_of_running_containers` |  NodeName, ClusterName ClusterName  |  The number of running containers per node in a cluster\.  | 
|  `node_number_of_running_pods` |  NodeName, ClusterName ClusterName  |  The number of running pods per node in a cluster\.  | 
|  `pod_cpu_reserved_capacity` |  PodName, ClusterName ClusterName  |  The CPU capacity that is reserved per pod in a cluster\.  | 
|  `pod_cpu_utilization` |  PodName, ClusterName Namespace, ClusterName Service, ClusterName ClusterName  |  The number of CPU units being used by pods\.  | 
|  `pod_cpu_utilization_over_pod_limit` |  PodName, ClusterName Namespace, ClusterName Service, ClusterName ClusterName  |  The number of CPU units being used by pods that is over the pod limit\.  | 
|  `pod_memory_reserved_capacity` |  PodName, ClusterName ClusterName  |  The bytes in memory that are reserved for pods\.  | 
|  `pod_memory_utilization` |  PodName, ClusterName Namespace, ClusterName Service, ClusterName ClusterName  |  The amount of memory, in bytes, currently being used by the pod or pods\.  | 
|  `pod_memory_utilization_over_pod_limit` |  PodName, ClusterName Namespace, ClusterName Service, ClusterName ClusterName  |  The amount of memory, in bytes, that is being used by pods that is over the pod limit\.  | 
|  `pod_network_rx_bytes` |  PodName, ClusterName Namespace, ClusterName Service, ClusterName ClusterName  |  The number of bytes being received over the network by the pod\.  | 
|  `pod_network_tx_bytes` |  PodName, ClusterName Namespace, ClusterName Service, ClusterName ClusterName  |  The number of bytes being transmitted over the network by the pod\.  | 
|  `service_number_of_running_pods` |  Service, ClusterName ClusterName  |  The number of pods running the service or services in the cluster\.  | 