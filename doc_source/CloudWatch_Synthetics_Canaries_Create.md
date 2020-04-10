# Creating a Canary<a name="CloudWatch_Synthetics_Canaries_Create"></a>


****  

|  | 
| --- |
| CloudWatch Synthetics is in open preview\. The preview is currently available in the following Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries_Create.html) The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [cw\-synthetics@amazon\.com](mailto:cw-synthetics@amazon.com) | 

You can use a "blueprint" provided by CloudWatch to create your canary, or write your own script\. Blueprints are provided for the following canary types:
+ Heartbeat monitor
+ API Canary
+ Broken link checker
+ GUI Workflow

**Important**  
Ensure that you use Synthetics canaries to monitor only endpoints and APIs where you have ownership or permissions\. Based on canary run frequency settings, these endpoints may experience increased traffic\.

If you are writing your own script, you can use several functions that CloudWatch Synthetics has built into a library\. For more information, see [Canary Runtime Versions](CloudWatch_Synthetics_Canaries_Library.md)\.

**To create a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

   CloudWatch displays a screen with details about canaries that you have already created\.

1. Choose **Create Canary**\.

1. On the next page, choose one of the following:
   + To base your canary on a blueprint script, choose **Use a blueprint**\. Then choose the type of canary you want to create\.
   + To upload your own Node\.js script to create a custom canary, choose **Upload a script**\.

     You can then drag your script into the **Script** area, or choose **Browse files** to navigate to the script in your file system\.
   + To import your script from an Amazon S3 bucket, choose **Import from S3**\. Then, under **Source location**, type the complete path to your canary or choose **Browse S3**\.

     You must have `s3:GetObject` and `s3:GetObjectVersion` permissions for the S3 bucket that you use\. The bucket must be in the same Region where you are creating the canary\.

1. Under **Name**, enter a name for your canary\. The name is used on many pages, so we recommend that you give it a descriptive name that distinguishes it from other canaries that you create\.

1. Under **Application or endpoint URL**, enter the URL that you want the canary to test\. This URL must include the protocol \(such as **https://**\)\.

   If you want the canary to test an endpoint on a VPC, you must also enter information about your VPC later in this procedure, under **VPC settings**\. 

1. If you are using your own script for the canary, then under **Lambda handler**, enter the entry point where you want the canary to start\. The string that you type must end with `.handler`\.

1. Under **Schedule**, choose whether to run this canary once or regularly on a schedule\.

1. Under **Data retention**, specify how long to retain information about both failed and successful canary runs\. The range is 1\-455 days\.

1. Under **Data Storage**, select the S3 bucket to use to store the data from the canary test runs\. If you leave this blank, a default S3 bucket will be used or created\.

1. Under **Access permissions**, choose whether to create a new IAM role to run the canary, or use an existing one\.

   You can't re\-use an IAM role that was created by the console for a previous canary, because that role is specific to that previous canary name\. You can only use an existing role if you have manually created a role that works for multiple canaries\.

   To use an existing role, you must have the `iam:PassRole` permission to pass that role to Synthetics and Lambda, and you must have the `iam:GetRole` permission as well\.

1. Under **Alarms**, choose whether you want CloudWatch alarms to be created for this canary\. Alarm settings are the same for all canaries that you enable alarms for\.

1. To have this canary test an endpoint that is on a VPC, choose **VPC settings** and do the following:

   1. Select the VPC that hosts the endpoint\.

   1. Select one or more subnets on your VPC\. You must select a private subnet, because Lambda instance can't be configured to run in a public subnet when an IP address cannot be assigned to the Lambda instance during execution\. For more information, see [Configuring a Lambda Function to Access Resources in a VPC](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html)\.

   1. Select one or more security groups on your VPC\.

   If the endpoint is on a VPC, you must enable your canary to send information to CloudWatch and Amazon S3\. For more information, see [Running a Canary on a VPC](CloudWatch_Synthetics_Canaries_VPC.md)\.

1. Under **Tags**, optionally add one or more key/value pairs as tags for this canary\. Tags can help you identify and organize your AWS resources and track your AWS costs\. For more information, see [Tagging Your Amazon CloudWatch Resources](CloudWatch-Tagging.md)\.

## Resources That Are Created for Canaries<a name="CloudWatch_Synthetics_Canaries_Resources_Created"></a>

When you create a canary, the following resources are created for the canary:
+ An IAM role with the name `CloudWatchSyntheticsRole-canary-name-uuid` \(if you use the AWS Management Console to create the canary and specify for a new role to be created for the canary\)
+ An IAM policy with the name `CloudWatchSyntheticsPolicy-canary-name-uuid`
+ An Amazon S3 bucket with the name `cw-syn-results-accountID-region`
+ Alarms with the name `Synthetics-Alarm-MyCanaryName` \(If you specify for alarms to be created for the canary\)
+ Lambda functions and layers, if you use a blueprint to create the canary\. These have the prefix `cwsyn-MyCanaryName`\.
+ CloudWatch Logs log groups with the following names: `/aws/lambda/cwsyn-MyCanaryName`