# Viewing Your Prometheus Metrics<a name="ContainerInsights-Prometheus-viewmetrics"></a>

You can monitor and alarm on all your Prometheus metrics including the curated pre\-aggregated metrics from App Mesh, NGINX, Java/JMX, Memcached, and HAProxy, and any other manually configured Prometheus exporter you may have added\. For more information about collecting metrics from other Prometheus exporters, see [Tutorial for Adding a New Prometheus Scrape Target: Prometheus KPI Server Metrics](ContainerInsights-Prometheus-Setup-configure.md#ContainerInsights-Prometheus-Setup-new-exporters)\.

In the CloudWatch console, Container Insights provides pre\-built reports for App Mesh and NGINX\. 

Container Insights also provides custom dashboards for each of the workloads that Container Insights collects curated metrics from\. You can download these dashboards from GitHub 

**To see all your Prometheus metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In the list of namespaces, choose **ContainerInsights/Prometheus**\.

1. Choose one of the sets of dimensions in the following list\. Then select the checkbox next to the metrics that you want to see\.

**To see pre\-built reports on your Prometheus metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Performance Monitoring**\.

1. In the drop\-down box near the top of the page, choose **Prometheus AppMesh** or **Prometheus NGINX**\.

   In the other drop\-down box, choose a cluster to view

We have also provided custom dashboards for NGINX, App Mesh, Memcached, HAProxy, and Java/JMX\.

**To use a custom dashboard that Amazon has provided**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose **Create Dashboard**\. Enter a name for the new dashboard, and choose **Create dashboard**\.

1. In **Add to this dashboard**, choose **Cancel**\.

1. Choose **Actions**, **View/edit source**\.

1. Download one of the following JSON files:
   + [ NGINX custom dashboard source on Github](https://raw.githubusercontent.com/gubupt/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/nginx-ingress/cw_dashboard_nginx_ingress_controller.json)\.
   + [ App Mesh custom dashboard source on Github](https://raw.githubusercontent.com/gubupt/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/appmesh/cw_dashboard_awsappmesh.json)\.
   + [ Memcached custom dashboard source on Github](https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/memcached/cw_dashboard_memcached.json)
   + [ HAProxy\-Ingress custom dashboard source on Github](https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/haproxy-ingress/cw_dashboard_haproxy_ingress.json)
   + [ Java/JMX custom dashboard source on Github](https://raw.githubusercontent.com/gubupt/amazon-cloudwatch-container-insights/prometheus-beta/k8s-deployment-manifest-templates/deployment-mode/service/cwagent-prometheus/sample_cloudwatch_dashboards/javajmx/cw_dashboard_javajmx.json)\.

1. Open the JSON file that you downloaded with a text editor, and make the following changes:
   + Replace all the `{{YOUR_CLUSTER-NAME}}` strings with the exact name of your cluster\. Make sure not to add whitespaces before or after the text\.
   + Replace all the `{{YOUR_AWS_REGION}}` strings with the AWS Region where your cluster is running\. For example, **us\-west\-1** Make sure not to add whitespaces before or after the text\. 
   + Replace all the `{{YOUR_NAMESPACE}}` strings with the exact namespace of your workload\.
   + Replace all the `{{YOUR_SERVICE_NAME}}` strings with the exact service name of your workload\. For example, **haproxy\-haproxy\-ingress\-controller\-metrics**

1. Copy the entire JSON blob and paste it into the text box in the CloudWatch console, replacing what is already in the box\.

1. Choose **Update**, **Save dashboard**\.