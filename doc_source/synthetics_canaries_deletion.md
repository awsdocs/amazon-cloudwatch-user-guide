# Editing or Deleting a Canary<a name="synthetics_canaries_deletion"></a>

You can edit or delete an existing canary\.

**Edit canary**

When you edit a canary, even if you don't change its schedule, the schedule is reset corresponding to when you edit the canary\. For example, if you have a canary that runs every hour, and you edit that canary, the canary will run immediately after the edit is completed and then every hour after that\.

**To edit or update a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

1. Select the button next to the canary name, and choose **Actions**, **Edit**\.

1. Make your changes and choose **Save**\.

**Delete canary**

When you delete a canary, resources used and created by the canary are not automatically deleted\. After you delete a canary that you do not intend to use again, you should also delete the following:
+ Lambda functions and layers used by this canary\. Their prefix is `cwsyn-MyCanaryName`\.
+ CloudWatch alarms created for this canary\. These alarms have a name that starts with `Synthetics-Alarm-MyCanaryName`\. For more information about deleting alarms, see [Editing or Deleting a CloudWatch Alarm](Edit-CloudWatch-Alarm.md)\.
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