# Scraping Additional Prometheus sources and Importing Those Metrics<a name="ContainerInsights-Prometheus-Setup-configure-ECS"></a>

The CloudWatch agent with Prometheus monitoring needs two configurations to scrape the Prometheus metrics\. One is for the standard Prometheus configurations as documented in [<scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) in the Prometheus documentation\. The other is for the CloudWatch agent configuration\.

For Amazon ECS clusters, the configurations are integrated with the Parameter Store of AWS Systems Manager by the secrets in the Amazon ECS task definition:
+ The secret `PROMETHEUS_CONFIG_CONTENT` is for the Prometheus scrape configuration\.
+ The secret `CW_CONFIG_CONTENT` is for the CloudWatch agent configuration\. 

To scrape additional Prometheus metrics sources and import those metrics to CloudWatch, you modify both the Prometheus scrape configuration and the CloudWatch agent configuration, and then re\-deploy the agent with the updated configuration\.

**VPC Security Group Requirements**

The ingress rules of the security groups for the Prometheus workloads must open the Prometheus ports to the CloudWatch agent for scraping the Prometheus metrics by the private IP\.

The egress rules of the security group for the CloudWatch agent must allow the CloudWatch agent to connect to the Prometheus workloads' port by private IP\. 

## Prometheus Scrape Configuration<a name="ContainerInsights-Prometheus-Setup-config-global"></a>

The CloudWatch agent supports the standard Prometheus scrape configurations as documented in [<scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) in the Prometheus documentation\. You can edit this section to update the configurations that are already in this file, and add additional Prometheus scraping targets\. By default, the sample configuration file contains the following global configuration lines:

```
global:
  scrape_interval: 1m
  scrape_timeout: 10s
```
+ **scrape\_interval**— Defines how frequently to scrape targets\.
+ **scrape\_timeout**— Defines how long to wait before a scrape request times out\.

You can also define different values for these settings at the job level, to override the global configurations\.

### Prometheus Scraping Jobs<a name="ContainerInsights-Prometheus-Setup-config-scrape"></a>

The CloudWatch agent YAML files already have some default scraping jobs configured\. For example, in the YAML files for Amazon ECS such as `cwagent-ecs-prometheus-metric-for-bridge-host.yaml`, the default scraping jobs are configured in the `ecs_service_discovery` section\.

```
"ecs_service_discovery": {
                  "sd_frequency": "1m",
                  "sd_result_file": "/tmp/cwagent_ecs_auto_sd.yaml",
                  "docker_label": {
                  },
                  "task_definition_list": [
                    {
                      "sd_job_name": "ecs-appmesh-colors",
                      "sd_metrics_ports": "9901",
                      "sd_task_definition_arn_pattern": ".*:task-definition\/.*-ColorTeller-(white):[0-9]+",
                      "sd_metrics_path": "/stats/prometheus"
                    },
                    {
                      "sd_job_name": "ecs-appmesh-gateway",
                      "sd_metrics_ports": "9901",
                      "sd_task_definition_arn_pattern": ".*:task-definition/.*-ColorGateway:[0-9]+",
                      "sd_metrics_path": "/stats/prometheus"
                    }
                  ]
                }
```

Each of these default targets are scraped, and the metrics are sent to CloudWatch in log events using embedded metric format\. For more information, see [Ingesting High\-Cardinality Logs and Generating Metrics with CloudWatch Embedded Metric Format](CloudWatch_Embedded_Metric_Format.md)\.

Log events from Amazon ECS clusters are stored in the **/aws/ecs/containerinsights/*cluster\_name*/prometheus** log group\.

Each scraping job is contained in a different log stream in this log group\.

