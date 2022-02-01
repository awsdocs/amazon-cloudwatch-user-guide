# Using Container Insights<a name="ContainerInsights"></a>

Use CloudWatch Container Insights to collect, aggregate, and summarize metrics and logs from your containerized applications and microservices\. Container Insights is available for Amazon Elastic Container Service \(Amazon ECS\), Amazon Elastic Kubernetes Service \(Amazon EKS\), and Kubernetes platforms on Amazon EC2\. Amazon ECS support includes support for Fargate\.

CloudWatch automatically collects metrics for many resources, such as CPU, memory, disk, and network\. Container Insights also provides diagnostic information, such as container restart failures, to help you isolate issues and resolve them quickly\. You can also set CloudWatch alarms on metrics that Container Insights collects\.

Container Insights collects data as *performance log events* using [embedded metric format](CloudWatch_Embedded_Metric_Format.md)\. These performance log events are entries that use a structured JSON schema that enables high\-cardinality data to be ingested and stored at scale\. From this data, CloudWatch creates aggregated metrics at the cluster, node, pod, task, and service level as CloudWatch metrics\. The metrics that Container Insights collects are available in CloudWatch automatic dashboards, and also viewable in the **Metrics** section of the CloudWatch console\.

CloudWatch does not automatically create all possible metrics from the log data, to help you manage your Container Insights costs\. However, you can view additional metrics and additional levels of granularity by using CloudWatch Logs Insights to analyze the raw performance log events\.

Metrics collected by Container Insights are charged as custom metrics\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

In Amazon EKS and Kubernetes, Container Insights uses a containerized version of the CloudWatch agent to discover all of the running containers in a cluster\. It then collects performance data at every layer of the performance stack\.

Container Insights supports encryption with the customer master key \(CMK\) for the logs and metrics that it collects\. To enable this encryption, you must manually enable KMS encryption for the log group that receives Container Insights data\. This results in Container Insights encrypting this data using the provided CMK\. Only symmetric CMKs are supported\. Do not use asymmetric CMKs to encrypt your log groups\.

For more information, see [Encrypt Log Data in CloudWatch Logs Using AWS KMS](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html)\.

## Supported platforms<a name="container-insights-platforms"></a>

Container Insights is available for Amazon Elastic Container Service, Amazon Elastic Kubernetes Service, and Kubernetes platforms on Amazon EC2 instances\.
+ For Amazon ECS, Container Insights collects metrics at the cluster, task and service levels on both Linux and Windows Server instances\. It can collect metrics at the instance\-level only on Linux instances\.

  For Amazon ECS, network metrics are available only for containers in `bridge` network mode and `awsvpc` network mode\. They are not available for containers in `host` network mode\.
+ For Amazon Elastic Kubernetes Service, and Kubernetes platforms on Amazon EC2 instances, Container Insights is supported only on Linux instances\.
+ Currently, Container Insights isn't supported in AWS Batch\.

## CloudWatch agent container image<a name="container-insights-download-limit"></a>

Amazon provides a CloudWatch agent container image on Amazon Elastic Container Registry\. For more information, see [cloudwatch\-agent](https://gallery.ecr.aws/cloudwatch-agent/cloudwatch-agent) on Amazon ECR\.

## Supported Regions<a name="container-insights-regions"></a>

Container Insights for Amazon ECS is supported in the following Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Africa \(Cape Town\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Tokyo\)
+ Asia Pacific \(Sydney\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ Europe \(Stockholm\)
+ Middle East \(Bahrain\)
+ South America \(São Paulo\)
+ AWS GovCloud \(US\-East\)
+ AWS GovCloud \(US\-West\)
+ China \(Beijing\)
+ China \(Ningxia\)

**Supported Regions for Amazon EKS and Kubernetes**

Container Insights for Amazon EKS and Kubernetes is supported in the following Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Canada \(Central\)
+ China \(Beijing\)
+ China \(Ningxia\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ Europe \(Stockholm\)
+ South America \(São Paulo\)
+ AWS GovCloud \(US\-East\)
+ AWS GovCloud \(US\-West\)

**Topics**