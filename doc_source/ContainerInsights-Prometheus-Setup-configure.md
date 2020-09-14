# Configuring the CloudWatch Agent for Prometheus Monitoring<a name="ContainerInsights-Prometheus-Setup-configure"></a>

The CloudWatch agent uses a YAML file to configure how it collects Prometheus metrics\. This file is called `prometheus-eks.yaml` for Amazon EKS clusters and `prometheus-k8s.yaml` for Kubernetes clusters on Amazon EC2 instances\.

This YAML file contains two config map sections:
+ The `name: prometheus-config` section contains the settings for Prometheus scraping\. It follows the standard Prometheus scrape config as documented in [<scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) in the Prometheus documentation\. You use this section to add scrape targets\. 
+ The `name: prometheus-cwagentconfig` section contains the configuration for the CloudWatch agent\. You can use this section to configure how the Prometheus metrics are collected by CloudWatch\. For example, you whitelist which metrics are to be imported into CloudWatch, and define their dimensions\. 

To scrape additional Prometheus metrics sources and import those metrics to CloudWatch, you can modify both sections of the YAML file and re\-deploy the agent with the updated configuration\.

## Prometheus Scrape Configuration<a name="ContainerInsights-Prometheus-Setup-config-global"></a>

The `prometheus-config` config map section in the CloudWatch agent YAML file supports standard Prometheus scrape configurations as documented in [<scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) in the Prometheus documentation\. You can edit this section to update the configurations that are already in this file, and add additional Prometheeus scraping targets\.

By default, the `prometheus-config` section of the `prometheus-eks.yaml` and `prometheus-k8s.yaml` files contains the following global configuration lines:

```
global:
  scrape_interval: 1m
  scrape_timeout: 10s
```
+ **scrape\_interval**— Defines how frequently to scrape targets\.
+ **scrape\_timeout**— Defines how long to wait before a scrape request times out\.

You can also define different values for these settings at the job level, to override the global configurations\.

### Prometheus Scraping Jobs<a name="ContainerInsights-Prometheus-Setup-config-scrape"></a>

The CloudWatch agent YAML files already have some default scraping jobs configured\. These are defined in the YAML files with `job_name` lines in the `scrape_configs` section\. For example, the following default `kubernetes-pod-jmx` section scrapes JMX exporter metrics\.

```
   - job_name: 'kubernetes-pod-jmx'
      sample_limit: 10000
      metrics_path: /metrics
      kubernetes_sd_configs:
      - role: pod
      relabel_configs:
      - source_labels: [__address__]
        action: keep
        regex: '.*:9404$'
      - action: labelmap
        regex: __meta_kubernetes_pod_label_(.+)
      - action: replace
        source_labels:
        - __meta_kubernetes_namespace
        target_label: Namespace
      - source_labels: [__meta_kubernetes_pod_name]
        action: replace
        target_label: pod_name
      - action: replace
        source_labels:
        - __meta_kubernetes_pod_container_name
        target_label: container_name
      - action: replace
        source_labels:
        - __meta_kubernetes_pod_controller_name
        target_label: pod_controller_name
      - action: replace
        source_labels:
        - __meta_kubernetes_pod_controller_kind
        target_label: pod_controller_kind
      - action: replace
        source_labels:
        - __meta_kubernetes_pod_phase
        target_label: pod_phase
```

Each of these default targets are scraped, and the metrics are sent to CloudWatch in log events using embedded metric format\. For more information, see [Ingesting High\-Cardinality Logs and Generating Metrics with CloudWatch Embedded Metric Format](CloudWatch_Embedded_Metric_Format.md)\.

These log events are stored in the **/aws/containerinsights/*cluster\_name*/prometheus** log group in CloudWatch Logs

