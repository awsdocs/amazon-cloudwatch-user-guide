# Deleting a Canary<a name="synthetics_canaries_deletion"></a>


****  

|  | 
| --- |
| CloudWatch Synthetics is in open preview\. The preview is currently available in the following Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/synthetics_canaries_deletion.html) The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [cw\-synthetics@amazon\.com](mailto:cw-synthetics@amazon.com) | 

When you delete a canary, resources used and created by the canary are not automatically deleted\. After you delete a canary that you do not intend to use again, you should also delete the following:
+ The Lambda functions and layers used by this canary\. These have the prefix `cwsyn-MyCanaryName`\.
+ The CloudWatch alarms created for this canary\. These alarms have a name of `Synthetics-Alarm-MyCanaryName`\.
+ Amazon S3 objects and buckets, such as the canary's results location and artifact location
+ IAM roles created for the canary\. These have the name ` role/service-role/CloudWatchSyntheticsRole-MyCanaryName `
+ CloudWatch Logs log groups created for the canary\. These logs groups have the following names: `/aws/lambda/cwsyn-MyCanaryName` 

Before you delete a canary, you may want to view the canary details and make note of this information for that canary, so that you can delete the correct resources after deleting the canary\.