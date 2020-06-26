# Deleting a Canary<a name="synthetics_canaries_deletion"></a>

When you delete a canary, resources used and created by the canary are not automatically deleted\. After you delete a canary that you do not intend to use again, you should also delete the following:
+ Lambda functions and layers used by this canary\. Their prefix is `cwsyn-MyCanaryName`\.
+ CloudWatch alarms created for this canary\. These alarms have a name of `Synthetics-Alarm-MyCanaryName`\. For more information about deleting alarms, see [Editing or Deleting a CloudWatch Alarm](Edit-CloudWatch-Alarm.md)\.
+ Amazon S3 objects and buckets, such as the canary's results location and artifact location\.
+ IAM roles created for the canary\. These have the name `role/service-role/CloudWatchSyntheticsRole-MyCanaryName`\. 
+ Log groups in CloudWatch Logs created for the canary\. These logs groups have the following names: `/aws/lambda/cwsyn-MyCanaryName`\. 

Before you delete a canary, you might want to view the canary details and make note of this information\. That way, you can delete the correct resources after you delete the canary\.