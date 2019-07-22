# Relevant Fields in Performance Log Events for Amazon EKS and Kubernetes<a name="Container-Insights-reference-performance-entries-EKS"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

For Amazon EKS and Kubernetes, the containerized CloudWatch agent emits data as performance log events\. This enables CloudWatch to ingest and store high\-cardinality data\. CloudWatch uses the data in the performance log events to create aggregated CloudWatch metrics at the cluster, node, and pod levels without the need to lose granular details\.

The following table lists the fields in these performance log events that are relevant to the collection of Container Insights metric data\. You can use CloudWatch Logs Insights to query for any of these fields to collect data or investigate issues\. For more information, see [Analyze Log Data With CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)\.


| Type | Log field | Source | Formula or Notes | 
| --- | --- | --- | --- | 
|  Pod |  `pod_cpu_utilization`  |  Calculated  |  Formula: `pod_cpu_usage_total / node_cpu_limit`  | 
|  Pod |  `pod_cpu_usage_total`  |  cadvisor  |   | 
|  Pod |  `pod_cpu_limit`  |  Calculated  |  Formula: `sum(container_cpu_limit)` If any containers in the pod don't have a CPU limit defined, this field doesn't appear in the log event\.  | 
|  Pod |  `pod_cpu_request`  |  Calculated  |  Formula: `sum(container_cpu_request)` `container_cpu_request` isn't guaranteed to be set\. Only the ones that are set are included in the sum\.  | 
|  Pod |  `pod_cpu_utilization_over_pod_limit`  |  Calculated  |  Formula: `pod_cpu_usage_total / pod_cpu_limit`  | 
|  Pod |  `pod_cpu_reserved_capacity`  |  Calculated  |  Formula: `pod_cpu_request / node_cpu_limit`  | 
|  Pod |  `pod_memory_utilization`  |  Calculated  |  Formula: `pod_memory_working_set / node_memory_limit`  | 
|  Pod |  `pod_memory_working_set`  |  cadvisor  |   | 
|  Pod |  `pod_memory_limit`  |  Calculated  |  Formula: `sum(container_memory_limit)` If any containers in the pod don't have a memory limit defined, this field doesn't appear in the log event\.  | 
|  Pod |  `pod_memory_request`  |  Calculated  |  Formula: `sum(container_memory_request)` `container_memory_request` isn't guaranteed to be set\. Only the ones that are set are included in the sum\.  | 
|  Pod |  `pod_memory_utilization_over_pod_limit`  |  Calculated  |  Formula: `pod_memory_working_set / pod_memory_limit` If any containers in the pod don't have a memory limit defined, this field doesn't appear in the log event\.  | 
|  Pod |  `pod_memory_reserved_capacity`  |  Calculated  |  Formula: `pod_memory_request / node_memory_limit`  | 
|  Pod |  `pod_network_tx_bytes`  |  Calculated  |  Formula: `sum(pod_interface_network_tx_bytes)` This data is available for all the network interfaces per pod\. The CloudWatch agent calculates the total and adds metric extraction rules\.  | 
|  Pod |  `pod_network_rx_bytes`  |  Calculated  |  Formula: `sum(pod_interface_network_rx_bytes)`  | 
|  Pod |  `pod_network_total_bytes`  |  Calculated  |  Formula: `pod_network_rx_bytes + pod_network_tx_bytes`  | 
|  PodNet |  `pod_interface_network_rx_bytes`  |  cadvisor  | This data is network rx bytes per second of a pod network interface\.  | 
|  PodNet |  `pod_interface_network_tx_bytes`  |  cadvisor  | This data is network tx bytes per second of a pod network interface\. | 
|  Container |  `container_cpu_usage_total`  |  cadvisor  |   | 
|  Container |  `container_cpu_limit`  |  cadvisor  |  Not guaranteed to be set\. It's not emitted if it's not set\. | 
|  Container |  `container_cpu_request`  |  cadvisor  |  Not guaranteed to be set\. It's not emitted if it's not set\. | 
|  Container |  `container_memory_working_set`  |  cadvisor  |   | 
|  Container |  `container_memory_limit`  |  pod  |  Not guaranteed to be set\. It's not emitted if it's not set\. | 
|  Container |  `container_memory_request`  |  pod  |  Not guaranteed to be set\. It's not emitted if it's not set\. | 
|  ContainerFS  |  `container_filesystem_capacity`  |  pod  |  This data is available per disk device\. | 
|  ContainerFS |  `container_filesystem_usage`  |  pod  |  This data is available per disk device\. | 
|  ContainerFS |  `container_filesystem_utilization`  |  Calculated  |  Formula: `container_filesystem_usage / container_filesystem_capacity` This data is available per device name\. | 
|  Node |  `node_cpu_utilization`  |  Calculated  |  Formula: `node_cpu_usage_total / node_cpu_limit`  | 
|  Node |  `node_cpu_usage_total`  |  cadvisor  |   | 
|  Node |  `node_cpu_limit`  |  /proc  |   | 
|  Node |  `node_cpu_request`  |  Calculated  | Formula: `sum(pod_cpu_request)`  | 
|  Node |  `node_cpu_reserved_capacity`  |  Calculated  | Formula: `node_cpu_request / node_cpu_limit`  | 
|  Node |  `node_memory_utilization`  |  Calculated  | Formula: `node_memory_working_set / node_memory_limit`  | 
|  Node |  `node_memory_working_set`  |  cadvisor  |   | 
|  Node |  `node_memory_limit`  |  /proc  |   | 
|  Node |  `node_memory_request`  |  Calculated  |  Formula: `sum(pod_memory_request)`  | 
|  Node |  `node_memory_reserved_capacity`  |  Calculated  | Formula: `node_memory_request / node_memory_limit`  | 
|  Node |  `node_network_rx_bytes`  |  Calculated  | Formula: `sum(node_interface_network_rx_bytes)`  | 
|  Node |  `node_network_tx_bytes`  |  Calculated  | Formula: `sum(node_interface_network_tx_bytes)`  | 
|  Node |  `node_network_total_bytes`  |  Calculated  | Formula: `node_network_rx_bytes + node_network_tx_bytes`  | 
|  Node |  `node_number_of_running_pods`  |  Pod List  |   | 
|  Node |  `node_number_of_running_containers`  |  Pod List  |   | 
|  NodeNet |  `node_interface_network_rx_bytes`  |  cadvisor  |  This data is network rx bytes per second of a worker node network interface\.  | 
|  NodeNet |  `node_interface_network_tx_bytes`  |  cadvisor  |  This data is network tx bytes per second of a worker node network interface\.  | 
|  NodeFS |  `node_filesystem_capacity`  |  cadvisor  |   | 
|  NodeFS |  `node_filesystem_usage`  |  cadvisor  |   | 
|  NodeFS |  `node_filesystem_utilization`  |  Calculated  |  Formula: `node_filesystem_usage / node_filesystem_capacity` This data is available per device name\.  | 
|  Cluster |  `cluster_failed_node_count`  |  API Server  |   | 
|  Cluster |  `cluster_node_count`  |  API Server  |   | 
|  Service |  `service_number_of_running_pods`  |  API Server  |   | 
|  Namespace |  `namespace_number_of_running_pods`  |  API Server  |   | 

