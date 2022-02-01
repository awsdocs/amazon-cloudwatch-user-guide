# Set up and configure Prometheus metrics collection on Amazon ECS clusters<a name="ContainerInsights-Prometheus-Setup-ECS"></a>

To collect Prometheus metrics from Amazon ECS clusters, you can use the CloudWatch agent as a collector or use the AWS Distro for OpenTelemetry collector\. For information about using the AWS Distro for OpenTelemetry collector, see [https://aws\-otel\.github\.io/docs/getting\-started/container\-insights/ecs\-prometheus](https://aws-otel.github.io/docs/getting-started/container-insights/ecs-prometheus)\.

The following sections explain how to use the CloudWatch agent as the collector to retrieve Prometheus metrics\. You install the CloudWatch agent with Prometheus monitoring on clusters running Amazon ECS, and you can optionally configure the agent to scrape additional targets\. These sections also provide optional tutorials for setting up sample workloads to use for testing with Prometheus monitoring\. 

Container Insights on Amazon ECS supports the following launch type and network mode combinations for Prometheus metrics:


| Amazon ECS launch type | Network modes supported | 
| --- | --- | 
|  EC2 \(Linux\)  |  bridge, host, and awsvpc  | 
|  Fargate  |  awsvpc  | 

**VPC security group requirements**

The ingress rules of the security groups for the Prometheus workloads must open the Prometheus ports to the CloudWatch agent for scraping the Prometheus metrics by the private IP\.

The egress rules of the security group for the CloudWatch agent must allow the CloudWatch agent to connect to the Prometheus workloads' port by private IP\. 

**Topics**
+ [Install the CloudWatch agent with Prometheus metrics collection on Amazon ECS clusters](ContainerInsights-Prometheus-install-ECS.md)
+ [Scraping additional Prometheus sources and importing those metrics](ContainerInsights-Prometheus-Setup-configure-ECS.md)
+ [\(Optional\) Set up sample containerized Amazon ECS workloads for Prometheus metric testing](ContainerInsights-Prometheus-Sample-Workloads-ECS.md)