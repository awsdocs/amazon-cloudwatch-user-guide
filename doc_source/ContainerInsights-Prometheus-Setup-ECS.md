# Set Up and Configure Prometheus Metrics Collection on Amazon ECS clusters<a name="ContainerInsights-Prometheus-Setup-ECS"></a>

The following sections explain how to install the CloudWatch agent with Prometheus monitoring on clusters running Amazon ECS, and how to configure the agent to scrape additional targets\. It also provides optional tutorials for setting up sample workloads to use for testing with Prometheus monitoring\. 

Container Insights on Amazon ECS supports the following launch type and network mode combinations for Prometheus metrics:


| Amazon ECS launch type | Network modes supported | 
| --- | --- | 
|  EC2 \(Linux\)  |  bridge, host, and awsvpc  | 
|  Fargate  |  awsvpc  | 

**VPC Security Group Requirements**

The ingress rules of the security groups for the Prometheus workloads must open the Prometheus ports to the CloudWatch agent for scraping the Prometheus metrics by the private IP\.

The egress rules of the security group for the CloudWatch agent must allow the CloudWatch agent to connect to the Prometheus workloads' port by private IP\. 

**Topics**
+ [Install the CloudWatch Agent with Prometheus Metrics Collection on Amazon ECS clusters](ContainerInsights-Prometheus-install-ECS.md)
+ [Scraping Additional Prometheus sources and Importing Those Metrics](ContainerInsights-Prometheus-Setup-configure-ECS.md)
+ [\(Optional\) Set Up Sample Containerized Workloads for Prometheus Metric Testing](ContainerInsights-Prometheus-Sample-Workloads-ECS.md)