## Metrics Calculation Examples<a name="Container-Insights-calculation-examples"></a>

This section includes examples that show how some of the values in the preceding table are calculated\.

Suppose that you have a cluster in the following state\.

```
Node1
   node_cpu_limit = 4
   node_cpu_usage_total = 3
   
   Pod1
     pod_cpu_usage_total = 2
     
     Container1
        container_cpu_limit = 1
        container_cpu_request = 1
        container_cpu_usage_total = 0.8
        
     Container2
        container_cpu_limit = null
        container_cpu_request = null
        container_cpu_usage_total = 1.2
        
   Pod2
     pod_cpu_usage_total = 0.4
     
     Container3
        container_cpu_limit = 1
        container_cpu_request = 0.5
        container_cpu_usage_total = 0.4
        
Node2
   node_cpu_limit = 8
   node_cpu_usage_total = 1.5
   
   Pod3
     pod_cpu_usage_total = 1
     
     Container4
        container_cpu_limit = 2
        container_cpu_request = 2
        container_cpu_usage_total = 1
```

The following table shows how pod CPU metrics are calculated using this data\.


| Metric | Formula | Pod1 | Pod2 | Pod3 | 
| --- | --- | --- | --- | --- | 
|  `pod_cpu_utilization` |  `pod_cpu_usage_total / node_cpu_limit`  |  2 / 4 = 50%  |  0\.4 / 4 = 10%  |  1 / 8 = 12\.5%  | 
|  `pod_cpu_utilization_over_pod_limit` |  `pod_cpu_usage_total / sum(container_cpu_limit)`  |  N/A because CPU limit for `Container2` isn't defined  |  0\.4 / 1 = 40%  |  1 / 2 = 50%  | 
|  `pod_cpu_reserved_capacity` |  `sum(container_cpu_request) / node_cpu_limit`  |  \(1 \+ 0\) / 4 = 25%  |  0\.5 / 4 = 12\.5%  |  2 / 8 = 25%  | 

The following table shows how node CPU metrics are calculated using this data\.


| Metric | Formula | Node1 | Node2 | 
| --- | --- | --- | --- | 
|  `node_cpu_utilization` |  `node_cpu_usage_total / node_cpu_limit`  |  3 / 4 = 75%  |  1\.5 / 8 = 18\.75%  | 
|  `node_cpu_reserved_capacity` |  `sum(pod_cpu_request) / node_cpu_limit`  |  1\.5 / 4 = 37\.5%  |  2 / 8 = 25%  | 