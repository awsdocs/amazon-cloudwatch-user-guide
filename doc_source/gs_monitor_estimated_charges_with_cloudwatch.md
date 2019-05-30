# Scenario: Monitor Your Estimated Charges Using CloudWatch<a name="gs_monitor_estimated_charges_with_cloudwatch"></a>

In this scenario, you create an Amazon CloudWatch alarm to monitor your estimated charges\. When you enable the monitoring of estimated charges for your AWS account, the estimated charges are calculated and sent several times daily to CloudWatch as metric data\.

Billing metric data is stored in the US East \(N\. Virginia\) Region and reflects worldwide charges\. This data includes the estimated charges for every service in AWS that you use, as well as the estimated overall total of your AWS charges\.

You can choose to receive alerts by email when charges have exceeded a certain threshold\. These alerts are triggered by CloudWatch and messages are sent using Amazon Simple Notification Service \(Amazon SNS\)\.

**Topics**
+ [Step 1: Enable Billing Alerts](#gs_turning_on_billing_metrics)
+ [Step 2: Create a Billing Alarm](#gs_creating_billing_alarm)
+ [Step 3: Check the Alarm Status](#gs_checking_billing_alarm)
+ [Step 4: Edit a Billing Alarm](#gs_editing_billing_alarm)
+ [Step 5: Delete a Billing Alarm](#gs_deleting_billing_alarm)

## Step 1: Enable Billing Alerts<a name="gs_turning_on_billing_metrics"></a>

Before you can create an alarm for your estimated charges, you must enable billing alerts, so that you can monitor your estimated AWS charges and create an alarm using billing metric data\. After you enable billing alerts, you cannot disable data collection, but you can delete any billing alarms that you created\.

After you enable billing alerts for the first time, it takes about 15 minutes before you can view billing data and set billing alarms\.

**Requirements**
+ You must be signed in as either an AWS account root user or an IAM user whose access to the Billing and Cost Management console has been activated\.
+ For consolidated billing accounts, billing data for each linked account can be found by logging in as the paying account\. You can view billing data for total estimated charges and estimated charges by service for each linked account as well as for the consolidated account\.

**To enable monitoring of your estimated charges**

1. Open the Billing and Cost Management console at [https://console\.aws\.amazon\.com/billing/home?\#](https://console.aws.amazon.com/billing/home?#/)\.

1. In the navigation pane, choose **Preferences**\.

1. Select **Receive Billing Alerts**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/billing_preferences.png)

1. Choose **Save preferences**\.

## Step 2: Create a Billing Alarm<a name="gs_creating_billing_alarm"></a>

After you've enabled billing alerts, you can create a billing alarm\. In this scenario, you create an alarm that sends an email message when your estimated charges for AWS exceed a specified threshold\.

**Note**  
This procedure uses the simple options\. To use the advanced options, see [Create a Billing Alarm](monitor_estimated_charges_with_cloudwatch.md#creating_billing_alarm_with_wizard) in *Create a Billing Alarm to Monitor Your Estimated AWS Charges*\.

**To create a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select metric**, **Billing**, **Total Estimated Charge**\.

1. Select the checkbox next to **EstimatedCharges** and choose **Select metric**

1. For **Whenever my total AWS charges for the month exceed**, specify the monetary amount \(for example, 200\) that must be exceeded to trigger the alarm and send an email notification\.
**Tip**  
The graph shows a current estimate of your charges that you can use to set an appropriate amount\.

1. For **send a notification to**, choose an existing notification list or create a new one\.

   To create a list, choose **New list** and type a comma\-separated list of email addresses to be notified when the alarm changes to the ALARM state\. Each email address is sent a subscription confirmation email\. The recipient must confirm the subscription before notifications can be sent to the email address\.

1. Choose **Create Alarm**\. 

## Step 3: Check the Alarm Status<a name="gs_checking_billing_alarm"></a>

Now, check the status of the billing alarm that you just created\.

**To check the alarm status**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm\. Until the subscription is confirmed, it is shown as "Pending confirmation"\. After the subscription is confirmed, refresh the console to show the updated status\.

## Step 4: Edit a Billing Alarm<a name="gs_editing_billing_alarm"></a>

For example, you may want to increase the amount of money you spend with AWS each month from $200 to $400\. You can edit your existing billing alarm and increase the monetary amount that must be exceeded before the alarm is triggered\.

**To edit a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm and choose **Actions**, **Modify**\.

1. For **Whenever my total AWS charges for the month exceed**, specify the new amount that must be exceeded to trigger the alarm and send an email notification\.

1. Choose **Save Changes**\.

## Step 5: Delete a Billing Alarm<a name="gs_deleting_billing_alarm"></a>

If you no longer need your billing alarm, you can delete it\.

**To delete a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm and choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.