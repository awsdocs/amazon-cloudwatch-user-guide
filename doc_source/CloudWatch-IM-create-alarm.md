# Creating alarms with Amazon CloudWatch Internet Monitor<a name="CloudWatch-IM-create-alarm"></a>

You can create Amazon CloudWatch alarms based on Amazon CloudWatch Internet Monitor metrics, just as you can for other Amazon CloudWatch metrics\.

For example, you can create an alarm based on the Internet Monitor metric `PerformanceScore`, and configure a notification to be sent when the metric is lower than a value that you choose\. You configure alarms for Internet Monitor metrics following the same guidelines as for other CloudWatch metrics\. 

Example Internet Monitor metrics that you might choose to create an alarm for are the following:
+ **PerformanceScore**
+ **AvailabilityScore**
+ **RoundtripTime**

To see all the metrics available for Internet Monitor, see [Using CloudWatch Metrics with Amazon CloudWatch Internet Monitor](CloudWatch-IM-view-cw-tools-metrics-dashboard.md)\.

The following procedure provides an example of setting an alarm on **PerformanceScore** by navigating to the metric in the CloudWatch dashboard\. Then, you follow the standard CloudWatch steps to create an alarm based on a threshold that you choose, and set up a notification or choose other options\.

**To create an alarm for **PerformanceScore** in CloudWatch Metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Metrics**, and then choose **All metrics**\.

1. Filter for Internet Monitor by choosing `AWS/InternetMonitor`\.

1. Choose **MeasurementSource, MonitorName**\.

1. In the list, select **PerformanceScore**\.

1. On the **GraphedMetrics**tab, under **Actions**, choose the bell icon to create an alarm based on a static threshold\.

Now, follow the standard CloudWatch steps to choose options for the alarm\. For example, you can choose to be notified by an Amazon SNS message if **PerformanceScore** is below a specific threshold number\. Alternatively, or in addition, you can add the alarm to a dashboard\.

For more information about options when you create a CloudWatch alarm, see [Create a CloudWatch alarm based on a static threshold](ConsoleAlarms.md)\.