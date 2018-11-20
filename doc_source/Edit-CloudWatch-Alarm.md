# Edit a CloudWatch Alarm<a name="Edit-CloudWatch-Alarm"></a>

You can edit an existing alarm, and update the Amazon SNS email notification list it uses\.

**To edit an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**\.

1. Select the alarm and choose **Actions**, **Modify**\.

1. On the **Modify Alarm** page, update the alarm as necessary and choose **Save Changes**\. To modify the expression, metrics, period, or statistic, first choose **Edit** near the top of the screen\.

   You cannot change the name of an existing alarm\. You can change the description\. Or you can copy the alarm, and give the new alarm a different name\. To copy an alarm, select the alarm and then choose **Actions**, **Copy**\.

**To update an email notification list that was created using the Amazon SNS console**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

1. In the navigation pane, choose **Topics**, and then select the ARN for your notification list \(topic\)\.

1. Do one of the following:
   + To add an email address, choose **Create subscription**\. For **Protocol**, choose **Email**\. For **Endpoint**, type the email address of the new recipient\. Choose **Create subscription**\.
   + To remove an email address, choose the **Subscription ID**\. Choose **Other subscription actions**, **Delete subscriptions**\.

1. Choose **Publish to topic**\.