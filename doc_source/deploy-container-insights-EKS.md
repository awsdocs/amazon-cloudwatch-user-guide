# Setting Up Container Insights on Amazon EKS and Kubernetes<a name="deploy-container-insights-EKS"></a>

The overall process for setting up Container Insights on Amazon EKS or Kubernetes is as follows:

1. Verify that you have the necessary prerequisites\.

1. Set up the CloudWatch agent as a DaemonSet on your Amazon EKS cluster or Kubernetes cluster to send metrics to CloudWatch, and set up FluentD as a DaemonSet to send logs to CloudWatch Logs\.

   You can perform these steps at once as part of the quick start setup, or do them separately\.

1. \(Optional\) Set up Amazon EKS control plane logging\.

1. \(Optional\) Set up the CloudWatch agent as a StatsD endpoint on the cluster to send StatsD metrics to CloudWatch\.

1. \(Optional\) Enable App Mesh Envoy Access Logs\.

**Topics**
+ [Verify Prerequisites](Container-Insights-prerequisites.md)
+ [Quick Start Setup for Container Insights on Amazon EKS and Kubernetes](Container-Insights-setup-EKS-quickstart.md)
+ [Set Up the CloudWatch Agent to Collect Cluster Metrics](Container-Insights-setup-metrics.md)
+ [Send logs to CloudWatch Logs](Container-Insights-EKS-logs.md)
+ [Updating or Deleting Container Insights on Amazon EKS and Kubernetes](ContainerInsights-update-delete.md)