# Create a CPU Usage Alarm that Sends Email<a name="US_AlarmAtThresholdEC2"></a>

You can create an CloudWatch alarm that sends an email message using Amazon SNS when the alarm changes state from OK to ALARM\.

The alarm changes to the ALARM state when the average CPU use of an EC2 instance exceeds a specified threshold for consecutive specified periods\.

## Set Up a CPU Usage Alarm Using the AWS Management Console<a name="cpu-usage-alarm-console"></a>

Use these steps to use the AWS Management Console to create a CPU usage alarm\.

**To create an alarm that sends email based on CPU usage**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Under **EC2 Metrics**, choose a metric category \(for example, **Per\-Instance Metrics**\)\.

1. Select a metric as follows:

   1. Select a row with the instance and the **CPUUtilization** metric\.

   1. For the statistic, choose **Average**, choose one of the predefined percentiles, or specify a custom percentile \(for example, p95\.45\)\.

   1. Choose a period \(for example, **5 minutes**\)\.

   1. Choose **Next**\.  
![\[CPUUtilization metric selected\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/alarm_create_send_email.png)

1. Define the alarm as follows:

   1. Under **Alarm Threshold**, type a unique name for the alarm \(for example, myHighCpuAlarm\) and a description of the alarm \(for example, CPU usage exceeds 70 percent\)\. Alarm names must contain only ASCII characters\.

   1. Under **Whenever**, for **is**, choose **>** and type **70**\. For **for**, type **2**\. This specifies that the alarm is triggered if the CPU usage is above 70 percent for two consecutive sampling periods\.  
![\[Alarm threshold specified\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/create-alarm-threshold.png)

   1. Under **Additional settings**, for **Treat missing data as**, choose **bad \(breaching threshold\)**, as missing data points may indicate that the instance is down\. 

   1. Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, select an existing SNS topic or create a new one\.  
![\[Alarm actions specified\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/create-alarm-actions.png)

   1. To create a new SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, myHighCpuAlarm\), and for **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent\.

   1. Choose **Create Alarm**\.

## Set Up a CPU Usage Alarm Using the AWS CLI<a name="cpu-usage-alarm-cli"></a>

Use these steps to use the AWS CLI to create a CPU usage alarm\.

**To create an alarm that sends email based on CPU usage**

1. Set up an SNS topic\. For more information, see [Set Up Amazon SNS Notifications](US_SetupSNS.md)\.

1. Create an alarm using the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command as follows\. 

   ```
   aws cloudwatch put-metric-alarm --alarm-name cpu-mon --alarm-description "Alarm when CPU exceeds 70%" --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanThreshold --dimensions  Name=InstanceId,Value=i-12345678 --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:111122223333:my-topic --unit Percent
   ```

1. Test the alarm by forcing an alarm state change using the [set\-alarm\-state](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/set-alarm-state.html) command\.

   1. Change the alarm state from `INSUFFICIENT_DATA` to `OK`: 

      ```
      aws cloudwatch set-alarm-state --alarm-name cpu-mon --state-reason "initializing" --state-value OK
      ```

   1. Change the alarm state from `OK` to `ALARM`:

      ```
      aws cloudwatch set-alarm-state --alarm-name cpu-mon --state-reason "initializing" --state-value ALARM
      ```

   1. Check that you have received an email notification about the alarm\.