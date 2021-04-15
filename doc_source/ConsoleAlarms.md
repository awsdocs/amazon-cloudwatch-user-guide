# Create a CloudWatch Alarm Based on a Static Threshold<a name="ConsoleAlarms"></a>

You choose a CloudWatch metric for the alarm to watch, and the threshold for that metric\. The alarm goes to `ALARM` state when the metric breaches the threshold for a specified number of evaluation periods\.

**To create an alarm based on a single metric**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select Metric** and do one of the following:
   + Choose the service namespace that contains the metric that you want\. Continue choosing options as they appear to narrow the choices\. When a list of metrics appears, select the check box next to the metric that you want\.
   + In the search box, enter the name of a metric, dimension, or resource ID and press Enter\. Then choose one of the results and continue until a list of metrics appears\. Select the check box next to the metric that you want\. 

1. Choose the **Graphed metrics** tab\.

   1. Under **Statistic** , choose one of the statistics or predefined percentiles, or specify a custom percentile \(for example, **p95\.45**\)\.

   1. Under **Period**, choose the evaluation period for the alarm\. When evaluating the alarm, each period is aggregated into one data point\.

      You can also choose whether the y\-axis legend appears on the left or right while you're creating the alarm\. This preference is used only while you're creating the alarm\.

   1. Choose **Select metric**\.

      The **Specify metric and conditions** page appears, showing a graph and other information about the metric and statistic you have selected\.

1. Under **Conditions**, specify the following:

   1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. 

   1. For **Whenever *metric* is**, specify whether the metric must be greater than, less than, or equal to the threshold\. Under **than\.\.\.**, specify the threshold value\.

   1. Choose **Additional configuration**\. For **Datapoints to alarm**, specify how many evaluation periods \(data points\) must be in the `ALARM` state to trigger the alarm\. If the two values here match, you create an alarm that goes to `ALARM` state if that many consecutive periods are breaching\.

      To create an M out of N alarm, specify a lower number for the first value than you specify for the second value\. For more information, see [Evaluating an Alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

   1. For **Missing data treatment**, choose how to have the alarm behave when some data points are missing\. For more information, see [Configuring How CloudWatch Alarms Treat Missing Data](AlarmThatSendsEmail.md#alarms-and-missing-data)\.

   1. If the alarm uses a percentile as the monitored statistic, a **Percentiles with low samples** box appears\. Use it to choose whether to evaluate or ignore cases with low sample rates\. If you choose **ignore \(maintain alarm state\)**, the current alarm state is always maintained when the sample size is too low\. For more information, see [Percentile\-Based CloudWatch Alarms and Low Data Samples](AlarmThatSendsEmail.md#percentiles-with-low-samples)\. 

1. Choose **Next**\.

1. Under **Notification**, select an SNS topic to notify when the alarm is in `ALARM` state, `OK` state, or `INSUFFICIENT_DATA` state\.

   To have the alarm send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\.

   To have the alarm not send notifications, choose **Remove**\.

1. To have the alarm perform Auto Scaling, EC2, or Systems Manager actions, choose the appropriate button and choose the alarm state and action to perform\.

1. When finished, choose **Next**\.

1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then choose **Next**\.

1. Under **Preview and create**, confirm that the information and conditions are what you want, then choose **Create alarm**\.

You can also add alarms to a dashboard\. For more information, see [Add an Alarm Widget to a CloudWatch Dashboard](add_remove_alarm_dashboard.md)\. 