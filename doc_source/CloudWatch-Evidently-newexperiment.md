# Create an experiment<a name="CloudWatch-Evidently-newexperiment"></a>

Use experiments to test different versions of a feature or website and collect data from real user sessions\. This way, you can make choices for your application based on evidence and data\.

Before you can add an experiment, you must have created a project\. For more information, see [Create a new project](CloudWatch-Evidently-newproject.md)\.

When you add an experiment, you can use a feature that you have already created, or create a new feature while you create the experiment\.

**To add an experiment to a project**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Select the button next to the name of the project and choose **Project actions**, **Create experiment**\.

1. For **Experiment name**, enter a name to be used to identify this feature within this project\.

   You can optionally add a description\.

1. Choose either **Select from existing features** or **Add new feature**\.

   If you are using an existing feature, select it under **Feature name**\.

   If you choose **Add new feature**, do the following:

   1. For **Feature name**, enter a name to be used to identify this feature within this project\. You can also optionally enter a description\.

   1. For **Feature variations**, for **Variation type** choose **Boolean**, **Long**, **Double**, or **String**\. The type defines which type of value is used for each variation\. For more information, see [Variation types](CloudWatch-Evidently-newfeature.md#CloudWatch-Evidently-variationtypes)\.

   1. Add up to five variations for your feature\. The **Value** for each variation must be valid for the **Variation type** that you selected\.

      Specify one of the variations to be the default\. This is the baseline that the other variations will be compared to, and should be the variation that is being served to your users now\. If you stop an experiment that uses this feature, the default variation is then served to the percentage of users that were in the experiment previously\.

   1. Choose **Sample code**\. The code example shows what you need to add to your application to set up the variations and assign user sessions to them\. You can choose between JavaScript and Java for the code\.

      You don't need to add the code to your application right now, but you must do so before you start the experiment\. For more information, see [Adding code to your application](CloudWatch-Evidently-code-application.md)\.

1. For **Audience**, first specify the percentage of the available users whose sessions will be used in the experiment\. Then allocate the traffic for the different variations that the experiment uses\.

   If a launch and an experiment are both running at the same time for the same feature, the audience is first directed to the launch\. Then, the percentage of traffic specified for the launch is taken from the overall audience\. After that, the percentage that you specify here is the percentage of the remaining audience that is used for the experiment\. Any remaining traffic after that is served the default variation\.

1. For **Metrics**, choose the metrics to use to evaluate the variations during the experiment\. You must use at least one metric for evaluation\.

   1. For **Metric source**, choose whether to use CloudWatch RUM metrics or custom metrics\.

   1. Enter a name for the metric\. For **Goal**, choose **Increase** if you want a higher value for the metric to indicate a better variation\. Choose **Decrease** if you want a lower value for the metric to indicate a better variation\.

   1. If you are using a custom metric, you can create the metric here using an Amazon EventBridge rule\. To create a custom metric, do the following:
      + Under **Metric rule**, for **Entity ID**, enter a way to identify the entity, This can be a user or session that does an action that causes a metric value to be recorded\. An example is `userDetails.userID`\.
      + For **Value key**, enter the value that is to be tracked to produce the metric\.
      + Optionally, enter a name for the units for the metric\. This unit name is for display purposes only, for use on graphs in the Evidently console\.

      You can use RUM metrics only if you have set up RUM to monitor this application\. For more information, see [Use CloudWatch RUM](CloudWatch-RUM.md)\.
**Note**  
If you use RUM metrics, and the app monitor is not configured to sample 100% of user sessions, then not all of the user sessions in the experiment will send metrics to Evidently\. To ensure that the experiment metrics are accurate, we recommend that the app monitor uses 100% of user sessions for sampling\.

   1. \(Optional\) To add more metrics to evaluate, choose **Add metric**\. You can evaluate as many as three metrics during the experiment\.

1. \(Optional\) To create CloudWatch alarms to use with this experiment, choose **CloudWatch alarms**\. The alarms can monitor whether the difference in results between each variation and the default variation is larger than a threshold that you specify\. If a variation's performance is worse than the default variation, and the difference is greater than your threshold, it goes into alarm state and notifies you\.

   Creating an alarm here creates one alarm for each variation that is not the default variation\.

   If you create an alarm, specify the following:
   + For **Metric name**, choose the experiment metric to use for the alarm\.
   + For **Alarm condition** choose what condition causes the alarm to go into alarm state, when the variation metric values are compared to the default variation metric values\. For example, choose **Greater** or **Greater/Equal** if higher numbers indicate for the variation indicates that it is performing poorly\. This would be appropriate if the metric is measuring page load time, for example\.
   + Enter a number for the threshold, which is the percentage difference in performance that will cause the alarm to go into `ALARM` state\.
   + For **Average over period**, choose how much metric data for each variation is aggregated together before being compared\.

   You can choose **Add new alarm** again to add more alarms to the experiment\.

   Next, choose **Set notifications for the alarm** and select or create an Amazon Simple Notification Service topic to send alarm notifications to\. For more information, see [Setting up Amazon SNS notifications](US_SetupSNS.md),

1. \(Optional\) To add tags to this experiment, choose **Tags**, **Add new tag**\.

   Then, for **Key**, enter a name for the tag\. You can add an optional value for the tag in **Value**\. 

   To add another tag, choose **Add new tag** again\.

   For more information, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

1. Choose **Create experiment**\.

1. If you haven't already, build the feature variants into your application\.

1. Choose **Done**\. The experiment does not start until you start it\.

After you complete the steps in the following procedure, the experiment starts immediately\.

**To start an experiment that you have created**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the name of the project\.

1. Choose the **Experiments** tab\.

1. Choose the button next to the name of the experiment, and choose **Actions**, **Start experiment**\.

1. \(Optional\) To view or modify the experiment settings you made when you created it, choose **Experiment setup**\.

1. Choose a time for the experiment to end\.

1. Choose **Start experiment**\.

   The experiment starts immediately\.