Each scraping job is contained in a different log stream in this log group\. For example, the Prometheus scraping job `kubernetes-pod-appmesh-envoy` is defined for App Mesh\. All App Mesh Prometheus metrics are sent to the log stream named **/aws/containerinsights/*cluster\_name*>prometheus/kubernetes\-pod\-appmesh\-envoy/**\.

To add a new scraping target, you add a new `job_name` section to the `scrape_configs` section of the YAML file, and restart the agent\. For an example of this process, see [Tutorial for Adding a New Prometheus Scrape Target: Prometheus API Server Metrics](#ContainerInsights-Prometheus-Setup-new-exporters)\.

## CloudWatch Agent Configuration for Prometheus<a name="ContainerInsights-Prometheus-Setup-cw-agent-config"></a>

The CloudWatch agent YAML file includes a `cwagentconfig.json` section\. This section is under `metrics_collected`, which tells the agent how to collect metrics embedded in logs\. It includes the following fields:
+ **prometheus\_config\_path**— specifies the Prometheus scrape configuration file path\. Do not change this field\.
+ **metric\_declaration**— are sections that specify the array of logs with embedded metric format to be generated\. There are `metric_declaration` sections for each Prometheus source that the CloudWatch agent imports from by default\. These sections each include the following fields:
  + `label_matcher` is a regular espression that checks the value of the Kubernetes labels listed in `source_labels`\. The metrics that match are whitelisted for inclusion in the embedded metric format sent to CloudWatch\. 
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

## Tutorial for Adding a New Prometheus Scrape Target: Prometheus API Server Metrics<a name="ContainerInsights-Prometheus-Setup-new-exporters"></a>

The Kubernetes API Server exposes Prometheus metrics on endpoints by default\. The official example for the Kubernetes API Server scraping configuration is available on [Github](https://github.com/prometheus/prometheus/blob/master/documentation/examples/prometheus-kubernetes.yml)\.

The following tutorial shows how to do the following steps to begin importing Kubernetes API Server metrics into CloudWatch:
+ Adding the Prometheus scraping configuration for Kubernetes API Server to the CloudWatch agent YAML file\.
+ Configuring the embedded metric format metrics definitions in the CloudWatch agent YAML file\.
+ \(Optional\) Creating a CloudWatch dashboard for the Kubernetes API Server metrics\.

**Note**  
The Kubernetes API Server exposes gauge, counter, histogram, and summary metrics\. In this release of Prometheus metrics support, CloudWatch imports only the metrics with gauge, counter and summary types\.

**To start collecting Kubernetes API Server Prometheus metrics in CloudWatch**

1. Download the latest version of the `prometheus-eks.yaml` or `prometheus-k8s.yaml` file by entering one of the following commands\. For an Amazon EKS cluster, enter the following command:

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-eks.yaml
   ```

   For a Kubernetes cluster running on an Amazon EC2 instance, enter the following command:

   ```
   curl -O https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/prometheus-k8s.yaml
   ```

1. Open the file with a text editor, find the `prometheus-config` section, and add the following section inside of that section\. Then save the changes:

   ```
       # Scrape config for API servers
       - job_name: 'kubernetes-apiservers'
         kubernetes_sd_configs:
           - role: endpoints
             namespaces:
               names:
                 - default
         scheme: https
         tls_config:
           ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
           insecure_skip_verify: true
         bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
         relabel_configs:
         - source_labels: [__meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
           action: keep
           regex: kubernetes;https
         - action: replace
           source_labels:
           - __meta_kubernetes_namespace
           target_label: Namespace
         - action: replace
           source_labels:
           - __meta_kubernetes_service_name
           target_label: Service
   ```

1. While you still have the YAML file open in the text editor, find the `cwagentconfig.json` section\. Add the following subsection and save the changes\. This section puts the KPI service metrics onto the CloudWatch agent allow list\. Three types of API Server metrics are added to the allow list:
   + etcd object counts
   + API Server registration controller metrics
   + API Server request metrics

   ```
   {"source_labels": ["job", "resource"],
     "label_matcher": "^kubernetes-apiservers;(services|daemonsets.apps|deployments.apps|configmaps|endpoints|secrets|serviceaccounts|replicasets.apps)",
     "dimensions": [["ClusterName","Service","resource"]],
     "metric_selectors": [
     "^etcd_object_counts$"
     ]
   },
   {"source_labels": ["job", "name"],
      "label_matcher": "^kubernetes-apiservers;APIServiceRegistrationController$",
      "dimensions": [["ClusterName","Service","name"]],
      "metric_selectors": [
      "^workqueue_depth$",
      "^workqueue_adds_total$",
      "^workqueue_retries_total$"
     ]
   },
   {"source_labels": ["job","code"],
     "label_matcher": "^kubernetes-apiservers;2[0-9]{2}$",
     "dimensions": [["ClusterName","Service","code"]],
     "metric_selectors": [
      "^apiserver_request_total$"
     ]
   },
   {"source_labels": ["job"],
     "label_matcher": "^kubernetes-apiservers",
     "dimensions": [["ClusterName","Service"]],
     "metric_selectors": [
     "^apiserver_request_total$"
     ]
   },
   ```

1. If you already have the CloudWatch agent with Prometheus support deployed in the cluster, you must delete it by entering the following command:

   ```
   kubectl delete deployment cwagent-prometheus -n amazon-cloudwatch
   ```

1. Deploy the CloudWatch agent with your updated configuration by entering one of the following commands\. For an Amazon EKS cluster, enter:

   ```
   kubectl apply -f prometheus-eks.yaml
   ```

   For a Kubernetes cluster, enter this command:

   ```
   cat prometheus-k8s.yaml \
   | sed "s/{{cluster_name}}/MyCluster/;s/{{region_name}}/region/" \
   | kubectl apply -f -
   ```
   Replace *MyCluster* and *region* to match your setting.

Once you have done this, you should see a new log stream named ** kubernetes\-apiservers ** in the **/aws/containerinsights/*cluster\_name*/prometheus** log group\. This log stream should include log events with an embedded metric format definition like the following:

