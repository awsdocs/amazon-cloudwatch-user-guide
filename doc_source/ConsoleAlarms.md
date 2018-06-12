# Create or Edit a CloudWatch Alarm<a name="ConsoleAlarms"></a>

You can choose specific metrics to trigger the alarm and specify thresholds for those metrics\. You can then set your alarm to change state when a metric exceeds a threshold that you have defined\.

**To create an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. For the **Select Metric** step, do the following:

   1. Choose a metric category \(for example, **EC2 Metrics**\)\.

   1. Select an instance and metric \(for example, **CPUUtilization**\)\.

   1. For the statistic, choose one of the statistics \(for example, Average\) or predefined percentiles, or specify a custom percentile \(for example, p95\.45\)\.

   1. Choose a period \(for example, **1 Hour**\)\.

   1. Choose **Next**\.

1. For the **Define Alarm** step, do the following:

   1. Under **Alarm Threshold**, type a unique name for the alarm and a description of the alarm\. The alarm name must contain only ASCII characters\. For **Whenever**, specify a threshold \(for example, 80 percent of CPU utilization\) and the number of datapoints \("M" out of "N"\) that must be breaching to trigger the alarm\. For more information, see [Evaluating an Alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

   1. Under **Additional settings**, for **Treat missing data as**, choose how to have the alarm treat missing data points\. For more information, see [Configuring How CloudWatch Alarms Treat Missing Data](AlarmThatSendsEmail.md#alarms-and-missing-data)\.

      If the alarm uses a percentile as the monitored statistic, choose whether to evaluate or ignore cases with low sample rates\. If you choose **ignore**, the current alarm state is maintained when the sample size is too low\. For more information, see [Percentile\-Based CloudWatch Alarms and Low Data Samples](AlarmThatSendsEmail.md#percentiles-with-low-samples)\.   
![\[Treat missing data and Percentiles with low samples UI fields\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/alarms_missing_data.PNG)

   1. Under **Actions**, select the type of action to have the alarm to perform when the alarm is triggered\.

   1. Choose **Create Alarm**\.

You can also add alarms to a dashboard\. For more information, see [Add or Remove an Alarm from a CloudWatch Dashboard](add_remove_alarm_dashboard.md)\. 

**To edit an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. Select the alarm, and then choose **Actions**, **Modify**\.

1. In the **Modify Alarm** dialog box, update the alarm as necessary and choose **Save Changes**\.

   You cannot change the name of an existing alarm\. You can change the description\. Or you can copy the alarm, and give the new alarm a different name\. To copy an alarm, select the alarm and then choose **Actions**, **Copy**\.

**To update an email notification list that was created using the Amazon SNS console**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

1. In the navigation pane, choose **Topics**, and then select the ARN for your notification list \(topic\)\.

1. Do one of the following:
   + To add an email address, choose **Create subscription**\. For **Protocol**, choose **Email**\. For **Endpoint**, type the email address of the new recipient\. Choose **Create subscription**\.
   + To remove an email address, choose the **Subscription ID**\. Choose **Other subscription actions**, **Delete subscriptions**\.

1. Choose **Publish to topic**\.