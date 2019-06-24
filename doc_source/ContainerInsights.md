# Using Container Insights<a name="ContainerInsights"></a>


****  

|  | 
| --- |
| CloudWatch Container Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

If you use Amazon Elastic Container Service for Kubernetes or other Kubernetes \(K8s\) platforms on Amazon EC2, you can use CloudWatch Container Insights to collect, aggregate, and summarize metrics and logs from your containerized applications and microservices\. The metrics include utilization for resources such as CPU, memory, disk, and network\. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly\. You can also set CloudWatch alarms on metrics that Container Insights collects\.

The metrics that Container Insights collects are available in CloudWatch automatic dashboards\. You can view further container data with CloudWatch Logs Insights\.

Container Insights uses a containerized version of the CloudWatch agent to discover all the running containers in a cluster and collect performance data at every layer of the performance stack\. Operational data is collected as *performance log events*\. These are entries that use a structured JSON schema that enables high\-cardinality data to be ingested and stored at scale\. From this data, CloudWatch creates higher\-level aggregated metrics at the cluster, node, pod, and service level as CloudWatch metrics\.

This preview release currently supports Container Insights only in the following Regions:
+ Canada \(Central\)
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ EU \(Frankfurt\)
+ EU \(Ireland\)
+ EU \(Paris\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Asia Pacific \(Seoul\)
+ South America \(São Paulo\)

**Topics**
+ [Setting Up Container Insights](deploy-container-insights.md)
+ [Viewing Container Insights Metrics](Container-Insights-view-metrics.md)
+ [Metrics Collected by Container Insights](Container-Insights-metrics.md)
+ [Relevant Fields in Performance Log Events](Container-Insights-reference-performance-entries.md)
+ [Updating the CloudWatch Agent Container Image](ContainerInsights-update-image.md)
+ [Deleting the CloudWatch Agent](ContainerInsights-delete-agent.md)