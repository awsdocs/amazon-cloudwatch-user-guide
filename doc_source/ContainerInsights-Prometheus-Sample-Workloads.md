# \(Optional\) Set up sample containerized Amazon EKS workloads for Prometheus metric testing<a name="ContainerInsights-Prometheus-Sample-Workloads"></a>

To test the Prometheus metric support in CloudWatch Container Insights, you can set up one or more of the following containerized workloads\. The CloudWatch agent with Prometheus support automatically collects metrics from each of these workloads\. To see the metrics that are collected by default, see [Prometheus metrics collected by the CloudWatch agent](ContainerInsights-Prometheus-metrics.md)\.

Before you can install any of these workloads, you must install Helm 3\.x by entering the following commands:

```
brew install helm
```

For more information, see [Helm](https://helm.sh)\.

**Topics**
+ [Set up AWS App Mesh sample workload for Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-appmesh.md)
+ [Set up NGINX with sample traffic on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-nginx.md)
+ [Set up memcached with a metric exporter on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-memcached.md)
+ [Set up Java/JMX sample workload on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-javajmx.md)
+ [Set up HAProxy with a metric exporter on Amazon EKS and Kubernetes](ContainerInsights-Prometheus-Sample-Workloads-haproxy.md)
+ [Tutorial for adding a new Prometheus scrape target: Redis on Amazon EKS and Kubernetes clusters](ContainerInsights-Prometheus-Setup-redis-eks.md)