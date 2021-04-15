# Creating a canary<a name="CloudWatch_Synthetics_Canaries_Create"></a>

**Important**  
Ensure that you use Synthetics canaries to monitor only endpoints and APIs where you have ownership or permissions\. Depending on the canary frequency settings, these endpoints might experience increased traffic\.

When you use the CloudWatch console to create a canary, you can use a blueprint provided by CloudWatch to create your canary or you can write your own script\. For more information, see [Using canary blueprints](CloudWatch_Synthetics_Canaries_Blueprints.md)\.

You can also create a canary using AWS CloudFormation if you are using your own script for the canary\. For more information, see [AWS::Synthetics::Canary ](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-synthetics-canary.html) in the *AWS CloudFormation User Guide*\.

If you are writing your own script, you can use several functions that CloudWatch Synthetics has built into a library\. For more information, see [Synthetics runtime versions](CloudWatch_Synthetics_Canaries_Library.md)\.

**To create a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

   

1. Choose **Create Canary**\.

1. Choose one of the following:
   + To base your canary on a blueprint script, choose **Use a blueprint**, and then choose the type of canary you want to create\. For more information about what each type of blueprint does, see [Using canary blueprints](CloudWatch_Synthetics_Canaries_Blueprints.md)\.
   + To upload your own Node\.js script to create a custom canary, choose **Upload a script**\.

     You can then drag your script into the **Script** area or choose **Browse files** to navigate to the script in your file system\.
   + To import your script from an S3 bucket, choose **Import from S3**\. Under **Source location**, enter the complete path to your canary or choose **Browse S3**\.

     You must have `s3:GetObject` and `s3:GetObjectVersion` permissions for the S3 bucket that you use\. The bucket must be in the same AWS Region where you are creating the canary\.

1. Under **Name**, enter a name for your canary\. The name is used on many pages, so we recommend that you give it a descriptive name that distinguishes it from other canaries\.

1. Under **Application or endpoint URL**, enter the URL that you want the canary to test\. This URL must include the protocol \(such as https://\)\.

   If you want the canary to test an endpoint on a VPC, you must also enter information about your VPC later in this procedure\. 

1. If you are using your own script for the canary, under **Lambda handler**, enter the entry point where you want the canary to start\. The string that you enter must end with `.handler`\.

1. If you are using environment variables in your script, choose **Environment variables** and then specify a value for each environment variable defined in your script\. For more information, see [Environment variables](CloudWatch_Synthetics_Canaries_WritingCanary_Nodejs.md#CloudWatch_Synthetics_Environment_Variables)\.

1. Under **Schedule**, choose whether to run this canary just once or run it regularly on a schedule\. When you use the CloudWatch console to create a canary, the options for canary frequency are once per minute, once per 5 minutes, and once per hour\. If you use the AWS CLI or APIs to create a canary, you can choose other frequency options\. 

1. Under **Data retention**, specify how long to retain information about both failed and successful canary runs\. The range is 1\-455 days\.

1. Under **Data Storage**, select the S3 bucket to use to store the data from the canary runs\. The bucket name can't contain a period \(\.\)\. If you leave this blank, a default S3 bucket is used or created\.

   If you are using the `syn-nodejs-puppeteer-3.0` or later runtime, when you enter the URL for the bucket in the text box, you can specify a bucket in the current Region or in a different Region\. If you are using an earlier runtime version, the bucket must be in the current Region\.

1. Under **Access permissions**, choose whether to create an IAM role to run the canary or use an existing one\.

   If you use the CloudWatch console to create a role for a canary when you create the canary, you can't re\-use the role for other canaries, because these roles are specific to just one canary\. If you have manually created a role that works for multiple canaries, you can use that existing role\.

   To use an existing role, you must have the `iam:PassRole` permission to pass that role to Synthetics and Lambda\. You must also have the `iam:GetRole` permission\.

1. \(Optional\) Under **Alarms**, choose whether you want default CloudWatch alarms to be created for this canary\. If you choose to create alarms, they are created with the following name convention:`Synthetics-Alarm-canaryName-index `

   `index` is a number representing each different alarm created for this canary\. The first alarm has an index of 1, the second alarm has an index of 2, and so on\.

1. \(Optional\) To have this canary test an endpoint that is on a VPC, choose **VPC settings**, and then do the following:

   1. Select the VPC that hosts the endpoint\.

   1. Select one or more subnets on your VPC\. You must select a private subnet because a Lambda instance can't be configured to run in a public subnet when an IP address can't be assigned to the Lambda instance during execution\. For more information, see [Configuring a Lambda Function to Access Resources in a VPC](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html)\.

   1. Select one or more security groups on your VPC\.

   If the endpoint is on a VPC, you must enable your canary to send information to CloudWatch and Amazon S3\. For more information, see [Running a canary on a VPC](CloudWatch_Synthetics_Canaries_VPC.md)\.

1. \(Optional\) Under **Tags**, add one or more key\-value pairs as tags for this canary\. Tags can help you identify and organize your AWS resources and track your AWS costs\. For more information, see [Tagging Your Amazon CloudWatch Resources](CloudWatch-Tagging.md)\.

1. \(Optional\) Under **Active tracing**, choose whether to enable active X\-Ray tracing for this canary\. This option is available only if the canary uses runtime version **syn\-nodejs\-2\.0** or later\. For more information, see [Canaries and X\-Ray tracing](CloudWatch_Synthetics_Canaries_tracing.md)\.

## Resources that are created for canaries<a name="CloudWatch_Synthetics_Canaries_Resources_Created"></a>

When you create a canary, the following resources are created:
+ An IAM role with the name `CloudWatchSyntheticsRole-canary-name-uuid` \(if you use CloudWatch console to create the canary and specify for a new role to be created for the canary\)
+ An IAM policy with the name `CloudWatchSyntheticsPolicy-canary-name-uuid`\.
+ An S3 bucket with the name `cw-syn-results-accountID-region`\.
+ Alarms with the name `Synthetics-Alarm-MyCanaryName`, if you want alarms to be created for the canary\.
+ Lambda functions and layers, if you use a blueprint to create the canary\. These resources have the prefix `cwsyn-MyCanaryName`\.
+ CloudWatch Logs log groups with the name `/aws/lambda/cwsyn-MyCanaryName`\.