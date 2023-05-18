# Retrieve custom metrics with StatsD<a name="CloudWatch-Agent-custom-metrics-statsd"></a>

You can retrieve additional custom metrics from your applications or services using the CloudWatch agent with the `StatsD` protocol\. StatsD is a popular open\-source solution that can gather metrics from a wide variety of applications\. StatsD is especially useful for instrumenting your own metrics\. For an example of using the CloudWatch agent and StatsD together, see [ How to better monitor your custom application metrics using Amazon CloudWatch Agent](https://aws.amazon.com/blogs/devops/new-how-to-better-monitor-your-custom-application-metrics-using-amazon-cloudwatch-agent/)\.

`StatsD` is supported on both Linux servers and servers running Windows Server\. CloudWatch supports the following `StatsD` format:

```
MetricName:value|type|@sample_rate|#tag1:
  value,tag1...
```
+ `MetricName` – A string with no colons, bars, \# characters, or @ characters\.
+ `value` – This can be either integer or float\.
+ `type` – Specify `c` for counter, `g` for gauge, `ms` for timer, `h` for histogram, or `s` for set\.
+ `sample_rate` – \(Optional\) A float between 0 and 1, inclusive\. Use only for counter, histogram, and timer metrics\. The default value is 1 \(sampling 100% of the time\)\.
+ `tags` – \(Optional\) A comma\-separated list of tags\. `StatsD` tags are similar to dimensions in CloudWatch\. Use colons for key/value tags, such as `env:prod`\.

You can use any `StatsD` client that follows this format to send the metrics to the CloudWatch agent\. For more information about some of the available `StatsD` clients, see the [StatsD client page on GitHub](https://github.com/etsy/statsd/wiki#client-implementations)\. 

To collect these custom metrics, add a `"statsd": {}` line to the `metrics_collected` section of the agent configuration file\. You can add this line manually\. If you use the wizard to create the configuration file, it's done for you\. For more information, see [Create the CloudWatch agent configuration file](create-cloudwatch-agent-configuration-file.md)\.

The `StatsD` default configuration works for most users\. There are four optional fields that you can add to the **statsd** section of the agent configuration file as needed:
+ `service_address` – The service address to which the CloudWatch agent should listen\. The format is `ip:port`\. If you omit the IP address, the agent listens on all available interfaces\. Only the UDP format is supported, so you don't need to specify a UDP prefix\. 

  The default value is `:8125`\.
+ `metrics_collection_interval` – How often in seconds that the `StatsD` plugin runs and collects metrics\. The default value is 10 seconds\. The range is 1–172,000\.
+ `metrics_aggregation_interval` – How often in seconds CloudWatch aggregates metrics into single data points\. The default value is 60 seconds\.

  For example, if `metrics_collection_interval` is 10 and `metrics_aggregation_interval` is 60, CloudWatch collects data every 10 seconds\. After each minute, the six data readings from that minute are aggregated into a single data point, which is sent to CloudWatch\.

  The range is 0–172,000\. Setting `metrics_aggregation_interval` to 0 disables the aggregation of `StatsD` metrics\.
+ `allowed_pending_messages` – The number of UDP messages that are allowed to queue up\. When the queue is full, the StatsD server starts dropping packets\. The default value is 10000\.

The following is an example of the **statsd** section of the agent configuration file, using the default port and custom collection and aggregation intervals\.

```
{
   "metrics":{
      "metrics_collected":{
         "statsd":{
            "service_address":":8125",
            "metrics_collection_interval":60,
            "metrics_aggregation_interval":300
         }
      }
   }
}
```

## Viewing StatsD metrics imported by the CloudWatch agent<a name="CloudWatch-view-statsd-metrics"></a>

After importing StatsD metrics into CloudWatch, you can view these metrics as time series graphs, and create alarms that can watch these metrics and notify you if they breach a threshold that you specify\. The following procedure shows how to view StatsD metrics as a time series graph\. For more information about setting alarms, see [ Using Amazon CloudWatch alarms](AlarmThatSendsEmail.md)\.

**To view StatsD metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose the namespace for the metrics collected by the agent\. By default, this is **CWAgent**, but you may have specified a different namespace in the CloudWatch agent configuration file\.

1. Choose a metric dimension \(for example, **Per\-Instance Metrics**\)\.

1. The **All metrics** tab displays all metrics for that dimension in the namespace\. You can do the following:

   1. To graph a metric, select the check box next to the metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To sort the table, use the column heading\.

   1. To filter by resource, choose the resource ID and then choose **Add to search**\.

   1. To filter by metric, choose the metric name and then choose **Add to search**\.

1. \(Optional\) To add this graph to a CloudWatch dashboard, choose **Actions**, **Add to dashboard**\.