# Setting Up Container Insights on Amazon EKS and Kubernetes<a name="deploy-container-insights-EKS"></a>

The overall process for setting up Container Insights on Amazon EKS or Kubernetes is as follows:

1. Verify that you have the necessary prerequisites\.

1. Set up the CloudWatch agent as a DaemonSet on your Amazon EKS cluster or Kubernetes cluster to send metrics to CloudWatch\.

1. Set up FluentD as a DaemonSet on your cluster to send logs to CloudWatch Logs\.

1. \(Optional\) Set up Amazon EKS control plane logging\.

1. \(Optional\) Set up the CloudWatch agent as a StatsD endpoint on the cluster to send StatsD metrics to CloudWatch\.

**Topics**
+ [Verify Prerequisites](Container-Insights-prerequisites.md)
+ [Set Up the CloudWatch Agent to Collect Cluster Metrics](Container-Insights-setup-metrics.md)
+ [Set Up FluentD as a DaemonSet to Send Logs to CloudWatch Logs](Container-Insights-setup-logs.md)
+ [\(Optional\) Set Up Amazon EKS Control Plane Logging](Container-Insights-setup-control-plane-logging.md)
+ [\(Optional\) Set Up the CloudWatch Agent as a StatsD Endpoint to Send StatsD Metrics to CloudWatch](Container-Insights-setup-StatsD.md)
+ [Updating the CloudWatch Agent Container Image](ContainerInsights-update-image.md)
+ [Deleting the CloudWatch Agent](ContainerInsights-delete-agent.md)