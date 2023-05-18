# Sample Java/JMX workload for Amazon ECS clusters<a name="ContainerInsights-Prometheus-Sample-Workloads-ECS-javajmx"></a>

JMX Exporter is an official Prometheus exporter that can scrape and expose JMX mBeans as Prometheus metrics\. For more information, see [prometheus/jmx\_exporter](https://github.com/prometheus/jmx_exporter)\.

The CloudWatch agent with Prometheus support scrapes the Java/JMX Prometheus metrics based on the service discovery configuration in the Amazon ECS cluster\. You can configure the JMX Exporter to expose the metrics on a different port or metrics\_path\. If you do change the port or path, update the default `ecs_service_discovery` section in the CloudWatch agent configuration\.

To collect metrics from a sample Prometheus workload for Amazon ECS, you must be running Container Insights in the cluster\. For information about installing Container Insights, see [Setting up Container Insights on Amazon ECS](deploy-container-insights-ECS.md)\.

**To install the Java/JMX sample workload for Amazon ECS clusters**

1. Follow the steps in these sections to create your Docker images\.
   + [ Example: Java Jar Application Docker image with Prometheus metrics](ContainerInsights-Prometheus-Sample-Workloads-javajmx.md#ContainerInsights-Prometheus-Sample-Workloads-javajmx-jar)
   + [ Example: Apache Tomcat Docker image with Prometheus metrics](ContainerInsights-Prometheus-Sample-Workloads-javajmx.md#ContainerInsights-Prometheus-Sample-Workloads-javajmx-tomcat)

1. Specify the following two docker labels in the Amazon ECS task definition file\. You can then run the task definition as an Amazon ECS service or Amazon ECS task in the cluster\.
   + Set `ECS_PROMETHEUS_EXPORTER_PORT` to point to the containerPort where the Prometheus metrics are exposed\.
   + Set `Java_EMF_Metrics` to `true`\. The CloudWatch agent uses this flag to generated the embedded metric format in the log event\.

   The following is an example:

   ```
   {
     "family": "workload-java-ec2-bridge",
     "taskRoleArn": "{{task-role-arn}}",
     "executionRoleArn": "{{execution-role-arn}}",
     "networkMode": "bridge",
     "containerDefinitions": [
       {
         "name": "tomcat-prometheus-workload-java-ec2-bridge-dynamic-port",
         "image": "your_docker_image_tag_for_tomcat_with_prometheus_metrics",
         "portMappings": [
           {
             "hostPort": 0,
             "protocol": "tcp",
             "containerPort": 9404
           }
         ],
         "dockerLabels": {
           "ECS_PROMETHEUS_EXPORTER_PORT": "9404",
           "Java_EMF_Metrics": "true"
         }
       }
     ],
     "requiresCompatibilities": [
       "EC2"  ],
     "cpu": "256",
     "memory": "512"
     }
   ```

The default setting of the CloudWatch agent in the AWS CloudFormation template enables both docker label\-based service discovery and task definition ARN\-based service discovery\. To view these default settings, see line 65 of the [ CloudWatch agent YAML configuration file](https://github.com/aws-samples/amazon-cloudwatch-container-insights/blob/latest/ecs-task-definition-templates/deployment-mode/replica-service/cwagent-prometheus/cloudformation-quickstart/cwagent-ecs-prometheus-metric-for-bridge-host.yaml#L65)\. The containers with the `ECS_PROMETHEUS_EXPORTER_PORT` label will be auto\-discovered based on the specified container port for Prometheus scraping\. 

The default setting of the CloudWatch agent also has the `metric_declaration` setting for Java/JMX at line 112 of the same file\. All docker labels of the target containers will be added as additional labels in the Prometheus metrics and sent to CloudWatch Logs\. For the Java/JMX containers with docker label `Java_EMF_Metrics=“true”`, the embedded metric format will be generated\. 