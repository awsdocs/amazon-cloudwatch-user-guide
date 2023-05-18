# Create a CloudWatch alarm based on a log group\-metric filter<a name="Create_alarm_log_group_metric_filter"></a>

 The procedure in this section describes how to create an alarm based on a log group\-metric filter\. With metric filters, you can look for terms and patterns in log data as the data is sent to CloudWatch\. For more information, see [Create metrics from log events using filters](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html) in the *Amazon CloudWatch Logs User Guide*\. Before you create an alarm based on a log group\-metric filter, you must complete the following actions: 
+  Create a log group\. For more information, see [Working with log groups and log streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html#Create-Log-Group) in the *Amazon CloudWatch Logs User Guide*\. 
+  Create a metric filter\. For more information, see [Create a metric filter for a log group](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CreateMetricFilterProcedure.html) in the *Amazon CloudWatch Logs User Guide*\. 

**To create an alarm based on a log group\-metric filter**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  From the navigation pane, choose **Logs**, and then choose **Log groups**\. 

1.  Choose the log group that includes your metric filter\. 

1.  Choose **Metric filters**\. 

1.  In the metric filters tab, select the box for the metric filter that you want to base your alarm on\. 

1.  Choose **Create alarm**\. 

1.  \(Optional\) Under **Metric**, edit **Metric name**, **Statistic**, and **Period**\. 

1.  Under **Conditions**, specify the following: 

   1.  For **Threshold type**, choose **Static** or **Anomaly detection**\. 

   1.  For **Whenever *your\-metric\-name* is \. \. \.**, choose **Greater**, **Greater/Equal**, **Lower/Equal** , or **Lower**\. 

   1.  For **than \. \. \.**, specify a number for your threshold value\. 

1.  Choose **Additional configuration**\. 

   1.  For **Data points to alarm**, specify how many data points trigger your alarm to go into the `ALARM` state\. If you specify matching values, your alarm goes into the `ALARM` state if that many consecutive periods are breaching\. To create an M\-out\-of\-N alarm, specify a number for the first value that's lower than the number you specify for the second value\. For more information, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html#alarm-evaluation)\. 

   1.  For **Missing data treatment**, select an option to specify how to treat missing data when your alarm is evaluated\. 

1.  Choose **Next**\. 

1.  For **Notification**, specify an Amazon SNS topic to notify when your alarm is in the `ALARM`, `OK`, or `INSUFFICIENT_DATA` state\. 

   1.  \(Optional\) To send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\. 

   1.  \(Optional\) To not send notifications, choose **Remove**\. 

1.  \(Optional\) If you want your alarm to perform actions for Amazon EC2 Auto Scaling, Amazon EC2, tickets, or AWS Systems Manager, choose the appropriate button, and specify the alarm state and action\. 
**Note**  
 Your alarm can perform Systems Manager actions only when it's in the `ALARM` state\. For information about Systems Manager actions, see [Configuring CloudWatch to create OpsItems](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter-create-OpsItems-from-CloudWatch-Alarms.html) and [Incident creation](https://docs.aws.amazon.com/incident-manager/latest/userguide/incident-creation.html)\. 

1.  Choose **Next**\. 

1.  For **Name and description**, enter a name and description for your alarm\. 

1.  For **Preview and create**, check that your configuration is correct, and choose **Create alarm**\. 