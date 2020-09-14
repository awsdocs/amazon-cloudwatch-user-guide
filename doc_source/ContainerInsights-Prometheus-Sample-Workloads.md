# \(Optional\) Set Up Sample Containerized Workloads for Prometheus Metric Testing<a name="ContainerInsights-Prometheus-Sample-Workloads"></a>

To test the Prometheus metric support in CloudWatch Container Insights, you can set up one or more of the following containerized workloads\. The CloudWatch agent with Prometheus support automatically collects metrics from each of these workloads\. To see the metrics that are collected by default, see [Prometheus Metrics Collected by the CloudWatch Agent](ContainerInsights-Prometheus-metrics.md)\.

Before you can install any of these workloads, you must install Helm 3\.x by entering the following commands:

```
brew install helm
helm repo add stable https://kubernetes-charts.storage.googleapis.com
```

For more information, see [Helm](https://helm.sh)\.

**Sample workloads for Amazon EKS and Kubernetes clusters**
+ [\(Optional\) Set Up AWS App Mesh sample workload for Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-appmesh.md)
+ [\(Optional\) Set Up NGINX with Sample Traffic on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-nginx.md)
+ [\(Optional\) Set Up memcached with a Metric Exporter on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-memcached.md)
+ [\(Optional\) Set Up Java/JMX sample workload on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-javajmx.md)
+ [\(Optional\) Set Up HAProxy with a Metric Exporter on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-haproxy.md)

**Sample workloads for Amazon ECS clusters**
+ [\(Optional\) Sample App Mesh Workload for Amazon ECS clusters](ContainerInsights-Prometheus-Sample-Workloads-ECS-appmesh.md)
+ [\(Optional\) Sample Java/JMX Workload for Amazon ECS clusters](ContainerInsights-Prometheus-Sample-Workloads-ECS-javajmx.md)