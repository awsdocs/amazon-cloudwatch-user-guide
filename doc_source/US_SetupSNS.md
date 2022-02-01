# Setting up Amazon SNS notifications<a name="US_SetupSNS"></a>

Amazon CloudWatch uses Amazon SNS to send email\. First, create and subscribe to an SNS topic\. When you create a CloudWatch alarm, you can add this SNS topic to send an email notification when the alarm changes state\. For more information, see the [Amazon Simple Notification Service Getting Started Guide](https://docs.aws.amazon.com/sns/latest/gsg/)\.

Alternatively, if you plan to create your CloudWatch alarm using the AWS Management Console, you can skip this procedure because you can create the topic when you create the alarm\.

**Note**  
When you create an Amazon SNS topic, you choose to make it a *standard topic* or a *FIFO topic*\. CloudWatch guarantees the publication of all alarm notifications to both types of topics\. However, even if you use a FIFO topic, in rare cases CloudWatch sends the notifications to the topic out of order\. If you use a FIFO topic, the alarm sets the message group ID of the alarm notifications to be a hash of the ARN of the alarm\.

## Setting up an Amazon SNS topic using the AWS Management Console<a name="set-up-sns-topic-console"></a>

First, create a topic, then subscribe to it\. You can optionally publish a test message to the topic\.

**To create an SNS topic**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. On the Amazon SNS dashboard, under **Common actions**, choose **Create Topic**\. 

1. In the **Create new topic** dialog box, for **Topic name**, enter a name for the topic \(for example, **my\-topic**\)\.

1. Choose **Create topic**\.

1. Copy the **Topic ARN** for the next task \(for example, arn:aws:sns:us\-east\-1:111122223333:my\-topic\)\.

**To subscribe to an SNS topic**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Subscriptions**, **Create subscription**\.

1. In the **Create subscription** dialog box, for **Topic ARN**, paste the topic ARN that you created in the previous task\.

1. For **Protocol**, choose **Email**\.

1. For **Endpoint**, enter an email address that you can use to receive the notification, and then choose **Create subscription**\.

1. From your email application, open the message from AWS Notifications and confirm your subscription\.

   Your web browser displays a confirmation response from Amazon SNS\.

**To publish a test message to an SNS topic**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics**\.

1. On the **Topics** page, select a topic and choose **Publish to topic**\.

1. In the **Publish a message** page, for **Subject**, enter a subject line for your message, and for **Message**, enter a brief message\.

1. Choose **Publish Message**\.

1. Check your email to confirm that you received the message\.

## Setting up an SNS topic using the AWS CLI<a name="set-up-sns-topic-cli"></a>

First, you create an SNS topic, and then you publish a message directly to the topic to test that you have properly configured it\.

**To set up an SNS topic**

1. Create the topic using the [create\-topic](https://docs.aws.amazon.com/cli/latest/reference/sns/create-topic.html) command as follows\.

   ```
   1. aws sns create-topic --name my-topic
   ```

   Amazon SNS returns a topic ARN with the following format:

   ```
   1. {
   2.     "TopicArn": "arn:aws:sns:us-east-1:111122223333:my-topic"
   3. }
   ```

1. Subscribe your email address to the topic using the [subscribe](https://docs.aws.amazon.com/cli/latest/reference/sns/subscribe.html) command\. If the subscription request succeeds, you receive a confirmation email message\.

   ```
   1. aws sns subscribe --topic-arn arn:aws:sns:us-east-1:111122223333:my-topic --protocol email --notification-endpoint my-email-address
   ```

   Amazon SNS returns the following:

   ```
   1. {
   2.     "SubscriptionArn": "pending confirmation"
   3. }
   ```

1. From your email application, open the message from AWS Notifications and confirm your subscription\.

   Your web browser displays a confirmation response from Amazon Simple Notification Service\.

1. Check the subscription using the [list\-subscriptions\-by\-topic](https://docs.aws.amazon.com/cli/latest/reference/sns/list-subscriptions-by-topic.html) command\.

   ```
   1. aws sns list-subscriptions-by-topic --topic-arn arn:aws:sns:us-east-1:111122223333:my-topic
   ```

   Amazon SNS returns the following:

   ```
    1. {
    2.   "Subscriptions": [
    3.     {
    4.         "Owner": "111122223333",
    5.         "Endpoint": "me@mycompany.com",
    6.         "Protocol": "email",
    7.         "TopicArn": "arn:aws:sns:us-east-1:111122223333:my-topic",
    8.         "SubscriptionArn": "arn:aws:sns:us-east-1:111122223333:my-topic:64886986-bf10-48fb-a2f1-dab033aa67a3"
    9.     }
   10.   ]
   11. }
   ```

1. \(Optional\) Publish a test message to the topic using the [publish](https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html) command\.

   ```
   1. aws sns publish --message "Verification" --topic arn:aws:sns:us-east-1:111122223333:my-topic
   ```

   Amazon SNS returns the following\.

   ```
   1. {
   2.     "MessageId": "42f189a0-3094-5cf6-8fd7-c2dde61a4d7d"
   3. }
   ```

1. Check your email to confirm that you received the message\.