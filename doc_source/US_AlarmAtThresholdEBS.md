# Creating a Storage Throughput Alarm that Sends Email<a name="US_AlarmAtThresholdEBS"></a>

You can set up an SNS notification and configure an alarm that sends email when Amazon EBS exceeds 100 MB throughput\.

## Setting Up a Storage Throughput Alarm Using the AWS Management Console<a name="storage-alarm-console"></a>

Use these steps to use the AWS Management Console to create an alarm based on Amazon EBS throughput\.

**To create a storage throughput alarm that sends email**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Under **EBS Metrics**, choose a metric category\.

1. Select the row with the volume and the **VolumeWriteBytes** metric\.

1. For the statistic, choose **Average**\. For the period, choose **5 Minutes**\. Choose **Next**\.

1. Under **Alarm Threshold**, enter a unique name for the alarm \(for example, **myHighWriteAlarm**\) and a description of the alarm \(for example, **VolumeWriteBytes exceeds 100,000 KiB/s**\)\. Alarm names must contain only ASCII characters\.

1. Under **Whenever**, for **is**, choose **>** and enter **100000**\. For **for**, enter **15** consecutive periods\.

   A graphical representation of the threshold is shown under **Alarm Preview**\.

1. Under **Additional settings**, for **Treat missing data as**, choose **ignore \(maintain alarm state\)** so that missing data points don't trigger alarm state changes\.

1. Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, choose an existing SNS topic or create one\.

   To create an SNS topic, choose **New list**\. For **Send notification to**, enter a name for the SNS topic \(for example, **myHighCpuAlarm**\), and for **Email list**, enter a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent to an email address\.

1. Choose **Create Alarm**\.

## Setting Up a Storage Throughput Alarm Using the AWS CLI<a name="storage-alarm-cli"></a>

Use these steps to use the AWS CLI to create an alarm based on Amazon EBS throughput\.

**To create a storage throughput alarm that sends email**

1. Create an SNS topic\. For more information, see [Setting Up Amazon SNS Notifications](US_SetupSNS.md)\.

1. Create the alarm\.

   ```
   1. aws cloudwatch put-metric-alarm --alarm-name ebs-mon --alarm-description "Alarm when EBS volume exceeds 100MB throughput" --metric-name VolumeReadBytes --namespace AWS/EBS --statistic Average --period 300 --threshold 100000000 --comparison-operator GreaterThanThreshold --dimensions Name=VolumeId,Value=my-volume-id --evaluation-periods 3 --alarm-actions arn:aws:sns:us-east-1:111122223333:my-alarm-topic --insufficient-data-actions arn:aws:sns:us-east-1:111122223333:my-insufficient-data-topic
   ```

1. Test the alarm by forcing an alarm state change using the [set\-alarm\-state](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/set-alarm-state.html) command\.

   1. Change the alarm state from `INSUFFICIENT_DATA` to `OK`\.

      ```
      1. aws cloudwatch set-alarm-state --alarm-name ebs-mon --state-reason "initializing" --state-value OK
      ```

   1. Change the alarm state from `OK` to `ALARM`\.

      ```
      1. aws cloudwatch set-alarm-state --alarm-name ebs-mon --state-reason "initializing" --state-value ALARM
      ```

   1. Change the alarm state from `ALARM` to `INSUFFICIENT_DATA`\.

      ```
      1. aws cloudwatch set-alarm-state --alarm-name ebs-mon --state-reason "initializing" --state-value INSUFFICIENT_DATA
      ```

   1. Check that you have received an email notification about the alarm\.