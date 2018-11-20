# Create a Load Balancer Latency Alarm that Sends Email<a name="US_AlarmAtThresholdELB"></a>

You can set up an Amazon SNS notification and configure an alarm that monitors latency exceeding 100 ms for your Classic Load Balancer\.

## Set Up a Latency Alarm Using the AWS Management Console<a name="load-balancer-alarm-console"></a>

Use these steps to use the AWS Management Console to create a load balancer latency alarm\.

**To create a load balancer latency alarm that sends email**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Under **CloudWatch Metrics by Category**, choose the **ELB Metrics** category\.

1. Select the row with the Classic Load Balancer and the **Latency** metric\.

1. For the statistic, choose **Average**, choose one of the predefined percentiles, or specify a custom percentile \(for example, p95\.45\)\.

1. For the period, choose **1 Minute**\.

1. Choose **Next**\.

1. Under **Alarm Threshold**, type a unique name for the alarm \(for example, **myHighCpuAlarm**\) and a description of the alarm \(for example, Alarm when Latency exceeds 100s\)\. Alarm names must contain only ASCII characters\.

1. Under **Whenever**, for **is**, choose **>** and type **0\.1**\. For **for**, type **3**\.

1. Under **Additional settings**, for **Treat missing data as**, choose **ignore \(maintain alarm state\)** so that missing data points do not trigger alarm state changes\.

   For **Percentiles with low samples**, choose **ignore \(maintain the alarm state\)** so that the alarm evaluates only situations with adequate numbers of data samples\. 

1. Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, choose an existing SNS topic or create a new one\.

   To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic \(for example, **myHighCpuAlarm**\), and for **Email list**, type a comma\-separated list of email addresses to be notified when the alarm changes to the `ALARM` state\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent\.

1. Choose **Create Alarm**\.

## Set Up a Latency Alarm Using the AWS CLI<a name="load-balancer-alarm-cli"></a>

Use these steps to use the AWS CLI to create a load balancer latency alarm\.

**To create a load balancer latency alarm that sends email**

1. Set up an SNS topic\. For more information, see [Set Up Amazon SNS Notifications](US_SetupSNS.md)\.

1. Create the alarm using the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command as follows:

   ```
   1. aws cloudwatch put-metric-alarm --alarm-name lb-mon --alarm-description "Alarm when Latency exceeds 100s" --metric-name Latency --namespace AWS/ELB --statistic Average --period 60 --threshold 100 --comparison-operator GreaterThanThreshold --dimensions Name=LoadBalancerName,Value=my-server --evaluation-periods 3 --alarm-actions arn:aws:sns:us-east-1:111122223333:my-topic --unit Seconds
   ```

1. Test the alarm by forcing an alarm state change using the [set\-alarm\-state](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/set-alarm-state.html) command\.

   1. Change the alarm state from `INSUFFICIENT_DATA` to `OK`: 

      ```
      1. aws cloudwatch set-alarm-state --alarm-name lb-mon --state-reason "initializing" --state-value OK
      ```

   1. Change the alarm state from `OK` to `ALARM`:

      ```
      1. aws cloudwatch set-alarm-state --alarm-name lb-mon --state-reason "initializing" --state-value ALARM
      ```

   1. Check that you have received an email notification about the alarm\.