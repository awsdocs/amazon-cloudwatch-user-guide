# Using Container Insights<a name="ContainerInsights"></a>

Use CloudWatch Container Insights to collect, aggregate, and summarize metrics and logs from your containerized applications and microservices\. Container Insights is available for Amazon Elastic Container Service \(Amazon ECS\), Amazon Elastic Kubernetes Service \(Amazon EKS\), and Kubernetes platforms on Amazon EC2\. Amazon ECS support includes support for Fargate\.

The metrics include utilization for resources such as CPU, memory, disk, and network\. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly\. You can also set CloudWatch alarms on metrics that Container Insights collects\.

The metrics that Container Insights collects are available in CloudWatch automatic dashboards\. You can analyze and troubleshoot container performance and logs data with CloudWatch Logs Insights\.

Operational data is collected as *performance log events*\. These are entries that use a structured JSON schema that enables high\-cardinality data to be ingested and stored at scale\. From this data, CloudWatch creates aggregated metrics at the cluster, node, pod, task, and service level as CloudWatch metrics\.

Metrics collected by Container Insights are charged as custom metrics\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

In Amazon EKS and Kubernetes, Container Insights uses a containerized version of the CloudWatch agent to discover all of the running containers in a cluster\. It then collects performance data at every layer of the performance stack\.

Container Insights supports encryption with the customer master key \(CMK\) for the logs and metrics that it collects\. To enable this encryption, you must manually enable KMS encryption for the log group that receives Container Insights data\. This results in Container Insights encrypting this data using the provided CMK\. Only symmetric CMKs are supported\. Do not use asymmetric CMKs to encrypt your log groups\.

For more information, see [Encrypt Log Data in CloudWatch Logs Using AWS KMS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html)\.

**Supported Platforms**

Container Insights is available for Amazon Elastic Container Service, Amazon Elastic Kubernetes Service, and Kubernetes platforms on Amazon EC2 instances\.
+ For Amazon ECS, Container Insights collects metrics at the cluster, task and service levels on both Linux and Windows Server instances\. It can collect metrics at the instance\-level only on Linux instances\.

  For Amazon ECS, network metrics are available only for containers in `bridge` network mode\. They are not available for containers in `awsvpc` network mode or `host` network mode\.
+ For Amazon Elastic Kubernetes Service, and Kubernetes platforms on Amazon EC2 instances, Container Insights is supported only on Linux instances\.
+ Currently, Container Insights isn't supported in AWS Batch\.

**Supported Regions for Amazon ECS**

Container Insights for Amazon ECS is supported in the following Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ Europe \(Stockholm\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Tokyo\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Mumbai\)
+ South America \(São Paulo\)

AWS Fargate is not supported in Europe \(Paris\) or South America \(São Paulo\)\.

**Supported Regions for Amazon EKS and Kubernetes**

Container Insights for Amazon EKS and Kubernetes is supported in the following Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ Europe \(Stockholm\)
+ Asia Pacific \(Hong Kong\)
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
+ [Container Insights Performance Log Reference](Container-Insights-reference.md)
+ [Container Insights Prometheus Metrics Monitoring](ContainerInsights-Prometheus.md)
+ [Troubleshooting Container Insights](ContainerInsights-troubleshooting.md)
+ [Building Your Own CloudWatch Agent Docker Image](ContainerInsights-build-docker-image.md)
+ [Deploying Other CloudWatch Agent Features in Your Containers](ContainerInsights-other-agent-features.md)