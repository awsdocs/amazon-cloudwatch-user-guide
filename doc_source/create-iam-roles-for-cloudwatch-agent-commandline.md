# Create IAM Roles and Users for Use With CloudWatch Agent<a name="create-iam-roles-for-cloudwatch-agent-commandline"></a>

Access to AWS resources requires permissions\. You create an IAM role, an IAM user, or both to grant permissions that the CloudWatch agent needs to write metrics to CloudWatch\. If you're going to use the agent on Amazon EC2 instances, you must create an IAM role\. If you're going to use the agent on on\-premises servers, you must create an IAM user\.

**Note**  
We recently modified the following procedures by using new `CloudWatchAgentServerPolicy` and `CloudWatchAgentAdminPolicy` policies created by Amazon, instead of requiring customers to create these policies themselves\. For writing files to and downloading files from the Parameter Store, the policies created by Amazon support only files with names that start with `AmazonCloudWatch-`\. If you have a CloudWatch agent configuration file with a file name that doesn't start with `AmazonCloudWatch-`, these policies can't be used to write the file to Parameter Store or download it from Parameter Store\.

If you're going to run the CloudWatch agent on Amazon EC2 instances, use the following steps to create the necessary IAM role\. This role provides permissions for reading information from the instance and writing it to CloudWatch\.

**To create the IAM role necessary to run the CloudWatch agent on EC2 instances**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Roles** and then **Create role**\. 

1. For **Choose the service that will use this role**, choose ****EC2** Allows EC2 instances to call AWS services on your behalf**\. Choose **Next: Permissions**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find the policy\. 

1. Choose **Next: Review**\.

1. Confirm that **CloudWatchAgentServerPolicy** appears next to **Policies**\. In **Role name**, enter a name for the role, such as *CloudWatchAgentServerRole*\. Optionally give it a description\. Then choose **Create role**\.

   The role is now created\.

   The role is now created\.

If you're going to run the CloudWatch agent on on\-premises servers, use the following steps to create the necessary IAM user\. 

**To create the IAM user necessary for the CloudWatch agent to run on on\-premises servers**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Users** and then **Add user**\. 

1. Enter the user name for the new user\.

1. Select **Programmatic access** and choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find the policy\.

1. Choose **Next: Review**\.

1. Confirm that the correct policies are listed, and choose **Create user**\.

1. Next to the name of the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent\. Choose **Close**\. 