```
{
   "CloudWatchMetrics":[
      {
         "Metrics":[
            {
               "Name":"apiserver_request_total"
            }
         ],
         "Dimensions":[
            [
               "ClusterName",
               "Service"
            ]
         ],
         "Namespace":"ContainerInsights/Prometheus"
      }
   ],
   "ClusterName":"my-cluster-name",
   "Namespace":"default",
   "Service":"kubernetes",
   "Timestamp":"1592267020339",
   "Version":"0",
   "apiserver_request_count":0,
   "apiserver_request_total":0,
   "code":"0",
   "component":"apiserver",
   "contentType":"application/json",
   "instance":"192.0.2.0:443",
   "job":"kubernetes-apiservers",
   "prom_metric_type":"counter",
   "resource":"pods",
   "scope":"namespace",
   "verb":"WATCH",
   "version":"v1"
}
```

You can view your metrics in the CloudWatch console in the **ContainerInsights/Prometheus** namespace\. You can also optionally create a CloudWatch dashboard for your Prometheus Kubernetes API Server metrics\.

### \(Optional\) Creating a Dashboard for Kubernetes API Server metrics<a name="ContainerInsights-Prometheus-Setup-KPI-dashboard"></a>

To see Kubernetes API Server metrics in your dashboard, you must have first completed the steps in the previous sections to start collecting these metrics in CloudWatch\.

**To create a dashboard for Kubernetes API Server metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Make sure you have the correct AWS Region selected\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose **Create Dashboard**\. Enter a name for the new dashboard, and choose **Create dashboard**\.

1. In **Add to this dashboard**, choose **Cancel**\.

1. Choose **Actions**, **View/edit source**\.

1. Download the following JSON file: [Kubernetes API Dashboard source](https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/kubernetes_api_server/cw_dashboard_kubernetes_api_server.json)\.

1. Open the JSON file that you downloaded with a text editor, and make the following changes:
   + Replace all the `{{YOUR_CLUSTER-NAME}}` strings with the exact name of your cluster\. Make sure not to add whitespaces before or after the text\.
   + Replace all the `{{YOUR_REGION}}` strings with the name of the Region where the metrics are collected\. For example `us-west-2`\. Be sure not to add whitespaces before or after the text\.

1. Copy the entire JSON blob and paste it into the text box in the CloudWatch console, replacing what is already in the box\.

1. Choose **Update**, **Save dashboard**\.