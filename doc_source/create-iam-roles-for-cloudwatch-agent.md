# Create IAM Roles and Users for Use with the CloudWatch Agent<a name="create-iam-roles-for-cloudwatch-agent"></a>

Access to AWS resources requires permissions\. You can create IAM roles and users that include the permissions that you need for the CloudWatch agent to write metrics to CloudWatch and for the CloudWatch agent to communicate with Amazon EC2 and AWS Systems Manager\. You use IAM roles on Amazon EC2 instances, and you use IAM users with on\-premises servers\.

One role or user enables CloudWatch agent to be installed on a server and send metrics to CloudWatch\. The other role or user is needed to store your CloudWatch agent configuration in Systems Manager Parameter Store\.Parameter Store enables multiple servers to use one CloudWatch agent configuration\.

The ability to write to Parameter Store is a broad and powerful permission\. You should use it only when you need it, and it shouldn't be attached to multiple instances in your deployment\. If you store your CloudWatch agent configuration in Parameter Store, we recommend the following:\.
+ Set up one instance where you perform this configuration\.
+ Use the IAM role with permissions to write to Parameter Store only on this instance\.
+ Use the IAM role with permissions to write to Parameter Store only while you are working with and saving the CloudWatch agent configuration file\.

**Note**  
We recently modified the following procedures by using new `CloudWatchAgentServerPolicy` and `CloudWatchAgentAdminPolicy` policies created by Amazon, instead of requiring customers to create these policies themselves\. For writing files to and downloading files from Parameter Store, the policies created by Amazon support only files with names that start with `AmazonCloudWatch-`\. If you have a CloudWatch agent configuration file with a file name that doesn't start with `AmazonCloudWatch-`, these policies can't be used to write the file to Parameter Store or to download the file from Parameter Store\.

## Create IAM Roles to Use with the CloudWatch Agent on Amazon EC2 Instances<a name="create-iam-roles-for-cloudwatch-agent-roles"></a>

The first procedure creates the IAM role that you must attach to each Amazon EC2 instance that runs the CloudWatch agent\. This role provides permissions for reading information from the instance and writing it to CloudWatch\.

The second procedure creates the IAM role that you must attach to the Amazon EC2 instance being used to create the CloudWatch agent configuration file, if you're going to store this file in Systems Manager Parameter Store so that other servers can use it\. This role provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. This role includes permissions sufficient to run the CloudWatch agent as well as to write to Parameter Store\.

**To create the IAM role necessary for each server to run the CloudWatch agent**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\. 

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. Immediately under **Choose the service that will use this role**, choose **EC2**,and then choose **Next: Permissions**\.

1. In the list of policies, select the box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find the policy\. 

1. To use Systems Manager to install or configure the CloudWatch agent, select the box next to **AmazonSSMManagedInstanceCore**\. If necessary, use the search box to find the policy\. This policy isn't necessary if you start and configure the agent only through the command line\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key/\-value pairs to organize, track, or control access for this role, and then choose **Next: Review**\.

1. For **Role name**, enter a name for your new role, such as **CloudWatchAgentServerRole** or another name that you prefer\.

1. \(Optional\) For **Role description**, enter a description\.

1. Confirm that **CloudWatchAgentServerPolicy** and optionally **AmazonSSMManagedInstanceCore** appear next to **Policies**\.

1. Choose **Create role**\.

   The role is now created\.

The following procedure creates the IAM role that can also write to Parameter Store\. You can use this role to store the agent configuration file in Parameter Store so that other servers can retrieve it\. 

The permissions for writing to Parameter Store provide broad access\. This role shouldn't be attached to all your servers, and only administrators should use it\. After you create the agent configuration file and copy it to Parameter Store, you should detach this role from the instance and use `CloudWatchAgentServerRole` instead\.

**To create the IAM role for an administrator to write to Parameter Store**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles** and then choose **Create role**\. 

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. Immediately under **Choose the service that will use this role**, choose **EC2**, and then choose **Next: Permissions**\.

1. In the list of policies, select the box next to **CloudWatchAgentAdminPolicy**\. If necessary, use the search box to find the policy\. 

1. To use Systems Manager to install or configure the CloudWatch agent, select the box next to **AmazonSSMManagedInstanceCore**\. If necessary, use the search box to find the policy\. This policy isn't necessary if you start and configure the agent only through the command line\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key/\-value pairs to organize, track, or control access for this role, and then choose **Next: Review**\.

1. For **Role name**, enter a name for your new role, such as **CloudWatchAgentAdminRole** or another name that you prefer\.

1. \(Optional\) For **Role description**, enter a description\.

1. Confirm that **CloudWatchAgentAdminPolicy** and optionally **AmazonSSMManagedInstanceCore** appear next to **Policies**\.

1. Choose **Create role**\.

   The role is now created\.

## Create IAM Users to Use with the CloudWatch Agent on On\-Premises Servers<a name="create-iam-roles-for-cloudwatch-agent-users"></a>

The first procedure creates the IAM user that you need to run the CloudWatch agent\. This user provides permissions to send data to CloudWatch\.

The second procedure creates the IAM user that you can use when creating the CloudWatch agent configuration file\. Use this procedure to store this file in Systems Manager Parameter Store so that other servers can use it\. This user provides permissions to write to Parameter Store, in addition to the permissions to write data to CloudWatch\. 

**To create the IAM user necessary for the CloudWatch agent to write data to CloudWatch**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**, and then choose **Add user**\. 

1. Enter the user name for the new user\.

1. For **Access type**, select **Programmatic access**, and then choose **Next: Permissions**\.

1. For **Set permissions**, choose **Attach existing policies directly**\.

1. In the list of policies, select the box next to **CloudWatchAgentServerPolicy**\. If necessary, use the search box to find the policy\.

1. To use Systems Manager to install or configure the CloudWatch agent, select the box next to **AmazonSSMManagedInstanceCore**\. If necessary, use the search box to find the policy\. This policy isn't necessary if you start and configure the agent only through the command line\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key/\-value pairs to organize, track, or control access for this role, and then choose **Next: Review**\.

1. Confirm that the correct policies are listed, and then choose **Create user**\.

1. In the row for the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent\. Choose **Close**\. 

The following procedure creates the IAM user that can also write to Parameter Store\. If you're going to store the agent configuration file in Parameter Store so that other servers can use it, you need to use this IAM user\. This IAM user provides permissions for writing to Parameter Store\. This user also provides the permissions for reading information from the instance and writing it to CloudWatch\. The permissions for writing to Systems Manager Parameter Store provide broad access\. This IAM user shouldn't be attached to all your servers, and only administrators should use it\. You should use this IAM user only when you are storing the agent configuration file in Parameter Store\.

**To create the IAM user necessary to store the configuration file in Parameter Store and send information to CloudWatch**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**, and then choose **Add user**\. 

1. Enter the user name for the new user\.

1. For **Access type**, select **Programmatic access**, and then choose **Next: Permissions**\.

1. For **Set permissions**, choose **Attach existing policies directly**\.

1. In the list of policies, select the box next to **CloudWatchAgentAdminPolicy**\. If necessary, use the search box to find the policy\.

1. To use Systems Manager to install or configure the CloudWatch agent, select the check box next to **AmazonSSMManagedInstanceCore**\. If necessary, use the search box to find the policy\. This policy isn't necessary if you start and configure the agent only through the command line\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key/\-value pairs to organize, track, or control access for this role, and then choose **Next: Review**\.

1. Confirm that the correct policies are listed, and then choose **Create user**\.

1. In the row for the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent\. Choose **Close**\. 