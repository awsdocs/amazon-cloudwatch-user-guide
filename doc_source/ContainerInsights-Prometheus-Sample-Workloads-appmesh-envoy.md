# \(Optional\) Enable App Mesh Envoy access logs<a name="ContainerInsights-Prometheus-Sample-Workloads-appmesh-envoy"></a>

You can set up Container Insights Fluentd to send App Mesh Envoy access logs to CloudWatch Logs\. For more information, see [Logging](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy-logs.html)\.

**To have Envoy access logs sent to CloudWatch Logs**

1. Set up Fluentd in the cluster\. For more information, see [\(Optional\) Set up Fluentd as a DaemonSet to send logs to CloudWatch Logs](Container-Insights-setup-logs.md)\.

1. Configure Envoy access logs for your virtual nodes\. For instructions, see [Logging](https://docs.aws.amazon.com/app-mesh/latest/userguide/envoy-logs.html)\. Be sure to configure the log path to be **/dev/stdout** in each virtual node\.

When you have finished, the envoy access logs are sent to the `/aws/containerinsights/Cluster_Name/application` log group\.