# Setting up Container Insights on Amazon EKS and Kubernetes<a name="deploy-container-insights-EKS"></a>

The overall process for setting up Container Insights on Amazon EKS or Kubernetes is as follows:

1. Verify that you have the necessary prerequisites\.

1. Set up the CloudWatch agent or the AWS Distro for OpenTelemetry as a DaemonSet on your cluster to send metrics to CloudWatch\. 

   Set up Fluent Bit or Fluentd to send logs to CloudWatch Logs\.

   You can perform these steps at once as part of the quick start setup if you are using the CloudWatch agent, or do them separately\.

1. \(Optional\) Set up Amazon EKS control plane logging\.

1. \(Optional\) Set up the CloudWatch agent as a StatsD endpoint on the cluster to send StatsD metrics to CloudWatch\.

1. \(Optional\) Enable App Mesh Envoy Access Logs\.

**Topics**
+ [Verify prerequisites](Container-Insights-prerequisites.md)
+ [Using the CloudWatch agent](Container-Insights-EKS-agent.md)
+ [Using AWS Distro for OpenTelemetry](Container-Insights-EKS-otel.md)
+ [Send logs to CloudWatch Logs](Container-Insights-EKS-logs.md)
+ [Updating or deleting Container Insights on Amazon EKS and Kubernetes](ContainerInsights-update-delete.md)