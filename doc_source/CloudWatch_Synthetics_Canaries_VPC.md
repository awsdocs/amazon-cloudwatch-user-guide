# Running a Canary on a VPC<a name="CloudWatch_Synthetics_Canaries_VPC"></a>

You can run canaries on endpoints on a VPC and public internal endpoints\. To run a canary on a VPC, you must have both the **DNS Resolution** and ** DNS hostnames** options enabled on the VPC\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\.

When you run a canary on a VPC endpoint, you must provide a way for it to send its metrics to CloudWatch and its artifacts to Amazon S3\. If the VPC is already enabled for internet access, there's nothing more for you to do\. The canary executes in your VPC, but can access the internet to upload its metrics and artifacts\.

If the VPC is not already enabled for internet access, you have two options:
+ Enable it for internet access\. For more information, see [How do I give internet access to my Lambda function in a VPC?](https://aws.amazon.com/premiumsupport/knowledge-center/internet-access-lambda-function/) on AWS Support\.
+ If you want to keep your VPC private, you can configure the canary to send its data to CloudWatch and Amazon S3 through private VPC endpoints\. If you have not already done so, you must create endpoints for these services on your VPC\. For more information, see [Using CloudWatch and CloudWatch Synthetics with Interface VPC Endpoints](cloudwatch-and-interface-VPC.md) and [Amazon VPC Endpoints for Amazon S3](https://docs.aws.amazon.com/glue/latest/dg/vpc-endpoints-s3.html)\. 

## Troubleshooting a Canary on a VPC<a name="CloudWatch_Synthetics_Canaries_VPC_troubleshoot"></a>

If you have issues after creating or updating a canary, one of the following sections might help you troubleshoot the problem\.

### New Canary in Error State or Canary Can't Be Updated<a name="CloudWatch_Synthetics_Canaries_VPC_troubleshoot_errorstate"></a>

If you create a canary to run on a VPC and it immediately goes into an error state, or you can't update a canary to run on a VPC, the canary's role might not have the right permissions\. To run on a VPC, a canary must have the permissions `ec2:CreateNetworkInterface`, `ec2:DescribeNetworkInterfaces`, and `ec2:DeleteNetworkInterface`\. These permissions are all contained in the `AWSLambdaVPCAccessExecutionRole` managed policy\. For more information, see [Execution Role and User Permissions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html#vpc-permissions)\.

If this issue happened when you created a canary, you must delete the canary, and create a new one\. If you use the CloudWatch console to create the new canary, under **Access Permissions**, select **Create a new role**\. A new role that includes all permissions required to run the canary is created\.

If this issue happens when you update a canary, you can update the canary again and provide a new role that has the required permissions\.

### "No test result returned" Error<a name="CloudWatch_Synthetics_Canaries_VPC_troubleshoot_noresult"></a>

If a canary displays a "no test result returned" error, one of the following issues might be the cause: 
+ If your VPC does not have internet access, you must use VPC endpoints to give the canary access to CloudWatch and Amazon S3\. You must enable the **DNS resolution** and **DNS hostname** options in the VPC for these endpoint addresses to resolve correctly\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\.
+ Canaries must run in private subnets within a VPC\. To check this, open the **Subnets** page in the VPC console\. Check the subnets that you selected when configuring the canary\. If they have a path to an internet gateway \(**igw\-**\), they are not private subnets\.

To help you troubleshoot these issues, see the logs for the canary\.

**To see the log events from a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Log groups**\.

1. Choose the name of the canary's log group\. The log group name starts with `/aws/lambda/cwsyn-canary-name`\.