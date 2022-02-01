# Set up and configure Prometheus metrics collection on Amazon EKS and Kubernetes clusters<a name="ContainerInsights-Prometheus-install-EKS"></a>

To collect Prometheus metrics from clusters running Amazon EKS or Kubernetes, you can use the CloudWatch agent as a collector or use the AWS Distro for OpenTelemetry collector\. For information about using the AWS Distro for OpenTelemetry collector, see [https://aws\-otel\.github\.io/docs/getting\-started/container\-insights/eks\-prometheus](https://aws-otel.github.io/docs/getting-started/container-insights/eks-prometheus)\.

The following sections explain how to collect Prometheus metrics using the CloudWatch agent\. They explain how to install the CloudWatch agent with Prometheus monitoring on clusters running Amazon EKS or Kubernetes, and how to configure the agent to scrape additional targets\. They also provide optional tutorials for setting up sample workloads to use for testing with Prometheus monitoring\.

**Topics**
+ [Install the CloudWatch agent with Prometheus metrics collection on Amazon EKS and Kubernetes clusters](ContainerInsights-Prometheus-Setup.md)
+ [Prometheus metric type conversion by the CloudWatch Agent](ContainerInsights-Prometheus-metrics-conversion.md)
+ [Prometheus metrics collected by the CloudWatch agent](ContainerInsights-Prometheus-metrics.md)
+ [Viewing your Prometheus metrics](ContainerInsights-Prometheus-viewmetrics.md)
+ [Prometheus metrics troubleshooting](ContainerInsights-Prometheus-troubleshooting.md)