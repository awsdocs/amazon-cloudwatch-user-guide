# Amazon EKS and Kubernetes Container Insights Metrics<a name="Container-Insights-metrics-EKS"></a>

The following table lists the metrics and dimensions that Container Insights collects for Amazon EKS and Kubernetes\. These metrics are in the `ContainerInsights` namespace\. For more information, see [Metrics](cloudwatch_concepts.md#Metric)\.

If you do not see any Container Insights metrics in your console, be sure that you have completed the setup of Container Insights\. Metrics do not appear before Container Insights has been set up completely\. For more information, see [Setting Up Container Insights](deploy-container-insights.md)\.

When you use Container Insights to collect the following metrics, the metrics are charged as custom metrics\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. 


| Metric Name | Dimensions | Description | 
| --- | --- | --- | 
|  `cluster_failed_node_count` |  ClusterName  |  The number of failed worker nodes in the cluster\. A node is considered failed if it is suffering from any [node conditions](https://github.com/kubernetes/node-problem-detector#remedy-systems)\.  | 
|  `cluster_node_count` |  ClusterName  |  The total number of worker nodes in the cluster\.  | 
|  `namespace_number_of_running_pods` |  Namespace ClusterName ClusterName  |  The number of pods running per namespace in the resource that is specified by the dimensions that you're using\.  | 
|  `node_cpu_limit` |  ClusterName  |  The maximum number of CPU units that can be assigned to a single node in this cluster\.  | 
|  `node_cpu_reserved_capacity` |  NodeName, ClusterName, InstanceId ClusterName  |  The percentage of CPU units that are reserved for node components, such as kubelet, kube\-proxy, and Docker\.  | 
|  `node_cpu_usage_total` |  ClusterName  |  The number of CPU units being used on the nodes in the cluster\.  | 
|  `node_cpu_utilization` |  NodeName, ClusterName, InstanceId ClusterName  |  The total percentage of CPU units being used on the nodes in the cluster\.  | 
|  `node_filesystem_utilization` |  NodeName, ClusterName, InstanceId ClusterName  |  The total percentage of file system capacity being used on nodes in the cluster\.  | 
|  `node_memory_limit` |  ClusterName  |  The maximum amount of memory, in bytes, that can be assigned to a single node in this cluster\.  | 
|  `node_memory_reserved_capacity` |  NodeName, ClusterName, InstanceId ClusterName  |  The percentage of memory currently being used on the nodes in the cluster\.  | 
|  `node_memory_utilization`  |  NodeName, ClusterName, InstanceId ClusterName  |  The percentage of memory currently being used by the node or nodes\. `node_memory_utilization` is calculated by `node_memory_working_set` /`node_memory_limit`\. It is the percentage of node memory usage over the node memory limitation\.  | 
|  `node_memory_working_set` |  ClusterName  |  The amount of memory, in bytes, being used in the working set of the nodes in the cluster\.  | 
|  `node_network_total_bytes` |  NodeName, ClusterName, InstanceId ClusterName  |  The total number of bytes per second transmitted and received over the network per node in a cluster\.  | 
|  `node_number_of_running_containers` |  NodeName, ClusterName, InstanceId ClusterName  |  The number of running containers per node in a cluster\.  | 
|  `node_number_of_running_pods` |  NodeName, ClusterName, InstanceId ClusterName  |  The number of running pods per node in a cluster\.  | 
|  `pod_cpu_reserved_capacity` |  PodName, Namespace, ClusterName ClusterName  |  The CPU capacity that is reserved per pod in a cluster\.  | 
|  `pod_cpu_utilization` |  PodName, Namespace, ClusterName Namespace, ClusterName Service, Namespace, ClusterName ClusterName  |  The percentage of CPU units being used by pods\.  | 
|  `pod_cpu_utilization_over_pod_limit` |  PodName, Namespace, ClusterName Namespace, ClusterName Service, Namespace, ClusterName ClusterName  |  The percentage of CPU units being used by pods that is over the pod limit\.  | 
|  `pod_memory_reserved_capacity` |  PodName, Namespace, ClusterName ClusterName  |  The percentage of memory that is reserved for pods\.  | 
|  `pod_memory_utilization` |  PodName, Namespace, ClusterName Namespace, ClusterName Service, Namespace, ClusterName ClusterName  |  The percentage of memory currently being used by the pod or pods\.  | 
|  `pod_memory_utilization_over_pod_limit` |  PodName, Namespace, ClusterName Namespace, ClusterName Service, Namespace, ClusterName ClusterName  |  The percentage of memory that is being used by pods that is over the pod limit\.  | 
|  `pod_number_of_container_restarts` |  PodName, Namespace, ClusterName  |  The total number of container restarts in a pod\.  | 
|  `pod_network_rx_bytes` |  PodName, Namespace, ClusterName Namespace, ClusterName Service, Namespace, ClusterName ClusterName  |  The number of bytes per second being received over the network by the pod\.  | 
|  `pod_network_tx_bytes` |  PodName, Namespace, ClusterName Namespace, ClusterName Service, Namespace, ClusterName ClusterName  |  The number of bytes per second being transmitted over the network by the pod\.  | 
|  `service_number_of_running_pods` |  Service, Namespace, ClusterName ClusterName  |  The number of pods running the service or services in the cluster\.  | 
