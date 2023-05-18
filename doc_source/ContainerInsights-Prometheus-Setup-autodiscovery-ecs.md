# Detailed guide for autodiscovery on Amazon ECS clusters<a name="ContainerInsights-Prometheus-Setup-autodiscovery-ecs"></a>

Prometheus provides dozens of dynamic service\-discovery mechanisms as described in [<scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config)\. However there is no built\-in service discovery for Amazon ECS\. The CloudWatch agent adds this mechanism\.

When the Amazon ECS Prometheus service discovery is enabled, the CloudWatch agent periodically makes the following API calls to Amazon ECS and Amazon EC2 frontends to retrieve the metadata of the running ECS tasks in the target ECS cluster\. 

```
EC2:DescribeInstances
ECS:ListTasks
ECS:ListServices
ECS:DescribeContainerInstances
ECS:DescribeServices
ECS:DescribeTasks
ECS:DescribeTaskDefinition
```

The metadata is used by the CloudWatch agent to scan the Prometheus targets within the ECS cluster\. The CloudWatch agent supports three service discovery modes:
+ Container docker label\-based service discovery
+ ECS task definition ARN regular expression\-based service discovery
+ ECS service name regular expression\-based service discovery

All modes can be used together\. CloudWatch agent de\-duplicates the discovered targets based on: `{private_ip}:{port}/{metrics_path}`\.

All discovered targets are written into a result file specified by the `sd_result_file` configuration field within the CloudWatch agent container\. The following is a sample result file: 

```
- targets:
  - 10.6.1.95:32785
  labels:
    __metrics_path__: /metrics
    ECS_PROMETHEUS_EXPORTER_PORT: "9406"
    ECS_PROMETHEUS_JOB_NAME: demo-jar-ec2-bridge-dynamic
    ECS_PROMETHEUS_METRICS_PATH: /metrics
    InstanceType: t3.medium
    LaunchType: EC2
    SubnetId: subnet-123456789012
    TaskDefinitionFamily: demo-jar-ec2-bridge-dynamic-port
    TaskGroup: family:demo-jar-ec2-bridge-dynamic-port
    TaskRevision: "7"
    VpcId: vpc-01234567890
    container_name: demo-jar-ec2-bridge-dynamic-port
    job: demo-jar-ec2-bridge-dynamic
- targets:
  - 10.6.3.193:9404
  labels:
    __metrics_path__: /metrics
    ECS_PROMETHEUS_EXPORTER_PORT_SUBSET_B: "9404"
    ECS_PROMETHEUS_JOB_NAME: demo-tomcat-ec2-bridge-mapped-port
    ECS_PROMETHEUS_METRICS_PATH: /metrics
    InstanceType: t3.medium
    LaunchType: EC2
    SubnetId: subnet-123456789012
    TaskDefinitionFamily: demo-tomcat-ec2-bridge-mapped-port
    TaskGroup: family:demo-jar-tomcat-bridge-mapped-port
    TaskRevision: "12"
    VpcId: vpc-01234567890
    container_name: demo-tomcat-ec2-bridge-mapped-port
    job: demo-tomcat-ec2-bridge-mapped-port
```

