# Editing or deleting a canary<a name="synthetics_canaries_deletion"></a>

You can edit or delete an existing canary\.

**Edit canary**

When you edit a canary, even if you don't change its schedule, the schedule is reset corresponding to when you edit the canary\. For example, if you have a canary that runs every hour, and you edit that canary, the canary will run immediately after the edit is completed and then every hour after that\.

**To edit or update a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

1. Select the button next to the canary name, and choose **Actions**, **Edit**\.

1. \(Optional\) If this canary performs visual monitoring of screenshots and you want to set the next run of the canary as the baseline, select **Set next run as new baseline**\.

1. \(Optional\) If this canary performs visual monitoring of screenshots and you want to remove a screenshot from visual monitoring or you want to designate parts of the screenshot to be ignored during visual comparisons, under **Visual Monitoring** choose **Edit Baseline**\.

   The screenshot appears, and you can do one of the following:
   + To remove the screenshot from being used for visual monitoring, select **Remove screenshot from visual test baseline**\.
   + To designate parts of the screenshot to be ignored during visual comparisons, click and drag to draw areas of the screen to ignore\. Once you have done this for all the areas that you want to ignore during comparisons, choose **Save**\.

1. Make any other changes to the canary that you'd like, and choose **Save**\.

**Delete canary**

When you delete a canary, resources used and created by the canary are not automatically deleted\. After you delete a canary that you do not intend to use again, you should also delete the following:
+ Lambda functions and layers used by this canary\. Their prefix is `cwsyn-MyCanaryName`\.
+ CloudWatch alarms created for this canary\. These alarms have a name that starts with `Synthetics-Alarm-MyCanaryName`\. For more information about deleting alarms, see [Editing or deleting a CloudWatch alarm](Edit-CloudWatch-Alarm.md)\.
+ Amazon S3 objects and buckets, such as the canary's results location and artifact location\.
+ IAM roles created for the canary\. These have the name `role/service-role/CloudWatchSyntheticsRole-MyCanaryName`\. 
+ Log groups in CloudWatch Logs created for the canary\. These logs groups have the following names: `/aws/lambda/cwsyn-MyCanaryName`\. 

Before you delete a canary, you might want to view the canary details and make note of this information\. That way, you can delete the correct resources after you delete the canary\.

**To delete a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

1. Select the button next to the canary name, and choose **Actions**, **Delete**\.

1. Enter **Delete** into the box and choose **Delete**\.

1. Delete the other resources used by and created for the canary, as listed earlier in this section\.