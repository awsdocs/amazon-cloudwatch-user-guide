# Editing or Deleting a CloudWatch Alarm<a name="Edit-CloudWatch-Alarm"></a>

You can edit or delete an existing alarm\.

CloudWatch has updated the alarm user interface\. By default, you're shown the new user interface, but you can choose to return to the old user interface\. This topic contains steps for each\.

**To edit an alarm using the new alarm user interface**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. Choose the name of the alarm\.

1. Choose **Edit**\.

   The **Specify metric and conditions** page appears, showing a graph and other information about the metric and statistic that you selected\.

1. To change the metric, choose **Edit**, choose the **All metrics** tab, and do one of the following:
   + Choose the service namespace that contains the metric that you want\. Continue choosing options as they appear to narrow the choices\. When a list of metrics appears, select the check box next to the metric that you want\.
   + In the search box, enter the name of a metric, dimension, or resource ID and press Enter\. Then choose one of the results and continue until a list of metrics appears\. Select the check box next to the metric that you want\. 

   Choose **Select metric**\.

1. To change other aspects of the alarm, choose the appropriate options\. To change how many data points must be breaching for the alarm to go into `ALARM` state or to change how missing data is treated, choose **Additional configuration**\.

1. Choose **Next**\.

1. Under **Notification**, **Auto Scaling action**, and **EC2 action**, optionally edit the actions taken when the alarm is triggered\. Then choose **Next**\.

1. Optionally change the alarm description\.

   You can't change the name of an existing alarm\. You can copy an alarm and give the new alarm a different name\. To copy an alarm, select the check box next to the alarm name in the alarm list and choose **Action**, **Copy**\.

1. Choose **Next**\.

1. Under **Preview and create**, confirm that the information and conditions are what you want, then choose **Update alarm**\.

**To edit an alarm using the older alarms user interface**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. Select the alarm and choose **Actions**, **Modify**\.

1. On the **Modify Alarm** page, update the alarm as necessary and choose **Save Changes**\. To modify the expression, metrics, period, or statistic, first choose **Edit** near the top of the screen\.

   You can't change the name of an existing alarm\. You can change the description\. Or you can copy the alarm and give the new alarm a different name\. To copy an alarm, select the alarm and then choose **Actions**, **Copy**\.

**To update an email notification list that was created using the Amazon SNS console**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics** and then select the ARN for your notification list \(topic\)\.

1. Do one of the following:
   + To add an email address, choose **Create subscription**\. For **Protocol**, choose **Email**\. For **Endpoint**, enter the email address of the new recipient\. Choose **Create subscription**\.
   + To remove an email address, choose the **Subscription ID**\. Choose **Other subscription actions**, **Delete subscriptions**\.

1. Choose **Publish to topic**\.

**To delete an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. Select the check box to the left of the name of the alarm, and choose **Actions**, **Delete**\.

1. Choose **Delete**\.