You can directly integrate this result file with Prometheus file\-based service discovery\. For more information about Prometheus file\-based service discovery, see [<file\_sd\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#file_sd_config)\.

 Suppose the result file is written to `/tmp/cwagent_ecs_auto_sd.yaml` The following Prometheus scrape configuration will consume it\.

```
global:
  scrape_interval: 1m
  scrape_timeout: 10s
scrape_configs:
  - job_name: cwagent-ecs-file-sd-config
    sample_limit: 10000
    file_sd_configs:
      - files: [ "/tmp/cwagent_ecs_auto_sd.yaml" ]
```

The CloudWatch agent also adds the following additional labels for the discovered targets\.
+ `container_name`
+ `TaskDefinitionFamily`
+ `TaskRevision`
+ `TaskGroup`
+ `StartedBy`
+ `LaunchType`
+ `job`
+ `__metrics_path__`
+ Docker labels

When the cluster has the EC2 launch type, the following three labels are added\.
+ `InstanceType`
+ `VpcId`
+ `SubnetId`

**Note**  
Docker labels that don't match the regular expression `[a-zA-Z_][a-zA-Z0-9_]*` are filtered out\. This matches the Prometheus conventions as listed in `label_name` in [Configuration file](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#labelname) in the Prometheus documentation\.

## ECS service discovery configuration examples<a name="ContainerInsights-Prometheus-Setup-autodiscovery-ecs-examples"></a>

This section includes examples that demonstrate ECS service discovery\.

**Example 1**

```
"ecs_service_discovery": {
  "sd_frequency": "1m",
  "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
  "docker_label": {
  }
}
```

This example enables docker label\-based service discovery\. The CloudWatch agent will query the ECS tasks’ metadata once per minute and write the discovered targets into the `/tmp/cwagent_ecs_auto_sd.yaml` file within the CloudWatch agent container\.

The default value of `sd_port_label` in the `docker_label` section is `ECS_PROMETHEUS_EXPORTER_PORT`\. If any running container in the ECS tasks has a `ECS_PROMETHEUS_EXPORTER_PORT` docker label, the CloudWatch agent uses its value as `container port` to scan all exposed ports of the container\. If there is a match, the mapped host port plus the private IP of the container are used to construct the Prometheus exporter target in the following format: `private_ip:host_port`\. 

The default value of `sd_metrics_path_label` in the `docker_label` section is `ECS_PROMETHEUS_METRICS_PATH`\. If the container has this docker label, its value will be used as the `__metrics_path__` \. If the container does not have this label, the default value `/metrics` is used\.

The default value of `sd_job_name_label` in the `docker_label` section is `job`\. If the container has this docker label, its value will be appended as one of the labels for the target to replace the default job name specified in the Prometheus configuration\. The value of this docker label is used as the log stream name in the CloudWatch Logs log group\. 

**Example 2**

```
"ecs_service_discovery": {
  "sd_frequency": "15s",
  "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
  "docker_label": {
    "sd_port_label": "ECS_PROMETHEUS_EXPORTER_PORT_SUBSET_A",
    "sd_job_name_label": "ECS_PROMETHEUS_JOB_NAME"  
  }
}
```

This example enables docker label\-based service discovery\. THe CloudWatch agent will query the ECS tasks’ metadata every 15 seconds and write the discovered targets into the `/tmp/cwagent_ecs_auto_sd.yaml` file within the CloudWatch agent container\. The containers with a docker label of `ECS_PROMETHEUS_EXPORTER_PORT_SUBSET_A` will be scanned\. The value of the docker label `ECS_PROMETHEUS_JOB_NAME` is used as the job name\.

**Example 3**

```
"ecs_service_discovery": {
  "sd_frequency": "5m",
  "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
  "task_definition_list": [
    {
      "sd_job_name": "java-prometheus",
      "sd_metrics_path": "/metrics",
      "sd_metrics_ports": "9404; 9406",
      "sd_task_definition_arn_pattern": ".*:task-definition/.*javajmx.*:[0-9]+"
    },
    {
      "sd_job_name": "envoy-prometheus",
      "sd_metrics_path": "/stats/prometheus",
      "sd_container_name_pattern": "^envoy$", 
      "sd_metrics_ports": "9901",
      "sd_task_definition_arn_pattern": ".*:task-definition/.*appmesh.*:23"
    }
  ]
}
```

This example enables ECS task definition ARN regular expression\-based service discovery\. The CloudWatch agent will query the ECS tasks’ metadata every five minutes and write the discovered targets into the `/tmp/cwagent_ecs_auto_sd.yaml` file within the CloudWatch agent container\.

Two task definition ARN regular expresion sections are defined:
+  For the first section, the ECS tasks with `javajmx` in their ECS task definition ARN are filtered for the container port scan\. If the containers within these ECS tasks expose the container port on 9404 or 9406, the mapped host port along with the private IP of the container are used to create the Prometheus exporter targets\. The value of `sd_metrics_path` sets `__metrics_path__` to `/metrics`\. So the CloudWatch agent will scrape the Prometheus metrics from `private_ip:host_port/metrics`, the scraped metrics are sent to the `java-prometheus` log stream in CloudWatch Logs in the log group `/aws/ecs/containerinsights/cluster_name/prometheus`\. 
+  For the second section, the ECS tasks with `appmesh` in their ECS task definition ARN and with `version` of `:23` are filtered for the container port scan\. For containers with a name of `envoy` that expose the container port on `9901`, the mapped host port along with the private IP of the container are used to create the Prometheus exporter targets\. The value within these ECS tasks expose the container port on 9404 or 9406, the mapped host port along with the private IP of the container are used to create the Prometheus exporter targets\. The value of `sd_metrics_path` sets `__metrics_path__` to `/stats/prometheus`\. So the CloudWatch agent will scrape the Prometheus metrics from `private_ip:host_port/stats/prometheus`, and send the scraped metrics to the `envoy-prometheus` log stream in CloudWatch Logs in the log group `/aws/ecs/containerinsights/cluster_name/prometheus`\. 

**Example 4**

```
"ecs_service_discovery": {
  "sd_frequency": "5m",
  "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
  "service_name_list_for_tasks": [
    {
      "sd_job_name": "nginx-prometheus",
      "sd_metrics_path": "/metrics",
      "sd_metrics_ports": "9113",
      "sd_service_name_pattern": "^nginx-.*"
    },
    {
      "sd_job_name": "haproxy-prometheus",
      "sd_metrics_path": "/stats/metrics",
      "sd_container_name_pattern": "^haproxy$",
      "sd_metrics_ports": "8404",
      "sd_service_name_pattern": ".*haproxy-service.*"
    }
  ]
}
```

This example enables ECS service name regular expression\-based service discovery\. The CloudWatch agent will query the ECS services’ metadata every five minutes and write the discovered targets into the `/tmp/cwagent_ecs_auto_sd.yaml` file within the CloudWatch agent container\.

Two service name regular expresion sections are defined:
+  For the first section, the ECS tasks that are associated with ECS services that have names matching the regular expression `^nginx-.*` are filtered for the container port scan\. If the containers within these ECS tasks expose the container port on 9113, the mapped host port along with the private IP of the container are used to create the Prometheus exporter targets\. The value of `sd_metrics_path` sets `__metrics_path__` to `/metrics`\. So the CloudWatch agent will scrape the Prometheus metrics from `private_ip:host_port/metrics`, and the scraped metrics are sent to the `nginx-prometheus` log stream in CloudWatch Logs in the log group `/aws/ecs/containerinsights/cluster_name/prometheus`\. 
+  or the second section, the ECS tasks that are associated with ECS services that have names matching the regular expression `.*haproxy-service.*` are filtered for the container port scan\. For containers with a name of `haproxy` expose the container port on 8404, the mapped host port along with the private IP of the container are used to create the Prometheus exporter targets\. The value of `sd_metrics_path` sets `__metrics_path__` to `/stats/metrics`\. So the CloudWatch agent will scrape the Prometheus metrics from `private_ip:host_port/stats/metrics`, and the scraped metrics are sent to the `haproxy-prometheus` log stream in CloudWatch Logs in the log group `/aws/ecs/containerinsights/cluster_name/prometheus`\. 

**Example 5**

```
"ecs_service_discovery": {
  "sd_frequency": "1m30s",
  "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
  "docker_label": {
    "sd_port_label": "MY_PROMETHEUS_EXPORTER_PORT_LABEL",
    "sd_metrics_path_label": "MY_PROMETHEUS_METRICS_PATH_LABEL",
    "sd_job_name_label": "MY_PROMETHEUS_METRICS_NAME_LABEL"  
  }
  "task_definition_list": [
    {
      "sd_metrics_ports": "9150",
      "sd_task_definition_arn_pattern": "*memcached.*"
    }
  ]
}
```

This example enables both ECS service discovery modes\. The CloudWatch agent will query the ECS tasks’ metadata every 90 seconds and write the discovered targets into the `/tmp/cwagent_ecs_auto_sd.yaml` file within the CloudWatch agent container\. 

For the docker\-based service discovery configuration:
+ The ECS tasks with docker label `MY_PROMETHEUS_EXPORTER_PORT_LABEL` will be filtered for Prometheus port scan\. The target Prometheus container port is specified by the value of the label `MY_PROMETHEUS_EXPORTER_PORT_LABEL`\. 
+ The value of the docker label `MY_PROMETHEUS_EXPORTER_PORT_LABEL` is used for `__metrics_path__`\. If the container does not have this docker label, the default value `/metrics` is used\. 
+ The value of the docker label `MY_PROMETHEUS_EXPORTER_PORT_LABEL` is used as the job label\. If the container does not have this docker label, the job name defined in the Prometheus configuration is used\.

For the ECS task definition ARN regular expression\-based service discovery configuration:
+ The ECS tasks with `memcached` in the ECS task definition ARN are filtered for container port scan\. The target Prometheus container port is 9150 as defined by `sd_metrics_ports`\. The default metrics path `/metrics` is used\. The job name defined in the Prometheus configuration is used\.