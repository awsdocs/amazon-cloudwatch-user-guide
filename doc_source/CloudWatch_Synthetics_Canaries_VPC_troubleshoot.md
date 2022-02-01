# Troubleshooting a canary on a VPC<a name="CloudWatch_Synthetics_Canaries_VPC_troubleshoot"></a>

If you have issues after creating or updating a canary on a VPC, one of the following sections might help you troubleshoot the problem\.

## New canary in error state or canary can't be updated<a name="CloudWatch_Synthetics_Canaries_VPC_troubleshoot_errorstate"></a>

If you create a canary to run on a VPC and it immediately goes into an error state, or you can't update a canary to run on a VPC, the canary's role might not have the right permissions\. To run on a VPC, a canary must have the permissions `ec2:CreateNetworkInterface`, `ec2:DescribeNetworkInterfaces`, and `ec2:DeleteNetworkInterface`\. These permissions are all contained in the `AWSLambdaVPCAccessExecutionRole` managed policy\. For more information, see [Execution Role and User Permissions](https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html#vpc-permissions)\.

If this issue happened when you created a canary, you must delete the canary, and create a new one\. If you use the CloudWatch console to create the new canary, under **Access Permissions**, select **Create a new role**\. A new role that includes all permissions required to run the canary is created\.

If this issue happens when you update a canary, you can update the canary again and provide a new role that has the required permissions\.

## "No test result returned" error<a name="CloudWatch_Synthetics_Canaries_VPC_troubleshoot_noresult"></a>

If a canary displays a "no test result returned" error, one of the following issues might be the cause: 
+ If your VPC does not have internet access, you must use VPC endpoints to give the canary access to CloudWatch and Amazon S3\. You must enable the **DNS resolution** and **DNS hostname** options in the VPC for these endpoint addresses to resolve correctly\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\.
+ Canaries must run in private subnets within a VPC\. To check this, open the **Subnets** page in the VPC console\. Check the subnets that you selected when configuring the canary\. If they have a path to an internet gateway \(**igw\-**\), they are not private subnets\.

To help you troubleshoot these issues, see the logs for the canary\.

**To see the log events from a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Log groups**\.

1. Choose the name of the canary's log group\. The log group name starts with `/aws/lambda/cwsyn-canary-name`\.