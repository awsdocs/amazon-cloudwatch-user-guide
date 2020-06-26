# Creating a CPU Usage Alarm That Sends Email<a name="US_AlarmAtThresholdEC2"></a>

You can create an CloudWatch alarm that sends an email message using Amazon SNS when the alarm changes state from `OK` to `ALARM`\.

The alarm changes to the `ALARM` state when the average CPU use of an EC2 instance exceeds a specified threshold for consecutive specified periods\.

## Setting Up a CPU Usage Alarm Using the AWS Management Console<a name="cpu-usage-alarm-console"></a>

Use these steps to use the AWS Management Console to create a CPU usage alarm\.

**To create an alarm based on CPU usage**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select metric**\.

1. In the **All metrics** tab, choose **EC2 metrics**\.

1. Choose a metric category \(for example, **Per\-Instance Metrics**\)\.

1. Find the row with the instance that you want listed in the **InstanceId** column and **CPUUtilization** in the **Metric Name** column\. Select the check box next to this row, and choose **Select metric**\.

1. Under **Specify metric and conditions**, for **Statistic** choose **Average**, choose one of the predefined percentiles, or specify a custom percentile \(for example, **p95\.45**\)\.

1. Choose a period \(for example, **5 minutes**\)\.

1. Under **Conditions**, specify the following:

   1. For **Threshold type**, choose **Static**\.

   1. For **Whenever CPUUtilization is**, specify **Greater**\. Under **than\.\.\.**, specify the threshold that is to trigger the alarm to go to ALARM state if the CPU utilization exceeds this percentage\. For example, 70\.

   1. Choose **Additional configuration**\. For **Datapoints to alarm**, specify how many evaluation periods \(data points\) must be in the `ALARM` state to trigger the alarm\. If the two values here match, you create an alarm that goes to `ALARM` state if that many consecutive periods are breaching\.

      To create an M out of N alarm, specify a lower number for the first value than you specify for the second value\. For more information, see [Evaluating an Alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

   1. For **Missing data treatment**, choose how to have the alarm behave when some data points are missing\. For more information, see [Configuring How CloudWatch Alarms Treat Missing Data](AlarmThatSendsEmail.md#alarms-and-missing-data)\.

   1. If the alarm uses a percentile as the monitored statistic, a **Percentiles with low samples** box appears\. Use it to choose whether to evaluate or ignore cases with low sample rates\. If you choose **ignore \(maintain alarm state\)**, the current alarm state is always maintained when the sample size is too low\. For more information, see [Percentile\-Based CloudWatch Alarms and Low Data Samples](AlarmThatSendsEmail.md#percentiles-with-low-samples)\. 

1. Choose **Next**\.

1. Under **Notification**, choose **In alarm** and select an SNS topic to notify when the alarm is in `ALARM` state

   To have the alarm send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\.

   To have the alarm not send notifications, choose **Remove**\.

1. When finished, choose **Next**\.

1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then choose **Next**\.

1. Under **Preview and create**, confirm that the information and conditions are what you want, then choose **Create alarm**\.

## Setting Up a CPU Usage Alarm Using the AWS CLI<a name="cpu-usage-alarm-cli"></a>

Use these steps to use the AWS CLI to create a CPU usage alarm\.

**To create an alarm that sends email based on CPU usage**

1. Set up an SNS topic\. For more information, see [Setting Up Amazon SNS Notifications](US_SetupSNS.md)\.

1. Create an alarm using the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command as follows\. 

   ```
   aws cloudwatch put-metric-alarm --alarm-name cpu-mon --alarm-description "Alarm when CPU exceeds 70%" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanThreshold --dimensions  Name=InstanceId,Value=i-12345678 --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:111122223333:my-topic --unit Percent
   ```

1. Test the alarm by forcing an alarm state change using the [set\-alarm\-state](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/set-alarm-state.html) command\.

   1. Change the alarm state from `INSUFFICIENT_DATA` to `OK`\.

      ```
      aws cloudwatch set-alarm-state --alarm-name cpu-mon --state-reason "initializing" --state-value OK
      ```

   1. Change the alarm state from `OK` to `ALARM`\.

      ```
      aws cloudwatch set-alarm-state --alarm-name cpu-mon --state-reason "initializing" --state-value ALARM
      ```

   1. Check that you have received an email notification about the alarm\.