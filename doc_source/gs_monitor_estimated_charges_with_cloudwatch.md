# Scenario: Monitor your estimated charges using CloudWatch<a name="gs_monitor_estimated_charges_with_cloudwatch"></a>

In this scenario, you create an Amazon CloudWatch alarm to monitor your estimated charges\. When you enable the monitoring of estimated charges for your AWS account, the estimated charges are calculated and sent several times daily to CloudWatch as metric data\.

Billing metric data is stored in the US East \(N\. Virginia\) Region and reflects worldwide charges\. This data includes the estimated charges for every service in AWS that you use, as well as the estimated overall total of your AWS charges\.

You can choose to receive alerts by email when charges have exceeded a certain threshold\. These alerts are triggered by CloudWatch and messages are sent using Amazon Simple Notification Service \(Amazon SNS\)\.

**Note**  
For information about analyzing CloudWatch charges that you have already been billed for, see [CloudWatch billing and cost](cloudwatch_billing.md)\.

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
+ You must be signed in using root user credentials or as a user who has been given permission to view billing information\.
+ For consolidated billing accounts, billing data for each linked account can be found by logging in as the paying account\. You can view billing data for total estimated charges and estimated charges by service for each linked account, in addition to the consolidated account\.
+ In a consolidated billing account, member linked account metrics are captured only if the payer account enables the **Receive Billing Alerts** preference\. If you change which account is your management/payer account, you must enable the billing alerts in the new management/payer account\.
+ The account must not be part of the Amazon Partner Network \(APN\) because billing metrics are not published to CloudWatch for APN accounts\. For more information, see [AWS Partner Network](https://aws.amazon.com/partners/)\.

**To enable the monitoring of estimated charges**

1. Open the AWS Billing console at [https://console\.aws\.amazon\.com/billing/](https://console.aws.amazon.com/billing/home?#/)\.

1. In the navigation pane, choose **Billing Preferences**\.

1. By **Alert preferences** choose **Edit**\.

1. Choose **Receive CloudWatch Billing Alerts**\.

1. Choose **Save preferences**\.

## Step 2: Create a billing alarm<a name="gs_creating_billing_alarm"></a>

**Important**  
 Before you create a billing alarm, you must set your Region to US East \(N\. Virginia\)\. Billing metric data is stored in this Region and represents worldwide charges\. You also must enable billing alerts for your account or in the management/payer account \(if you are using consolidated billing\)\. For more information, see [Step 1: Enable billing alerts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/gs_monitor_estimated_charges_with_cloudwatch.html#gs_turning_on_billing_metrics)\. 

 In this procedure, you create an alarm that sends a notification when your estimated charges for AWS exceed a defined threshold\. 

**To create a billing alarm using the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Alarms**, and then choose **All alarms**\. 

1.  Choose **Create alarm**\. 

1.  Choose **Select metric**\. In **Browse**, choose **Billing**, and then choose **Total Estimated Charge**\. 
**Note**  
 If you don't see the **Billing**/**Total Estimated Charge** metric, enable billing alerts, and change your Region to US East \(N\. Virginia\)\. For more information, see [Enabling billing alerts](monitor_estimated_charges_with_cloudwatch.md#turning_on_billing_metrics)\. 

1.  Select the box for the **EstimatedCharges** metric, and then choose **Select metric**\. 

1. For **Statistic**, choose **Maximum**\.

1. For **Period**, choose **6 hours**\.

1.  For **Threshold type**, choose **Static**\. 

1.  For **Whenever EstimatedCharges is \. \. \.**, choose **Greater**\. 

1.  For **than \. \. \.**, define the value that you want to cause your alarm to trigger\. For example, **200** USD\. 

   The **EstimatedCharges** metric values are only in US dollars \(USD\), and the currency conversion is provided by Amazon Services LLC\. For more information, see [ What is AWS Billing?](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)\.
**Note**  
 After you define a threshold value, the preview graph displays your estimated charges for the current month\. 

1. Choose **Additional Configuration** and do the following:
   + For **Datapoints to alarm**, specify **1 out of 1**\.
   + For **Missing data treatment**, choose **Treat missing data as missing**\.

1.  Choose **Next**\. 

1.  Under **Notification**, ensure that **In alarm** is selected\. Then specify an Amazon SNS topic to be notified when your alarm is in the `ALARM` state\. The Amazon SNS topic can include your email address so that you recieve email when the billing amount crosses the threshold that you specified\.

   You can select an existing Amazon SNS topic, create a new Amazon SNS topic, or use a topic ARN to notify other account\. If you want your alarm to send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\. 

1.  Choose **Next**\. 

1.  Under **Name and description**, enter a name for your alarm\. 

   1.  \(Optional\) Enter a description of your alarm\. 

1. Choose **Next**\.

1.  Under **Preview and create**, make sure that your configuration is correct, and then choose **Create alarm**\. 

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