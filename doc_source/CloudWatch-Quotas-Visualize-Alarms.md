# Visualizing Your Service Quotas and Setting Alarms<a name="CloudWatch-Quotas-Visualize-Alarms"></a>

You can use the CloudWatch console to visualize your service quotas, and see how your current usage compares\. You can also set alarms to be notified when you approach a quota\.

**To visualize a service quota and optionally set an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. On the **All metrics** tab, choose **Usage**, then choose **By AWS Resource**\.

   The list of service quota usage metrics appears\.

1. Select the check box next to one of the metrics\.

   The graph displays your current usage of that AWS resource\.

1. To add your service quota to the graph, do the following:

   1. Choose the **Graphed metrics** tab\.

   1. Choose **Math expression**, **Start with an empty expression**\. Then in the new row, under **Details**, enter **SERVICE\_QUOTA\(m1\)**\.

      A new line is added to the graph, displaying the service quota for the resource represented in the metric\.

1. To see your current usage as a percentage of the quota, add a new expression or change the current **SERVICE\_QUOTA** expression\. For the new expression, use **m1/SERVICE\_QUOTA\(m1\)\*100**

1. \(Optional\) To set an alarm that notifies you if you approach the service quota, do the following:

   1. On the **m1/SERVICE\_QUOTA\(m1\)\*100** row, under **Actions**, choose the alarm icon\. It looks like a bell\.

      The alarm creation page appears\.

   1. Under **Conditions**, ensure that **Threshold type** is **Static** and **Whenever Expression1 is** is set to **Greater**\. Under **than**, enter **80**\. This creates an alarm that goes into ALARM state when your usage exceeds 80 percent of the quota\.

   1. Choose **Next**\.

   1. On the next page, select an Amazon SNS topic or create a new one\. This topic is notified when the alarm goes to ALARM state\. Then choose **Next**\.

   1. On the next page, enter a name and description for the alarm, and then choose **Next**\.

   1. Choose **Create alarm**\.