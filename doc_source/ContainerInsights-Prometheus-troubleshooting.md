# Prometheus Metrics Troubleshooting<a name="ContainerInsights-Prometheus-troubleshooting"></a>

This section provides help for troubleshooting your Prometheus metrics setup\. 

## General Troubleshooting Steps<a name="ContainerInsights-Prometheus-troubleshooting-general"></a>

To confirm that the CloudWatch agent is running, enter the following command\.

```
kubectl get pod -n amazon-cloudwatch
```

The output should include a row with cwagent\-prometheus\-*id* in the NAME column and Running in the STATUS column\.

To display details about the running pod, enter the following command\. Replace *pod\-name* with the completename of your pod that has s name that starts with `cw-agent-prometheus`\.

```
kubectl describe pod pod-name -n amazon-cloudwatch
```

If you have CloudWatch Container Insights installed, you can use CloudWatch Logs Insights to query the logs from the CloudWatch agent collecting the Prometheus metrics\.

**To query the application logs**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **CloudWatch Logs Insights**\.

1. Select the log group for the application logs, **/aws/containerinsights/*cluster\-name*/application**

1. Replace the search query expression with the following query, and choose **Run query**

   ```
   fields ispresent(kubernetes.pod_name) as haskubernetes_pod_name, stream, kubernetes.pod_name, log | 
   filter haskubernetes_pod_name and kubernetes.pod_name like /cwagent-prometheus
   ```

You can also confirm that Prometheus metrics and metadata are being ingested as CloudWatch Logs events\.

**To confirm that Prometheus data is being ingested**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **CloudWatch Logs Insights**\.

1. Select the **/aws/containerinsights/*cluster\-name*/prometheus**

1. Replace the search query expression with the following query, and choose **Run query**

   ```
   fields @timestamp, @message | sort @timestamp desc | limit 20
   ```

## Logging Dropped Prometheus Metrics<a name="ContainerInsights-Prometheus-troubleshooting-droppedmetrics"></a>

This beta release does not collect Prometheus metrics of the histogram or summary types\. You can use the CloudWatch agent to check whether any Prometheus metrics are being dropped because they are one of these types\. You can also log a list of the first 500 Prometheus metrics that are dropped and not sent to CloudWatch because they are histogram or summary metrics\.

To see whether any metrics are being dropped, enter the following command:

```
kubectl logs -l "app=cwagent-prometheus" -n amazon-cloudwatch --tail=-1
```

If any metrics are being dropped, you will see the following lines in the `/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log` file\.

```
I! Drop Prometheus metrics with unsupported types. Only Gauge and Counter are supported.
I! Please enable CWAgent debug mode to view the first 500 dropped metrics
```

If you see those lines and want to know what metrics are being dropped, use the following steps\.

**To log a list of dropped Prometheus metrics**

1. Change the CloudWatch agent to debug mode by adding the following bold lines to your `prometheus-eks.yaml` or `prometheus-k8s.yaml`file, and save the file\.

   ```
   {
         "agent": {
           "debug": true
         },
   ```

   This section of the file should then look like this:

   ```
   cwagentconfig.json: |
       {
         "agent": {
           "debug": true
         },
         "logs": {
           "metrics_collected": {
   ```

1. Reinstall the CloudWatch agent to enable debug mode by entering the following commands:

   ```
   kubectl delete deployment cwagent-prometheus -n amazon-cloudwatch
   kubectl apply -f prometheus.yaml
   ```

   The dropped metrics are logged in the CloudWatch agent pod\.

1. To retrieve the logs from the CloudWatch agent pod, enter the following command:

   ```
   kubectl logs -l "app=cwagent-prometheus" -n amazon-cloudwatch --tail=-1
   ```

   Or, if you have Container Insights FluentD logging installed, the logs are also saved in the CloudWatch Logs log group **/aws/containerinsights/*cluster\_name*/application**\.

   To query these logs, you can follow the steps for querying the application logs in [General Troubleshooting Steps](#ContainerInsights-Prometheus-troubleshooting-general)\.

## Where are the Prometheus Metrics Ingested as CloudWatch Logs Log Events?<a name="ContainerInsights-Prometheus-troubleshooting-metrics_ingested"></a>

The CloudWatch agent creates a log stream for each Prometheus scrape job configuration\. For example, in the `prometheus-eks.yaml` and `prometheus-k8s.yaml` files, the line `job_name: 'kubernetes-pod-appmesh-envoy'` scrapes App Mesh metrics\. The Prometheus target is defined as `kubernetes-pod-appmesh-envoy`\. So all App Mesh Prometheus metrics are ingested as CloudWatch Logs events in the log stream **kubernetes\-pod\-appmesh\-envoy** under the log group named **/aws/containerinsights/cluster\-name/Prometheus**\.

## I Don't See Prometheus Metrics in CloudWatch Metrics<a name="ContainerInsights-Prometheus-troubleshooting-no-metrics"></a>

First, make sure that the Prometheus metrics are ingested as log events in the log group **/aws/containerinsights/cluster\-name/Prometheus**\. Use the information in [Where are the Prometheus Metrics Ingested as CloudWatch Logs Log Events?](#ContainerInsights-Prometheus-troubleshooting-metrics_ingested) to help you check the target log stream\. If the log stream is not created or there are no new log events in the log stream, check the following:
+ Check that the Prometheus metrics exporter endpoints are set up correctly
+ Check that the Prometheus scraping configurations in the `config map: cwagent-prometheus` section of the CloudWatch agent YAML file is correct\. The configuration should be the same as it would be in a Prometheus configuration file\. For more information, see [<scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) in the Prometheus documentation\.

If the Prometheus metrics are ingested as log events correctly, check that the embedded metric format settings are added into the log events to generate the CloudWatch metrics\.

```
"CloudWatchMetrics":[
   {
      "Metrics":[
         {
            "Name":"envoy_http_downstream_cx_destroy_remote_active_rq"
         }
      ],
      "Dimensions":[
         [
            "ClusterName",
            "Namespace"
         ]
      ],
      "Namespace":"ContainerInsights/Prometheus"
   }
],
```

For more information about embedded metric format, see [Specification: Embedded Metric Format ](CloudWatch_Embedded_Metric_Format_Specification.md)\.

If there is no embedded metric format in the log events, check that the `metric_definitions` are configured correctly in the `config map: prometheus-cwagentconfig` section of the CloudWatch agent installation YAML file\. For more information, see [Tutorial for Adding a New Prometheus Scrape Target: Prometheus KPI Server Metrics](ContainerInsights-Prometheus-Setup-configure.md#ContainerInsights-Prometheus-Setup-new-exporters)\.