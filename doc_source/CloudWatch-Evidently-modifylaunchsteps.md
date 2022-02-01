# Modify a launch's future steps<a name="CloudWatch-Evidently-modifylaunchsteps"></a>

You can modify the configuration of launch steps that haven't happened yet, and also add more steps to a launch\.

**To modify the steps for a launch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the name of the project that contains the launch\.

1. Choose the **Launches** tab\.

1. Choose the name of the launch\.

   Choose **Modify launch traffic**\.

1. Choose **Schedule launch**\.

1. For any steps that have not started yet, you can modify the percentage of the available audience to use in the experiment\. You can also modify how their traffic is allocated among the variations\.

   You can add more steps to the launch by choosing **Add another step**\. A launch can have a maximum of five steps\.

1. Choose **Modify**\.