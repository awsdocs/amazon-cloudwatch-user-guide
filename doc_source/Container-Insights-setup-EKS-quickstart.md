# Quick Start Setup for Container Insights on Amazon EKS<a name="Container-Insights-setup-EKS-quickstart"></a>

To complete the setup of Container Insights, you can follow the quick start instructions in this section\.

Alternatively, you can instead follow the instructions in the following two sections, [Set Up the CloudWatch Agent to Collect Cluster Metrics](Container-Insights-setup-metrics.md) and [Set Up FluentD as a DaemonSet to Send Logs to CloudWatch Logs](Container-Insights-setup-logs.md)\. Those sections provide more details on how CloudWatch agent works with Amazon EKS and the configuration, but require you to perform more installation steps\.

To deploy Container Insights using the quick start, enter the following command\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/cluster-name/;s/{{region_name}}/cluster-region/" | kubectl apply -f -
```

In this command, *cluster\-name* is the name of your Amazon EKS or Kubernetes cluster, and *cluster\-region* is the name of the Region where the logs are published\. We recommend that you use the same Region where your cluster is deployed to reduce the AWS outbound data transfer costs\.

For example, to deploy Container Insights on the cluster named `MyCluster` and publish the logs and metrics to US West \(Oregon\), enter the following command\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/us-west-2/" | kubectl apply -f -
```

**Deleting Container Insights**

If you want to remove Container Insights after using the quick start setup, enter the following command\.

```
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluentd-quickstart.yaml | sed "s/{{cluster_name}}/cluster-name/;s/{{region_name}}/cluster-region/" | kubectl delete -f -
```