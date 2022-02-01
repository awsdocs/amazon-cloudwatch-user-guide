# Container Insights Prometheus metrics monitoring<a name="ContainerInsights-Prometheus"></a>

CloudWatch Container Insights monitoring for Prometheus automates the discovery of Prometheus metrics from containerized systems and workloads\. Prometheus is an open\-source systems monitoring and alerting toolkit\. For more information, see [ What is Prometheus?](https://prometheus.io/docs/introduction/overview/) in the Prometheus documentation\.

Discovering Prometheus metrics is supported for [Amazon Elastic Container Service](https://aws.amazon.com/ecs/), [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) and [Kubernetes](https://aws.amazon.com/kubernetes/) clusters running on Amazon EC2 instances\. The Prometheus counter, gauge, and summary metric types are collected\. Support for histogram metrics is planned for an upcoming release\.

For Amazon ECS and Amazon EKS clusters, both the EC2 and Fargate launch types are supported\. Container Insights automatically collects metrics from several workloads, and you can configure it to collect metrics from any workload\.

You can adopt Prometheus as an open\-source and open\-standard method to ingest custom metrics in CloudWatch\. The CloudWatch agent with Prometheus support discovers and collects Prometheus metrics to monitor, troubleshoot, and alarm on application performance degradation and failures faster\. This also reduces the number of monitoring tools required to improve observability\.

Container Insights Prometheus support involves pay\-per\-use of metrics and logs, including collecting, storing, and analyzing\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

**Pre\-built dashboards for some workloads**

The Container Insights Prometheus solution includes pre\-built dashboards for the popular workloads that are listed in this section\. For sample configurations for these workloads, see [\(Optional\) Set up sample containerized Amazon ECS workloads for Prometheus metric testing](ContainerInsights-Prometheus-Sample-Workloads-ECS.md) and [\(Optional\) Set up sample containerized Amazon EKS workloads for Prometheus metric testing](ContainerInsights-Prometheus-Sample-Workloads.md)\.

You can also configure Container Insights to collect Prometheus metrics from other containerized services and applications by editing the agent configuration file\.

Workloads with pre\-built dashboards for Amazon EKS clusters and Kubernetes clusters running on Amazon EC2 instances:
+ AWS App Mesh
+ NGINX
+ Memcached
+ Java/JMX
+ HAProxy

Workloads with pre\-built dashboards for Amazon ECS clusters:
+ AWS App Mesh
+ Java/JMX
+ NGINX
+ NGINX Plus