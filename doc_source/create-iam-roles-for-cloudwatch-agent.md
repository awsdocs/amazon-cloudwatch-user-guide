# Create IAM Roles and Users for Use With CloudWatch Agent<a name="create-iam-roles-for-cloudwatch-agent"></a>

Access to AWS resources requires permissions\. You can create IAM roles and users that include the permissions you need for the CloudWatch agent to write metrics to CloudWatch, and for the CloudWatch agent to communicate with Amazon EC2 and AWS Systems Manager\. You use IAM roles on Amazon EC2 instance, and IAM users on on\-premises servers\.

One role and one user enable CloudWatch agent to be installed on a server and send metrics to CloudWatch\. The other role or user is needed to store your CloudWatch agent configuration in Systems Manager Parameter Store, which enables multiple servers to use one CloudWatch agent configuration\.

The ability to write to Parameter Store is a broad and powerful permission, and should be used only when you need it, and should not be attached to multiple instances in your deployment\. If you are going to store your CloudWatch agent configuration in Parameter Store, you should set up one instance where you perform this configuration, and use the IAM role with permissions to write to Parameter Store only on this instance, and only while you are working with and saving the CloudWatch agent configuration file\.

## Create IAM Roles to Use with CloudWatch Agent on Amazon EC2 Instances<a name="create-iam-roles-for-cloudwatch-agent-roles"></a>

The first procedure creates the IAM role that you need to attach to each Amazon EC2 instance that runs the CloudWatch agent\. This role provides permissions for reading information from the instance and writing it to CloudWatch\.

The second procedure creates the IAM role that you need to attach to the Amazon EC2 instance being used to create the CloudWatch agent configuration file, if you are going to store this file in Systems Manager Parameter Store so that other servers can use it\. This role provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. This role includes permissions sufficient to run the CloudWatch agent as well as to write to Parameter Store\.

**To create the IAM role necessary for each server to run CloudWatch agent**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Paste the following text into the window, replacing the sample text in the window\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "CloudWatchAgentServerPolicy",
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "cloudwatch:PutMetricData",
                   "ec2:DescribeTags",
                   "logs:DescribeLogStreams",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents",
                   "ssm:GetParameter"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. Give the policy a name, such as **CloudWatchAgentServerPolicy**\. Note this name, as you need it later in this procedure\. Optionally give it a description, and choose **Create Policy**\.

1. In the navigation pane on the left, choose **Roles**, **Create role**\. 

1. Choose **EC2**, **EC2 Role for Simple Systems Manager**\. Choose **Next: Permissions**\.

1. In **Role name**, type a name for the role, such as CloudWatchAgentServerRole\. Optionally give it a description, and choose **Create role**\.

1. In the list of roles, choose the role that you just created\.

1. Choose **Attach policy**\. In the resulting list, select the check box next to the policy that you created earlier in this procedure\. Use the search box to find the policy, if necessary\. Choose **Attach policy**\.

   The role is now created\.

The following procedure creates the IAM role that can also write to Parameter Store\. You need to use this role if you are going to store the agent configuration file in Parameter Store so that other servers can use it\. This role provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. The permissions for writing to Parameter Store provide broad powers, and should not be attached to all your servers, and should be used only by administrators\. After you are finished creating the agent configuration file and copying it to Parameter Store, you should detach this role from the instance and use the **CloudWatchAgentServerPolicy** instead\.

**To create the IAM role necessary for an administrator to save an agent configuration file to Systems Manager Parameter Store**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Paste the following text into the window, replacing the sample text in the window\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "CloudWatchAgentAdminPolicy",
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "cloudwatch:PutMetricData",
                   "ec2:DescribeTags",
                   "logs:DescribeLogStreams",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents",
                   "ssm:GetParameter",
                   "ssm:PutParameter"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. Give the policy a name, such as **CloudWatchAgentAdminPolicy**\. Note this name, as you need it later in this procedure\. Optionally give it a description, and choose **Create Policy**\.

1. In the navigation pane on the left, choose **Roles**, **Create role**\. 

1. Choose **EC2**, **EC2 Role for Simple Systems Manager**\. Choose **Next: Permissions**\.

1. Choose **Next: Review**\.

1. In **Role name**, type a name for the role, such as CloudWatchAgentAdminRole\. Optionally give it a description, and choose **Create role**\.

1. In the list of roles, choose the role that you just created\.

1. Choose **Attach policy**\. In the resulting list, select the check box next to the policy that you created earlier in this procedure\. Use the search box to find the policy, if necessary\. Choose **Attach policy**\.

   The role is now created\.

## Create IAM Users to Use with CloudWatch Agent on On\-premises Servers<a name="create-iam-roles-for-cloudwatch-agent-users"></a>

The first procedure creates the IAM user that you need for running the CloudWatch agent\. This user provides permissions for reading information from the instance and writing it to CloudWatch\.

The second procedure creates the IAM user that you can use when creating the CloudWatch agent configuration file, if you are going to store this file in Systems Manager Parameter Store so that other servers can use it\. This user provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. 

**To create the IAM user necessary for each server to run the CloudWatch agent**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Paste the following text into the window, replacing the sample text in the window\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "CloudWatchAgentServerPolicy",
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "cloudwatch:PutMetricData",
                   "ec2:DescribeTags",
                   "logs:DescribeLogStreams",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents",
                   "ssm:GetParameter"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. Give the policy a name, such as **CloudWatchAgentServerPolicy**\. Note this name, as you need it later in this procedure\. Optionally give it a description, and choose **Create Policy**\.

1. In the navigation pane on the left, choose **Users**, **Add user**\. 

1. Type the user name for the new user\.

1. Select **Programmatic access**, and choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In the list of policies, select the check boxes next to two policies: the policy you just created, and **AmazonEC2RoleforSSM**\. Choose **Next: Review**\.

1. Confirm that the correct policies are listed, and choose **Create user**\.

1. Next to the name of the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent, and choose **Close**\. 

The following procedure creates the IAM user that can also write to Parameter Store\. If you are going to store the agent configuration file in Parameter Store so that other servers can use it, you need to use this user\. This user provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. The permissions for writing to Systems Manager Parameter Store provide broad powers, and should not be attached to all your servers, and should be used only by administrators\. You should use this IAM user only when you are storing the agent configuration file in Parameter Store\.

**To create the IAM user necessary for each server to run CloudWatch agent**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**\. 

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Paste the following text into the window, replacing the sample text in the window\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "CloudWatchAgentAdminPolicy",
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "cloudwatch:PutMetricData",
                   "ec2:DescribeTags",
                   "logs:DescribeLogStreams",
                   "logs:CreateLogGroup",
                   "logs:PutLogEvents",
                   "ssm:GetParameter",
                   "ssm:PutParameter"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. Give the policy a name, such as **CloudWatchAgentAdminPolicy**\. Note this name, as you need it later in this procedure\. Optionally give it a description, and choose **Create Policy**\.

1. In the navigation pane on the left, choose **Users**, **Add user**\. 

1. Type the user name for the new user\.

1. Select **Programmatic access**, and choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In the list of policies, select the check boxes next to two policies: the policy you just created, and **AmazonEC2RoleforSSM**\. Choose **Next: Review**\.

1. Confirm that the correct policies are listed, and choose **Create user**\.

1. Next to the name of the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent, and choose **Close**\. 