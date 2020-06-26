# Deploying Other CloudWatch Agent Features in Your Containers<a name="ContainerInsights-other-agent-features"></a>

You can deploy additional monitoring features in your containers using the CloudWatch agent\. These features include the following:
+ **Embedded Metric Format**— For more information, see [Ingesting High\-Cardinality Logs and Generating Metrics with CloudWatch Embedded Metric Format](CloudWatch_Embedded_Metric_Format.md)\.
+ **StatsD**— For more information, see [Retrieve Custom Metrics with StatsD ](CloudWatch-Agent-custom-metrics-statsd.md)\.

Instructions and necessary files are located on GitHub at the following locations:
+ For Amazon ECS containers, see [ Example Amazon ECS task definitions based on deployment modes](https://github.com/aws-samples/amazon-cloudwatch-container-insights/tree/latest/ecs-task-definition-templates/deployment-mode)\.
+ For Amazon EKS and Kubernetes containers, see [ Example Kubernetes YAML files based on deployment modes](https://github.com/aws-samples/amazon-cloudwatch-container-insights/tree/latest/k8s-deployment-manifest-templates/deployment-mode)\.