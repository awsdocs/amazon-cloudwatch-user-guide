# Setting up Container Insights on Amazon ECS<a name="deploy-container-insights-ECS"></a>

You can use one or both of the following options to enable Container Insights on Amazon ECS clusters:
+ Use the AWS Management Console or the AWS CLI to start collecting cluster\-level, task\-level, and service\-level metrics\.
+ Deploy the CloudWatch agent as a DaemonSet to start collecting of instance\-level metrics on clusters that are hosted on Amazon EC2 instances\.

**Topics**
+ [Setting up Container Insights on Amazon ECS for cluster\- and service\-level metrics](deploy-container-insights-ECS-cluster.md)
+ [Setting up Container Insights on Amazon ECS using AWS Distro for OpenTelemetry](deploy-container-insights-ECS-adot.md)
+ [Deploying the CloudWatch agent to collect EC2 instance\-level metrics on Amazon ECS](deploy-container-insights-ECS-instancelevel.md)
+ [Deploying the AWS Distro for OpenTelemetry to collect EC2 instance\-level metrics on Amazon ECS clusters](deploy-container-insights-ECS-OTEL.md)
+ [Set up Firelens to send logs to CloudWatch Logs](deploy-container-insights-ECS-logs.md)