# Create IAM roles and users for use with CloudWatch agent<a name="create-iam-roles-for-cloudwatch-agent-commandline"></a>

Access to AWS resources requires permissions\. You create an IAM role, an IAM user, or both to grant permissions that the CloudWatch agent needs to write metrics to CloudWatch\. If you're going to use the agent on Amazon EC2 instances, you must create an IAM role\. If you're going to use the agent on on\-premises servers, you must create an IAM user\.

**Note**  
We recently modified the following procedures by using new `CloudWatchAgentServerPolicy` and `CloudWatchAgentAdminPolicy` policies created by Amazon, instead of requiring customers to create these policies themselves\. For writing files to and downloading files from the Parameter Store, the policies created by Amazon support only files with names that start with `AmazonCloudWatch-`\. If you have a CloudWatch agent configuration file with a file name that doesn't start with `AmazonCloudWatch-`, these policies can't be used to write the file to Parameter Store or download it from Parameter Store\.

If you're going to run the CloudWatch agent on Amazon EC2 instances, use the following steps to create the necessary IAM role\. This role provides permissions for reading information from the instance and writing it to CloudWatch\.

**To create the IAM role necessary to run the CloudWatch agent on EC2 instances**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Roles** and then **Create role**\. 

1. Make sure that **AWS service** is selected under **Trusted entity type**\.

1. For **Use case**, choose **EC2** under **Common use cases**,

1. Choose **Next**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find the policy\. 

1. Choose **Next**\.

1.  In **Role name**, enter a name for the role, such as *CloudWatchAgentServerRole*\. Optionally give it a description\. Then choose **Create role**\.

   The role is now created\.

1. \(Optional\) If the agent is going to send logs to CloudWatch Logs and you want the agent to be able to set retention policies for these log groups, you need to add the `logs:PutRetentionPolicy` permission to the role\. For more information, see [Allowing the CloudWatch agent to set log retention policy](#CloudWatch-Agent-PutLogRetention)\.

If you're going to run the CloudWatch agent on on\-premises servers, use the following steps to create the necessary IAM user\. 

**To create the IAM user necessary for the CloudWatch agent to run on on\-premises servers**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Users** and then **Add users**\. 

1. Enter the user name for the new user\.

1. Select **Access key \- Programmatic access** and choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find the policy\.

1. Choose **Next: Tags**\.

1. Optionally create tags for the new IAM user, and then choose **Next:Review**\.

1. Confirm that the correct policy is listed, and choose **Create user**\.

1. Next to the name of the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent\. Choose **Close**\. 

## Allowing the CloudWatch agent to set log retention policy<a name="CloudWatch-Agent-PutLogRetention"></a>

You can configure the CloudWatch agent to set the retention policy for log groups that it sends log events to\. If you do this, you must grant the `logs:PutRetentionPolicy` to the IAM role or user that the agent uses\. The agent uses an IAM role to run on Amazon EC2 instances, and uses an IAM user for on\-premises servers\.

**To grant the CloudWatch agent's IAM role permission to set log retention policies**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Roles**\.

1. In the search box, Type the beginning of the name of the CloudWatch agent's IAM role\. You chose this name when you created the role\. It might be named `CloudWatchAgentServerRole`\.

   When you see the role, choose the name of the role\.

1. In the **Permissions** tab, choose **Add permissions**, **Create inline policy**\.

1. Choose the **JSON** tab and copy the following policy into the box, replacing the default JSON in the box:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "logs:PutRetentionPolicy",
         "Resource": "*"
       }
     ]
   }
   ```

1. Choose **Review policy**\.

1. For **Name**, enter **CloudWatchAgentPutLogsRetention** or something similar, and choose **Create policy**\.

**To grant the CloudWatch agent's IAM user permission to set log retention policies**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the left navigation pane, choose **Users**\.

1. In the search box, Type the beginning of the name of the CloudWatch agent's IAM user\. You chose this name when you created the user\.

   When you see the user, choose the name of the user\.

1. In the **Permissions** tab, choose **Add inline policy**\.

1. Choose the **JSON** tab and copy the following policy into the box, replacing the default JSON in the box:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "logs:PutRetentionPolicy",
         "Resource": "*"
       }
     ]
   }
   ```

1. Choose **Review policy**\.

1. For **Name**, enter **CloudWatchAgentPutLogsRetention** or something similar, and choose **Create policy**\.