To add a new scraping target, you add a new entry in the `task_definition_list` section under the `ecs_service_discovery` section\. of the YAML file, and restart the agent\. For an example of this process, see [Tutorial for Adding a New Prometheus Scrape Target: Prometheus API Server Metrics](ContainerInsights-Prometheus-Setup-configure.md#ContainerInsights-Prometheus-Setup-new-exporters)\.

## CloudWatch Agent Configuration for Prometheus<a name="ContainerInsights-Prometheus-Setup-cw-agent-config"></a>

The CloudWatch agent configuration file has a `prometheus` section under `metrics_collected` for the Prometheus scraping configuration\. It includes the following configuration options:
+ **cluster\_name**— specifies the cluster name to be added as a label in the log event\. This field is optional\. If you omit it, the agent can detect the Amazon ECS cluster name\.
+ **log\_group\_name**— specifies the log group name for the scraped Prometheus metrics\. This field is optional\. If you omit it, CloudWatch uses **/aws/ecs/containerinsights/*cluster\_name*/prometheus** for logs from Amazon ECS clusters\.
+ **prometheus\_config\_path**— specifies the Prometheus scrape configuration file path\. If the value of this field starts with `env:` the Prometheus scrape configuration file contents will be retrieved from the container's environment variable\. Do not change this field\.
+ **ecs\_service\_discovery**— is the section to specify the configurations of the Amazon ECS Prometheus target auto\-discovery functions\. Two modes are supported to discover the Prometheus targets: discovery based on the container’s docker label or discovery based on the Amazon ECS task definition ARN regular expression\. You can use the two modes together and the CloudWatch agent will de\-duplicate the discovered targets based on: *\{private\_ip\}:\{port\}/\{metrics\_path\}*\.

  The `ecs_service_discovery` section can contain the following fields:
  + `sd_frequency` is the frequency to discover the Prometheus exporters\. Specify a number and a unit suffix\. For example, `1m` for once per minute or `30s` for once per 30 seconds\. Valid unit suffixes are `ns`, `us`, `ms`, `s`, `m`, and `h`\.

    This field is optional\. The default is 60 seconds \(1 minute\)\.
  + `sd_target_cluster` is the target Amazon ECS cluster name for auto\-discovery\. This field is optional\. The default is the name of the Amazon ECS cluster where the CloudWatch agent is installed\. 
  + `sd_cluster_region` is the target Amazon ECS cluster's Region\. This field is optional\. The default is the Region of the Amazon ECS cluster where the CloudWatch agent is installed\. \.
  + `sd_result_file` is the path of the YAML file for the Prometheus target results\. The Prometheus scrape configuration will refer to this file\.
  + `docker_label` is an optional section that you can use to specify the configuration for docker label\-based service discovery\. If you omit this section, docker label\-based discovery is not used\. This section can contain the following fields:
    + `sd_port_label` is the container's docker label name that specifies the container port for Prometheus metrics\. The default value is `ECS_PROMETHEUS_EXPORTER_PORT`\. If the container does not have this docker label, the CloudWatch agent will skip it\.
    + `sd_metrics_path_label` is the container's docker label name that specifies the Prometheus metrics path\. The default value is `ECS_PROMETHEUS_METRICS_PATH`\. If the container does not have this docker label, the the agent assumes the default path `/metrics`\.
    + `sd_job_name_label` is the container's docker label name that specifies the Prometheus scrape job name\. The default value is `job`\. If the container does not have this docker label, the CloudWatch agent uses the job name in the Prometheus scrape configuration\.
  + `task_definition_list` is an optional section that you can use to specify the configuration of task definition\-based service discovery\. If you omit this section, task definition\-based discovery is not used\. This section can contain the following fields:
    + `sd_task_definition_arn_pattern` is the pattern to use to specify the Amazon ECS task definitions to discover\. This is a regular expression\.
    + `sd_metrics_ports` lists the containerPort for the Prometheus metrics\. Separate the containerPorts with semicolons\.
    + `sd_container_name_pattern` specifies the Amazon ECS task container names\. This is a regular expression\.
    + `sd_metrics_path` specifies the Prometheus metric path\. If you omit this, the agent assumes the default path `/metrics`
    + `sd_job_name` specifies the Prometheus scrape job name\. If you omit this field, the CloudWatch agent uses the job name in the Prometheus scrape configuration\.
  + `service_name_list_for_tasks` is an optional section that you can use to specify the configuration of service name\-based discovery\. If you omit this section, service name\-based discovery is not used\. This section can contain the following fields:
    + `sd_service_name_pattern` is the pattern to use to specify the Amazon ECS service where tasks are to be discovered\. This is a regular expression\.
    + `sd_metrics_ports` Lists the `containerPort` for the Prometheus metrics\. Separate multiple `containerPorts` with semicolons\.
    + `sd_container_name_pattern` specifies the Amazon ECS task container names\. This is a regular expression\.
    + `sd_metrics_path` specifies the Prometheus metrics path\. If you omit this, the agent assumes that the default path `/metrics`\.
    + `sd_job_name` specifies the Prometheus scrape job name\. If you omit this field, the CloudWatch agent uses the job name in the Prometheus scrape configuration\. 
+ **metric\_declaration**— are sections that specify the array of logs with embedded metric format to be generated\. There are `metric_declaration` sections for each Prometheus source that the CloudWatch agent imports from by default\. These sections each include the following fields:
  + `label_matcher` is a regular expression that checks the value of the labels listed in `source_labels`\. The metrics that match are enabled for inclusion in the embedded metric format sent to CloudWatch\. 
  + `source_labels` specifies the value of the labels that are checked by the `label_matcher` line\.
  + `label_separator` specifies the separator to be used in the ` label_matcher` line if multiple `source_labels` are specified\. The default is `;`\. You can see this default used in the `label_matcher` line in the following example\.
  + `metric_selectors` is a regular expression that specifies the metrics to be collected and sent to CloudWatch\.
  + `dimensions` is the list of labels to be used as CloudWatch dimensions for each selected metric\.

See the following `metric_declaration` example\.

```
"metric_declaration": [
  {
     "source_labels":[ "Service", "Namespace"],
     "label_matcher":"(.*node-exporter.*|.*kube-dns.*);kube-system$",
     "dimensions":[
        ["Service", "Namespace"]
     ],
     "metric_selectors":[
        "^coredns_dns_request_type_count_total$"
     ]
  }
]
```

This example configures an embedded metric format section to be sent as a log event if the following conditions are met:
+ The value of `Service` contains either `node-exporter` or `kube-dns`\.
+ The value of `Namespace` is `kube-system`\.
+ The Prometheus metric `coredns_dns_request_type_count_total` contains both `Service` and `Namespace` labels\.

The log event that is sent includes the following highlighted section:

```
{
   "CloudWatchMetrics":[
      {
         "Metrics":[
            {
               "Name":"coredns_dns_request_type_count_total"
            }
         ],
         "Dimensions":[
            [
               "Namespace",
               "Service"
            ]
         ],
         "Namespace":"ContainerInsights/Prometheus"
      }
   ],
   "Namespace":"kube-system",
   "Service":"kube-dns",
   "coredns_dns_request_type_count_total":2562,
   "eks_amazonaws_com_component":"kube-dns",
   "instance":"192.168.61.254:9153",
   "job":"kubernetes-service-endpoints",
   ...
}
```