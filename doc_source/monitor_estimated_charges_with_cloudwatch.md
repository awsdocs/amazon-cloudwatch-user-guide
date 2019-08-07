# Creating a Billing Alarm to Monitor Your Estimated AWS Charges<a name="monitor_estimated_charges_with_cloudwatch"></a>

You can monitor your estimated AWS charges by using Amazon CloudWatch\. When you enable the monitoring of estimated charges for your AWS account, the estimated charges are calculated and sent several times daily to CloudWatch as metric data\.

Billing metric data is stored in the US East \(N\. Virginia\) Region and represents worldwide charges\. This data includes the estimated charges for every service in AWS that you use, in addition to the estimated overall total of your AWS charges\.

The alarm triggers when your account billing exceeds the threshold you specify\. It triggers only when actual billing exceeds the threshold\. It doesn't use projections based on your usage so far in the month\.

If you create a billing alarm at a time when your charges have already exceeded the threshold, the alarm goes to the `ALARM` state immediately\.

**Topics**
+ [Enabling Billing Alerts](#turning_on_billing_metrics)
+ [Creating a Billing Alarm](#creating_billing_alarm_with_wizard)
+ [Deleting a Billing Alarm](#deleting_billing_alarm)

## Enabling Billing Alerts<a name="turning_on_billing_metrics"></a>

Before you can create an alarm for your estimated charges, you must enable billing alerts, so that you can monitor your estimated AWS charges and create an alarm using billing metric data\. After you enable billing alerts, you can't disable data collection, but you can delete any billing alarms that you created\.

After you enable billing alerts for the first time, it takes about 15 minutes before you can view billing data and set billing alarms\.

**Requirements**
+ You must be signed in using account root user credentials or as an IAM user that has been given permission to view billing information\.
+ For consolidated billing accounts, billing data for each linked account can be found by logging in as the paying account\. You can view billing data for total estimated charges and estimated charges by service for each linked account, in addition to the consolidated account\.

**To enable the monitoring of estimated charges**

1. Open the Billing and Cost Management console at [https://console\.aws\.amazon\.com/billing/](https://console.aws.amazon.com/billing/home?#/)\.

1. In the navigation pane, choose **Billing Preferences**\.

1. Choose **Receive Billing Alerts**\.

1. Choose **Save preferences**\.

## Creating a Billing Alarm<a name="creating_billing_alarm_with_wizard"></a>

After you've enabled billing alerts, you can create a billing alarm\. In this procedure, you create an alarm that sends an email message when your estimated charges for AWS exceed a specified threshold\.

**Note**  
This procedure uses the advanced options\. For more information about using the simple options, see [Create a Billing Alarm](gs_monitor_estimated_charges_with_cloudwatch.md#gs_creating_billing_alarm) in *Monitor Your Estimated Charges Using CloudWatch*\.

**To create a billing alarm using the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and represents worldwide charges\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select metric**\. In the **All metrics** tab, choose **Billing**, **Total Estimated Charge**\.

1. Select the check box next to **EstimatedCharges**, and choose **Select metric**\.

1. Under **Conditions**, choose **Static**\.

1. For **Whenever EstimatedCharges is**, choose **Greater**\.

1. For **than**, enter the monetary amount \(for example, **200**\) that must be exceeded to trigger the alarm\.
**Note**  
The preview graph displays your current charges for the month\.

1. Choose **Next**\.

1. Under **Notification**, select an SNS topic to notify when the alarm is in `ALARM` state\.

   To have the alarm send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\.

1. When finished, choose **Next**\.

1. Enter a name and description for the alarm\. The name must contain only ASCII characters\. Then choose **Next**\.

1. Under **Preview and create**, confirm that the information and conditions are what you want, then choose **Create alarm**\.

## Deleting a Billing Alarm<a name="deleting_billing_alarm"></a>

You can delete your billing alarm when you no longer need it\.

**To delete a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm and choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.