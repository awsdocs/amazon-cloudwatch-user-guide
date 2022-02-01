# Viewing your metrics and logs in the console<a name="CloudWatch_Embedded_Metric_Format_View"></a>

After you generate embedded metric format logs that extract metrics, you can use the CloudWatch console to view the metrics\. Embedded metrics have the dimensions that you specified when you generated the logs\. Also, embedded metrics that you generated using the client libraries have the following default dimensions:
+ ServiceType 
+ ServiceName
+ LogGroup

**To view metrics that were generated from embedded metric format logs**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select a namespace that you specified for your embedded metrics when you generated them\. If you used the client libraries to generate the metrics and did not specify a namespace, then select **aws\-embedded\-metrics**\. This is the default namespace for embedded metrics generated using the client libraries\.

1. Select a metric dimension \(for example, **ServiceName**\)\.

1. The **All metrics** tab displays all metrics for that dimension in the namespace\. You can do the following:

   1. To sort the table, use the column heading\.

   1. To graph a metric, select the check box next to the metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To filter by resource, choose the resource ID and then choose **Add to search**\.

   1. To filter by metric, choose the metric name and then choose **Add to search**\.

**Querying logs using CloudWatch Logs Insights**

You can query the detailed log events associated with the extracted metrics by using CloudWatch Logs Insights to provide deep insights into the root causes of operational events\. One of the benefits of extracting metrics from your logs is that you can filter your logs later by the unique metric \(metric name plus unique dimension set\) and metric values, to get context on the events that contributed to the aggregated metric value

For example, to get an impacted request id or x\-ray trace id, you could run the following query in CloudWatch Logs Insights\.

```
filter Latency > 1000 and Operation = "Aggregator"
| fields RequestId, TraceId
```

You can also perform query\-time aggregation on high\-cardinality keys, such as finding the customers impacted by an event\. The following example illustrates this\.

```
filter Latency > 1000 and Operation = "Aggregator"
| stats count() by CustomerId
```

For more information, see [Analyzing Log Data with CloudWatch Logs Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html)