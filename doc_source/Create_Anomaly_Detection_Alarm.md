# Create a CloudWatch alarm based on anomaly detection<a name="Create_Anomaly_Detection_Alarm"></a>

You can create an alarm based on CloudWatch anomaly detection, which analyzes past metric data and creates a model of expected values\. The expected values take into account the typical hourly, daily, and weekly patterns in the metric\.

You set a value for the anomaly detection threshold, and CloudWatch uses this threshold with the model to determine the "normal" range of values for the metric\. A higher value for the threshold produces a thicker band of "normal" values\.

You can choose whether the alarm is triggered when the metric value is above the band of expected values, below the band, or either above or below the band\.

You also can create anomaly detection alarms on single metrics and the outputs of metric math expressions\. You can use these expressions to create graphs that visualize anomaly detection bands\.

Cross\-account or cross\-Region alarms based on anomaly detection are not supported\.

For more information, see [Using CloudWatch anomaly detection](CloudWatch_Anomaly_Detection.md)\.

**Note**  
If you're already using anomaly detection for visualization purposes on a metric in the Metrics console and you create an anomaly detection alarm on that same metric, then the threshold that you set for the alarm doesn't change the threshold that you already set for visualization\. For more information, see [Creating a graph](graph_a_metric.md#create-metric-graph)\.

**To create an alarm that's based on anomaly detection**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Alarms**, and then choose **All alarms**\. 

1.  Choose **Create alarm**\. 

1. Do one of the following:
   +  Choose the service namespace that contains your metric, and then continue choosing options as they appear to narrow down your options\. When a list of metrics appears, select the check box that's next to your metric\. 
   +  In the search box, enter the name of a metric, dimension, or resource ID\. Select one of the results, and then continue choosing options as they appear until a list of metrics appears\. Select the check box that's next to your metric\. 

1.  Choose **Graphed metrics**\. 

   1.  \(Optional\) Under the *Statistic* column, choose the dropdown, and then select one of the predefined statistics or percentiles\. You can use the search box in the dropdown to specify a custom percentile, such as **p95\.45**\. 

   1.  \(Optional\) Under the *Period* column, choose the dropdown, and then select one of the predefined evaluation periods\. 
**Note**  
 When CloudWatch evaluates your alarm, it aggragates the period into a single datapoint\. For an anomaly detection alarm, the evaluation period must be one minute or longer\. 

   1.  \(Optional\) Under the *Y axis* column, you can specify whether the Y\-axis legend appears on the left or right side of the graph\. 
**Note**  
 This option can be used only while you're creating your alarm\. 

1.  Choose **Select metric**\. 

1.  Under ***Conditions***, specify the following: 

   1. Choose **Anomaly detection**\.

       If the model for this metric and statistic already exists, CloudWatch displays a preview of the anomaly detection band in the graph at the top of the screen\. After you create your alarm, it can take up to 15 minutes for the actual anomaly detection band to appear in the graph\. Before that, the band that you see is an approximation of the anomaly detection band\. 
**Tip**  
 To see the graph at the top of the screen in a longer time frame, choose **Edit** at the top\-right of the screen\. 

       If the model for this metric and statistic doesn't already exist, CloudWatch generates the anomaly detection band after you finish creating your alarm\. For new models, it can take up to 3 hours for the actual anomaly detection band to appear in your graph\. It can take up to two weeks for the new model to train, so the anomaly detection band shows more accurate expected values\. 

   1. For **Whenever *metric* is**, specify when to trigger the alarm\. For example, when the metric is greater than, lower than, or outside the band \(in either direction\)\.

   1. For **Anomaly detection threshold**, choose the number to use for the anomaly detection threshold\. A higher number creates a thicker band of "normal" values that is more tolerant of metric changes\. A lower number creates a thinner band that will go to `ALARM` state with smaller metric deviations\. The number does not have to be a whole number\.

   1. Choose **Additional configuration**\. For **Datapoints to alarm**, specify how many evaluation periods \(data points\) must be in the `ALARM` state to trigger the alarm\. If the two values here match, you create an alarm that goes to `ALARM` state if that many consecutive periods are breaching\.

      To create an M out of N alarm, specify a number for the first value that is lower than the number for the second value\. For more information, see [Evaluating an alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

   1. For **Missing data treatment**, choose how the alarm behaves when some data points are missing\. For more information, see [Configuring how CloudWatch alarms treat missing data](AlarmThatSendsEmail.md#alarms-and-missing-data)\.

   1. If the alarm uses a percentile as the monitored statistic, a **Percentiles with low samples** box appears\. Use it to choose whether to evaluate or ignore cases with low sample rates\. If you choose **Ignore \(maintain alarm state\)**, the current alarm state is always maintained when the sample size is too low\. For more information, see [Percentile\-based CloudWatch alarms and low data samples](AlarmThatSendsEmail.md#percentiles-with-low-samples)\. 

1.  Choose **Next**\. 

1. Under **Notification**, select an SNS topic to notify when the alarm is in `ALARM` state, `OK` state, or `INSUFFICIENT_DATA` state\.

   To send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\.

   Choose **Remove** if you don't want the alarm to send notifications\.

1. You can set up the alarm to perform EC2 actions when it changes state, or to create a Systems Manager OpsItem or incident when it goes into ALARM state\. To do this, choose the appropriate button and then choose the alarm state and action to perform\.

   For more information about Systems Manager actions, see [ Configuring CloudWatch to create OpsItems from alarms](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter-create-OpsItems-from-CloudWatch-Alarms.html) and [ Incident creation](https://docs.aws.amazon.com/incident-manager/latest/userguide/incident-creation.html)\.
**Note**  
To create an alarm that performs an AWS Systems Manager Incident Manager action, you must have certain permissions\. For more information, see [ Identity\-based policy examples for AWS Systems Manager Incident Manager](https://docs.aws.amazon.com/incident-manager/latest/userguide/security_iam_id-based-policy-examples.html)\.

1.  Choose **Next**\. 

1.  Under ***Name and description***, enter a name and description for your alarm, and choose **Next**\. 
**Tip**  
 The alarm name must contain only UTF\-8 characters, and can't contain ASCII control characters 

1.  Under ***Preview and create***, confirm that your alarm's information and conditions are correct, and choose **Create alarm**\. 

## Modifying an anomaly detection model<a name="Modify_Anomaly_Detection_Model"></a>

After you create an alarm, you can adjust the anomaly detection model\. You can exclude certain time periods from being used in the model creation\. It is critical that you exclude unusual events such as system outages, deployments, and holidays from the training data\. You can also specify whether to adjust the model for Daylight Savings Time changes\.

**To adjust the anomaly detection model for an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **All alarms**\.

1. Choose the name of the alarm\. If necessary, use the search box to find the alarm\.

1. Choose **Analyse**, **In metrics**\.

1. In the **Details** column, choose **ANOMALY\_DETECTION\_BAND**, **Edit anomaly detection model**\.

1. To exclude a time period from being used to produce the model, choose the calendar icon by **End date**\. Then, select or enter the days and times to exclude from training and choose **Apply**\.

1. If the metric is sensitive to Daylight Savings Time changes, select the appropriate time zone in the **Metric timezone** box\.

1. Choose **Update**\.

## Deleting an anomaly detection model<a name="Delete_Anomaly_Detection_Model"></a>

Using anomaly detection for an alarm accrues charges\. As a best practice, if your alarm no longer needs an anomaly dection model, delete the alarm first and the model second\. When anomaly dection alarms are evaluated, any missing anomaly detectors are created on your behalf\. If you delete the model without deleting the alarm, the alarm automatically recreates the model\.

**To delete an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **All Alarms**\.

1. Choose the name of the alarm\.

1. Choose **Actions**, **Delete**\.

**To delete an anomaly detection model that was used for an alarm**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Metrics**, and then choose **All metrics**\. 

1.  Choose **Browse**, and then select the metric that includes the anomaly detection model\. You can search for your metric in the search box or select your metric by choosing through the options\. 
   +  \(Optional\) If you're using the original interface, choose **All metrics**, and then choose the metric that includes the anomaly detection model\. You can search for your metric in the search box or select your metric by choosing through the options\. 

1.  Choose **Graphed metrics**\. 

1.  In the **Graphed metrics** tab, choose the name of the anomaly detection model that you want to remove, and choose **Delete anomaly detection model**\. 
   +  \(Optional\) If you're using the original interface, choose **Edit model**\. You're directed to a new screen\. On the new screen, choose **Delete model**, and then choose **Delete**\. 