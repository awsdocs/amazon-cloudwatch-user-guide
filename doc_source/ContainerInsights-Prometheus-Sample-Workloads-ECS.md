# \(Optional\) Set up sample containerized Amazon ECS workloads for Prometheus metric testing<a name="ContainerInsights-Prometheus-Sample-Workloads-ECS"></a>

To test the Prometheus metric support in CloudWatch Container Insights, you can set up one or more of the following containerized workloads\. The CloudWatch agent with Prometheus support automatically collects metrics from each of these workloads\. To see the metrics that are collected by default, see [Prometheus metrics collected by the CloudWatch agent](ContainerInsights-Prometheus-metrics.md)\.

**Topics**
+ [Sample App Mesh workload for Amazon ECS clusters](ContainerInsights-Prometheus-Sample-Workloads-ECS-appmesh.md)
+ [Sample Java/JMX workload for Amazon ECS clusters](ContainerInsights-Prometheus-Sample-Workloads-ECS-javajmx.md)
+ [Sample NGINX workload for Amazon ECS clusters](ContainerInsights-Prometheus-Setup-nginx-ecs.md)
+ [Sample NGINX Plus workload for Amazon ECS clusters](ContainerInsights-Prometheus-Setup-nginx-plus-ecs.md)
+ [Tutorial for adding a new Prometheus scrape target: Memcached on Amazon ECS](ContainerInsights-Prometheus-Setup-memcached-ecs.md)
+ [Tutorial for scraping Redis Prometheus metrics on Amazon ECS Fargate](ContainerInsights-Prometheus-Setup-redis-ecs.md)