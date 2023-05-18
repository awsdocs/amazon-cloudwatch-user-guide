# Create a launch<a name="CloudWatch-Evidently-newlaunch"></a>

To expose a new feature or change to a specified percentage of your users, create a launch\. You can then monitor key metrics such as page load times and conversions before you roll out the feature to all of your users\.

Before you can add a launch, you must have created a project\. For more information, see [Create a new project](CloudWatch-Evidently-newproject.md)\.

When you add a launch, you can use a feature that you have already created, or create a new feature while you create the launch\.

**To add a launch to a project**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Select the button next to the name of the project and choose **Project actions**, **Create launch**\.

1. For **Launch name**, enter a name to be used to identify this feature within this project\.

   You can optionally add a description\.

1. Choose either **Select from existing features** or **Add new feature**\.

   If you are using an existing feature, select it under **Feature name**\.

   If you choose **Add new feature**, do the following:

   1. For **Feature name**, enter a name to be used to identify this feature within this project\. You can optionally add a description\.

   1. For **Feature variations**, for **Variation type** choose **Boolean**, **Long**, **Double**, or **String**\. For more information, see [Variation types](CloudWatch-Evidently-newfeature.md#CloudWatch-Evidently-variationtypes)\.

   1. Add up to five variations for your feature\. The **Value** for each variation must be valid for the **Variation type** that you selected\.

      Specify one of the variations to be the default\. This is the baseline that the other variations will be compared to, and should be the variation that is being served to your users now\. If you stop an experiment, this default variation will then be served to all users\.

   1. Choose **Sample code**\. The code example shows what you need to add to your application to set up the variations and assign user sessions to them\. You can choose between JavaScript, Java, and Python for the code\.

      You don't need to add the code to your application right now, but you must do so before you start the launch\.

      For more information, see [Adding code to your application](CloudWatch-Evidently-code-application.md)\.

1. For **Launch configuration**, choose whether to start the launch immediately or schedule it to start later\.

1. \(Optional\) To specify different traffic splits for audience segments that you have defined, instead of the traffic split that you will use for your general audience, choose **Add Segment Overrides**\.

   In **Segment Overrides**, select a segment and define the traffic split to use for that segment\.

   You can optionally define more segments to define traffic splits for by choosing **Add Segment Override**\. A launch can have up to six segment overrides\.

   For more information, see [Use segments to focus your audience](CloudWatch-Evidently-segments.md)\.

1. For **Traffic configuration**, select the traffic percentage to assign to each variation for the general audience that doesn't match the segment overrides\. You can also choose to exclude variations from being served to users\.

   The **Traffic summary** shows how much of your overall traffic is available for this launch\.

1. If you choose to schedule the launch to start later, you can add multiple steps to the launch\. Each step can use different percentages for serving the variations\. To do this, choose **Add another step** and then specify the schedule and traffic percentages for the next step\. You can include as many as five steps in a launch\.

1. If you want to track your feature performance with metrics during the launch, choose **Metrics**, **Add metric**\. You can use either CloudWatch RUM metrics or custom metrics\.

   To use a custom metric, you can create the metric here using an Amazon EventBridge rule\. To create a custom metric, do the following:
   + Choose **Custom metrics** and enter a name for the metric\.
   + Under **Metric rule**, for **Entity ID**, enter the way to identify the entity\. This can be a user or session that does an action that causes a metric value to be recorded\. An example is `userDetails.userID`\.
   + For **Value key**, enter the value that is to be tracked to produce the metric\.
   + Optionally, enter a name for the units for the metric\. This unit name is for display purposes only, for use on graphs in the Evidently console\.

   As you enter those fields, the box shows examples of how to code the EventBridge rule to create the metric\. For more information about EventBridge, see [ What Is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)

   To use RUM metrics, you must already have a RUM app monitor set up for your application\. For more information, see [Set up an application to use CloudWatch RUM](CloudWatch-RUM-get-started.md)\.
**Note**  
If you use RUM metrics, and the app monitor is not configured to sample 100% of user sessions, then not all of the user sessions that participate in the launch will send metrics to Evidently\. To ensure that the launch metrics are accurate, we recommend that the app monitor uses 100% of user sessions for sampling\.

1. \(Optional\) If you create at least one metric for the launch, you can associate an existing CloudWatch alarm with this launch\. To do so, choose **Associate CloudWatch alarms**\.

   When you associate an alarm with a launch, CloudWatch Evidently must add tags to the alarm with the project name and launch name\. This is so that CloudWatch Evidently can display the correct alarms in the launch information in the console\.

   To acknowledge that CloudWatch Evidently will add these tags, choose **Allow Evidently to tag the alarm resource identified below with this launch resource\.** Then, choose **Associate alarm** and enter the alarm name\.

   For information about creating CloudWatch alarms, see [ Using Amazon CloudWatch alarms](AlarmThatSendsEmail.md)\.

1. \(Optional\) To add tags to this launch, choose **Tags**, **Add new tag**\.

   Then, for **Key**, enter a name for the tag\. You can add an optional value for the tag in **Value**\. 

   To add another tag, choose **Add new tag** again\.

   For more information, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

1. Choose **Create launch**\.