# Create a Billing Alarm to Monitor Your Estimated AWS Charges<a name="monitor_estimated_charges_with_cloudwatch"></a>

You can monitor your estimated AWS charges using Amazon CloudWatch\. When you enable the monitoring of estimated charges for your AWS account, the estimated charges are calculated and sent several times daily to CloudWatch as metric data\.

Billing metric data is stored in the US East \(N\. Virginia\) region and represents worldwide charges\. This data includes the estimated charges for every service in AWS that you use, in addition to the estimated overall total of your AWS charges\.

The alarm triggers when your account billing exceeds the threshold you specify\. It triggers only when actual billing exceeds the threshold\. It does not use projections based on your usage so far in the month\.

If you create a billing alarm at a time when your charges have already exceeded the threshold, the alarm goes to the `ALARM` state immediately\.

**Topics**
+ [Enable Billing Alerts](#turning_on_billing_metrics)
+ [Create a Billing Alarm](#creating_billing_alarm_with_wizard)
+ [Check the Alarm Status](#checking_billing_alarm)
+ [Delete a Billing Alarm](#deleting_billing_alarm)

## Enable Billing Alerts<a name="turning_on_billing_metrics"></a>

Before you can create an alarm for your estimated charges, you must enable billing alerts, so that you can monitor your estimated AWS charges and create an alarm using billing metric data\. After you enable billing alerts, you cannot disable data collection, but you can delete any billing alarms that you created\.

After you enable billing alerts for the first time, it takes about 15 minutes before you can view billing data and set billing alarms\.

**Requirements**
+ You must be signed in using AWS account root user credentials; IAM users cannot enable billing alerts for your AWS account\.
+ For consolidated billing accounts, billing data for each linked account can be found by logging in as the paying account\. You can view billing data for total estimated charges and estimated charges by service for each linked account, in addition to the consolidated account\.

**To enable the monitoring of estimated charges**

1. Open the Billing and Cost Management console at [https://console\.aws\.amazon\.com/billing/home?\#](https://console.aws.amazon.com/billing/home?#/)\.

1. In the navigation pane, choose **Preferences**\.

1. Choose **Receive Billing Alerts**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/billing_preferences.png)

1. Choose **Save preferences**\.

## Create a Billing Alarm<a name="creating_billing_alarm_with_wizard"></a>

After you've enabled billing alerts, you can create a billing alarm\. In this procedure, you create an alarm that sends an email message when your estimated charges for AWS exceed a specified threshold\.

**Note**  
This procedure uses the advanced options\. For more information about using the simple options, see [Create a Billing Alarm](gs_monitor_estimated_charges_with_cloudwatch.md#gs_creating_billing_alarm) in *Monitor Your Estimated Charges Using CloudWatch*\.

**To create a billing alarm using the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the region to US East \(N\. Virginia\)\. Billing metric data is stored in this region and represents worldwide charges\.

1. In the navigation pane, choose **Alarms**, **Billing**, **Create Alarm**\.

1. Choose **show advanced** to switch to the advanced options\.

1. Under **Alarm Threshold**, replace the default name for the alarm \(for example, My Estimated Charges\) and a description for the alarm \(for example, Estimated Monthly Charges\)\. Alarm names must contain only ASCII characters\.

1. Under **Whenever charges for**, for **is**, choose **>=** and then type the monetary amount \(for example, 200\) that must be exceeded to trigger the alarm and send an email\.
**Note**  
Under **Alarm Preview**, there is an estimate of your charges that you can use to set an appropriate amount\.

1. Under **Additional settings**, for **Treat missing data as**, choose **ignore \(maintain alarm state\)** so that missing data points do not trigger alarm state changes\.

1. Under **Actions**, for **Whenever this alarm**, choose **State is ALARM**\. For **Send notification to**, choose an existing SNS topic or create a new one\.

   To create an SNS topic, choose **New list**\. For **Send notification to**, type a name for the SNS topic, and for **Email list**, type a comma\-separated list of email addresses where email notifications should be sent\. Each email address is sent a topic subscription confirmation email\. You must confirm the subscription before notifications can be sent to an email address\.

1. Choose **Create Alarm**\.

## Check the Alarm Status<a name="checking_billing_alarm"></a>

You can check the status of your billing alarm\.

**To check alarm status**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the region to US East \(N\. Virginia\)\. Billing metric data is stored in this region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**, **Billing**\.

1. Select the check box next to the alarm\. Until the subscription is confirmed, it is shown as "Pending confirmation"\. After the subscription is confirmed, refresh the console to show the updated status\.

## Delete a Billing Alarm<a name="deleting_billing_alarm"></a>

You can delete your billing alarm when you no longer need it\.

**To delete a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the region to US East \(N\. Virginia\)\. Billing metric data is stored in this region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**, **Billing**\.

1. Select the check box next to the alarm and choose **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.