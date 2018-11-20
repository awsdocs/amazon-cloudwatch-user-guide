# Create a CloudWatch Alarm Based on a CloudWatch Metric<a name="ConsoleAlarms"></a>

You choose a CloudWatch metric to for the alarm to watch, and the threshold for that metric\. The alarm goes to ALARM state when the metric breaches the threshold for a specified number of evaluation periods\.

**To create an alarm based on a single metric**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select Metric** and do one of the following:

   1. Choose the service namespace that contains the metric you want\. Continue choosing options as they appear to narrow the choices\. When a list of metrics is displayed, select the checkbox next to the metric you want\.

      or

   1. In the search box, type the name of a metric, dimension, or resource ID and press ENTER\. Then choose one of the results and continue until a list of metrics is displayed\. Select the checkbox next to the metric you want\. 

1. Choose the **Graphed metrics** tab\.

   1. Under **Statistic** , choose one of the statistics or predefined percentiles, or specify a custom percentile \(for example, p95\.45\)\.

   1. Under **Period**, choose the evaluation period for the alarm\. When evaluating the alarm, each period is aggregated into one data point\.

      You can also choose whether the Y axis legend appears on the left or right while you are creating the alarm\. This preference is used only while you are creating the alarm\.

   1. Choose **Select metric**\.

1. Type a name and description for the alarm\. The name must contain only ASCII characters\. 

1. For **Whenever**, specify the alarm condition\.

   1. For **is:**, specify whether the metric must be greater than, less than, or equal to the threshold, and specify the threshold value\.

   1. For **for:**, specify how many evaluation periods \(datapoints\) must be in the ALARM state to trigger the alarm\. Initially, you can change only the second value, and the first value changes to match your entry\. This creates an alarm that goes to ALARM state if that many consecutive periods are breaching\.

      To create an M out of N alarm, choose the pencil icon\. You can then change the M number to be different than the N number\. For more information, see [Evaluating an Alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

1. Under **Additional settings**, for **Treat missing data as**, choose how to have the alarm behave when some data points are missing\. For more information, see [Configuring How CloudWatch Alarms Treat Missing Data](AlarmThatSendsEmail.md#alarms-and-missing-data)\.

1. If the alarm uses a percentile as the monitored statistic, a **Percentiles with low samples** box is displayed\. Use it to choose whether to evaluate or ignore cases with low sample rates\. If you choose **ignore \(maintain the alarm state\)**, the current alarm state is always maintained when the sample size is too low\. For more information, see [Percentile\-Based CloudWatch Alarms and Low Data Samples](AlarmThatSendsEmail.md#percentiles-with-low-samples)\. 

1. Under **Actions**, select the type of action to have the alarm to perform when the alarm is triggered\. Use the **\+Notification**, **\+AutoScaling Action**, and **\+EC2 Action** buttons to have the alarm perform multiple actions\. Specify at least one action\.

1. Choose **Create Alarm**\.

You can also add alarms to a dashboard\. For more information, see [Add or Remove an Alarm from a CloudWatch Dashboard](add_remove_alarm_dashboard.md)\. 