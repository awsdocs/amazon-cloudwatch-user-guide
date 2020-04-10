# Setting Up Container Insights on Amazon ECS<a name="deploy-container-insights-ECS"></a>

You can use one or both of the following options to enable Container Insights on Amazon ECS clusters:
+ Use the AWS Management Console or the AWS CLI to start collecting cluster\-level, task\-level, and service\-level metrics\.
+ Deploy the CloudWatch agent as a DaemonSet to start collecting of instance\-level metrics on clusters that are hosted on Amazon EC2 instances\.

**Topics**
+ [Setting Up Container Insights on Amazon ECS for Cluster\- and Service\-Level Metrics](deploy-container-insights-ECS-cluster.md)
+ [Deploying the CloudWatch Agent to Collect EC2 Instance\-Level Metrics on Amazon ECS](deploy-container-insights-ECS-instancelevel.md)