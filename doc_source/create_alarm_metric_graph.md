# Creating an Alarm from a Metric on a Graph<a name="create_alarm_metric_graph"></a>

You can graph a metric and then create an alarm from the metric on the graph, which has the benefit of populating many of the alarm fields for you\.

**To create an alarm from a metric on a graph**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select a metric namespace \(for example, **EC2**\) and then a metric dimension \(for example, **Per\-Instance Metrics**\)\.

1. The **All metrics** tab displays all metrics for that dimension in that namespace\. To graph a metric, select the check box next to the metric\.

1. To create an alarm for the metric, choose the **Graphed metrics** tab\. For **Actions**, choose the alarm icon\.  
![\[Create an alarm from a graphed metric\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_graph_alarm.png)

1. Under **Alarm Threshold**, enter a unique name for the alarm and a description of the alarm\. For **Whenever**, specify a threshold and the number of periods\.

1. Under **Actions**, select the type of action to have the alarm perform when the alarm is triggered\.

1. \(Optional\) For **Period**, choose a different value\. For **Statistic**, choose **Standard** to specify one of the statistics in the list or choose **Custom** to specify a percentile \(for example, **p95\.45**\)\.  
![\[Update the period and statistic for an alarmed metric\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metric_alarm_period_statistic.png)

1. Choose **Create Alarm**\.