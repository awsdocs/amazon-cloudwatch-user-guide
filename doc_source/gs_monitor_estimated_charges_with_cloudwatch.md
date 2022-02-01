# Scenario: Monitor your estimated charges using CloudWatch<a name="gs_monitor_estimated_charges_with_cloudwatch"></a>

In this scenario, you create an Amazon CloudWatch alarm to monitor your estimated charges\. When you enable the monitoring of estimated charges for your AWS account, the estimated charges are calculated and sent several times daily to CloudWatch as metric data\.

Billing metric data is stored in the US East \(N\. Virginia\) Region and reflects worldwide charges\. This data includes the estimated charges for every service in AWS that you use, as well as the estimated overall total of your AWS charges\.

You can choose to receive alerts by email when charges have exceeded a certain threshold\. These alerts are triggered by CloudWatch and messages are sent using Amazon Simple Notification Service \(Amazon SNS\)\.

**Topics**
+ [Step 1: Enable billing alerts](#gs_turning_on_billing_metrics)
+ [Step 2: Create a billing alarm](#gs_creating_billing_alarm)
+ [Step 3: Check the alarm status](#gs_checking_billing_alarm)
+ [Step 4: Edit a billing alarm](#gs_editing_billing_alarm)
+ [Step 5: Delete a billing alarm](#gs_deleting_billing_alarm)

## Step 1: Enable billing alerts<a name="gs_turning_on_billing_metrics"></a>

Before you can create an alarm for your estimated charges, you must enable billing alerts, so that you can monitor your estimated AWS charges and create an alarm using billing metric data\. After you enable billing alerts, you cannot disable data collection, but you can delete any billing alarms that you created\.

After you enable billing alerts for the first time, it takes about 15 minutes before you can view billing data and set billing alarms\.

**Requirements**
+ You must be signed in using account root user credentials or as an IAM user that has been given permission to view billing information\.
+ For consolidated billing accounts, billing data for each linked account can be found by logging in as the paying account\. You can view billing data for total estimated charges and estimated charges by service for each linked account, in addition to the consolidated account\.
+ In a consolidated billing account, member linked account metrics are captured only if the payer account enables the **Receive Billing Alerts** preference\. If you change which account is your management/payer account, you must enable the billing alerts in the new management/payer account\.
+ The account must not be part of the Amazon Partner Network \(APN\) because billing metrics are not published to CloudWatch for APN accounts\. For more information, see [AWS Partner Network](https://aws.amazon.com/partners/)\.

**To enable monitoring of your estimated charges**

1. Open the Billing and Cost Management console at [https://console\.aws\.amazon\.com/billing/](https://console.aws.amazon.com/billing/home?#/)\.

1. In the navigation pane, choose **Preferences**\.

1. Select **Receive Billing Alerts**\.

1. Choose **Save preferences**\.

## Step 2: Create a billing alarm<a name="gs_creating_billing_alarm"></a>

**Important**  
Before you can create a billing alarm,you must enable billing alerts in your account, or in the management/payer account if you are using consolidated billing\. For more information, see [Enabling billing alerts](monitor_estimated_charges_with_cloudwatch.md#turning_on_billing_metrics)\.

After you've enabled billing alerts, you can create a billing alarm\. In this scenario, you create an alarm that sends an email message when your estimated charges for AWS exceed a specified threshold\.

**Note**  
This procedure uses the simple options\. To use the advanced options, see [Creating a billing alarm](monitor_estimated_charges_with_cloudwatch.md#creating_billing_alarm_with_wizard) in *Create a Billing Alarm to Monitor Your Estimated AWS Charges*\.

**To create a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select metric**, **Billing**, **Total Estimated Charge**\.

   If you don't see **Billing** or the **Total Estimated Charge** metric, you might need to enable billing alerts\. For more information, see [Step 1: Enable billing alerts](#gs_turning_on_billing_metrics)\.

1. Select the checkbox next to **EstimatedCharges** and choose **Select metric**

1. For **Whenever my total AWS charges for the month exceed**, specify the monetary amount \(for example, 200\) that must be exceeded to trigger the alarm and send an email notification\. Then choose **Next**\.
**Tip**  
The graph shows a current estimate of your charges that you can use to set an appropriate amount\.

1. For **send a notification to**, do one of the following:
   + Choose **Select an existing SNS topic** and then select the topic to notify under **Send a notification to**\. 
   + Choose **Create a new topic** and then type a name for the new SNS topic and enter the email addresses that are to receive the notifications\. Separate the email names with commas\.

1. Choose **Create Alarm**\. 

## Step 3: Check the alarm status<a name="gs_checking_billing_alarm"></a>

Now, check the status of the billing alarm that you just created\.

**To check the alarm status**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm\. Until the subscription is confirmed, it is shown as "Pending confirmation"\. After the subscription is confirmed, refresh the console to show the updated status\.

## Step 4: Edit a billing alarm<a name="gs_editing_billing_alarm"></a>

For example, you may want to increase the amount of money you spend with AWS each month from $200 to $400\. You can edit your existing billing alarm and increase the monetary amount that must be exceeded before the alarm is triggered\.

**To edit a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm and choose **Actions**, **Modify**\.

1. For **Whenever my total AWS charges for the month exceed**, specify the new amount that must be exceeded to trigger the alarm and send an email notification\.

1. Choose **Save Changes**\.

## Step 5: Delete a billing alarm<a name="gs_deleting_billing_alarm"></a>

If you no longer need your billing alarm, you can delete it\.

**To delete a billing alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, change the Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and reflects worldwide charges\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box next to the alarm and choose **Actions**, **Delete**\.

1. When prompted for confirmation, choose **Yes, Delete**\.