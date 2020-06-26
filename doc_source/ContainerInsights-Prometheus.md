# Container Insights Prometheus Metrics Monitoring<a name="ContainerInsights-Prometheus"></a>


****  

|  | 
| --- |
| Support for Prometheus metrics is in beta\. The beta is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

CloudWatch Container Insights monitoring for Prometheus automates the discovery of Prometheus metrics from containerized systems and workloads\. Prometheus is an open\-source systems monitoring and alerting toolkit\. For more information, see [ What is Prometheus?](https://prometheus.io/docs/introduction/overview/) in the Prometheus documentation\.

For this beta, discovering Prometheus metrics is supported for [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) and [Kubernetes](https://aws.amazon.com/kubernetes/) clusters runnning on Amazon EC2 instances\. In this beta release, only the Prometheus counter and gauge metric types are collected\. Support for histogram and summary metrics is planned for an upcoming release\.

The Container Insights Prometheus solution automatically collects metrics from the following containerized workloads and systems, and you can configure it to collect Prometheus metrics from other containerized services and applications\.
+ AWS App Mesh
+ NGINX
+ Memcached
+ Java/JMX
+ HAProxy

You can adopt Prometheus as an open\-source and open\-standard method to ingest custom metrics in CloudWatch\. The CloudWatch agent with Prometheus support discovers and collects Prometheus metrics to monitor, troubleshoot, and alarm on application performance degradation and failures faster\. This also reduces the number of monitoring tools required to improve observability\.

Container Insights Prometheus support involves pay\-per\-use of metrics and logs, including collecting, storing, and analyzing\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.