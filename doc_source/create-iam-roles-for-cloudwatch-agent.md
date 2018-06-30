# Create IAM Roles and Users for Use With CloudWatch Agent<a name="create-iam-roles-for-cloudwatch-agent"></a>

Access to AWS resources requires permissions\. You can create IAM roles and users that include the permissions you need for the CloudWatch agent to write metrics to CloudWatch, and for the CloudWatch agent to communicate with Amazon EC2 and AWS Systems Manager\. You use IAM roles on Amazon EC2 instances, and you use IAM users with on\-premises servers to enable the agent to send data to CloudWatch\.

One role and one user enable CloudWatch agent to be installed on a server and send metrics to CloudWatch\. The other role or user is needed to store your CloudWatch agent configuration in Systems Manager Parameter Store, which enables multiple servers to use one CloudWatch agent configuration\.

The ability to write to Parameter Store is a broad and powerful permission, and should be used only when you need it, and should not be attached to multiple instances in your deployment\. If you are going to store your CloudWatch agent configuration in Parameter Store, you should set up one instance where you perform this configuration, and use the IAM role with permissions to write to Parameter Store only on this instance, and only while you are working with and saving the CloudWatch agent configuration file\.

**Note**  
We recently modified the following procedures by using new **CloudWatchAgentServerPolicy** and **CloudWatchAgentAdminPolicy** policies created by Amazon, instead of requiring customers to create these policies themselves\. For writing files to and downloading files from the Parameter Store, the policies created by Amazon support only files with names that start with "AmazonCloudWatch\-" If you have a CloudWatch agent configuration file with a filename that does not start with `AmazonCloudWatch-`, these policies cannot be used to write the file to Parameter Store or download it from Parameter Store\.

## Create IAM Roles to Use with CloudWatch Agent on Amazon EC2 Instances<a name="create-iam-roles-for-cloudwatch-agent-roles"></a>

The first procedure creates the IAM role that you need to attach to each Amazon EC2 instance that runs the CloudWatch agent\. This role provides permissions for reading information from the instance and writing it to CloudWatch\.

The second procedure creates the IAM role that you need to attach to the Amazon EC2 instance being used to create the CloudWatch agent configuration file, if you are going to store this file in Systems Manager Parameter Store so that other servers can use it\. This role provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. This role includes permissions sufficient to run the CloudWatch agent as well as to write to Parameter Store\.

**To create the IAM role necessary for each server to run CloudWatch agent**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Roles**, **Create role**\. 

1. For **Choose the service that will use this role**, choose ****EC2** Allows EC2 instances to call AWS services on your behalf**\. Choose **Next: Permissions**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. Use the search box to find the policy, if necessary\. 

1. If you will use SSM to install or configure the CloudWatch agent, select the check box next to **AmazonEC2RoleforSSM**\. Use the search box to find the policy, if necessary\. This policy is not necessary if you will start and configure the agent only through the command line\.

1. Choose **Next: Review**

1. Confirm that **CloudWatchAgentServerPolicy** and optionally **AmazonEC2RoleforSSM** appear next to **Policies**\. In **Role name**, type a name for the role, such as CloudWatchAgentServerRole\. Optionally give it a description, and choose **Create role**\.

   The role is now created\.

The following procedure creates the IAM role that can also write to Parameter Store\. You need to use this role if you are going to store the agent configuration file in Parameter Store so that other servers can use it\. This role provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. The permissions for writing to Parameter Store provide broad powers, and should not be attached to all your servers, and should be used only by administrators\. After you are finished creating the agent configuration file and copying it to Parameter Store, you should detach this role from the instance and use the **CloudWatchAgentServerPolicy** instead\.

**To create the IAM role necessary for an administrator to save an agent configuration file to Systems Manager Parameter Store**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Roles**, **Create role**\. 

1.  For **Choose the service that will use this role**, choose ****EC2** Allows EC2 instances to call AWS services on your behalf**\. Choose **Next: Permissions**\.

1. In the list of policies, select the check box next to **CloudWatchAgentAdminPolicy**\. Use the search box to find the policy, if necessary\. 

1. If you will use SSM to install or configure the CloudWatch agent, select the check box next to **AmazonEC2RoleforSSM**\. Use the search box to find the policy, if necessary\. This policy is not necessary if you will start and configure the agent only through the command line\.

1. Choose **Next: Review**

1. Confirm that **CloudWatchAgentAdminPolicy** and optionally **AmazonEC2RoleforSSM** appear next to **Policies**\. In **Role name**, type a name for the role, such as CloudWatchAgentAdminRole\. Optionally give it a description, and choose **Create role**\.

   The role is now created\.

## Create IAM Users to Use with CloudWatch Agent on On\-premises Servers<a name="create-iam-roles-for-cloudwatch-agent-users"></a>

The first procedure creates the IAM user that you need for running the CloudWatch agent\. This user provides permissions for sending data to CloudWatch\.

The second procedure creates the IAM user that you can use when creating the CloudWatch agent configuration file, if you are going to store this file in Systems Manager Parameter Store so that other servers can use it\. This user provides permissions for writing to Parameter Store, in addition to the permissions for writing data to CloudWatch\. 

**To create the IAM user necessary for CloudWatch agent to write data to CloudWatch**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Users**, **Add user**\. 

1. Type the user name for the new user\.

1. Select **Programmatic access**, and choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In the list of policies, select the check box next to **CloudWatchAgentServerPolicy**\. Use the search box to find the policy, if necessary\.

1. If you will use SSM to install or configure the CloudWatch agent, select the check box next to **AmazonEC2RoleforSSM**\. Use the search box to find the policy, if necessary\. This policy is not necessary if you will start and configure the agent only through the command line\.

1. Choose **Next: Review**\.

1. Confirm that the correct policies are listed, and choose **Create user**\.

1. Next to the name of the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent, and choose **Close**\. 

The following procedure creates the IAM user that can also write to Parameter Store\. If you are going to store the agent configuration file in Parameter Store so that other servers can use it, you need to use this user\. This user provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch\. The permissions for writing to Systems Manager Parameter Store provide broad powers, and should not be attached to all your servers, and should be used only by administrators\. You should use this IAM user only when you are storing the agent configuration file in Parameter Store\.

**To create the IAM user necessary to store the configuration file in Parameter Store and send information to CloudWatch**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Users**, **Add user**\. 

1. Type the user name for the new user\.

1. Select **Programmatic access**, and choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In the list of policies, select the check box next to **CloudWatchAgentAdminPolicy**\. se the search box to find the policy, if necessary\.

1. If you will use SSM to install or configure the CloudWatch agent, select the check box next to **AmazonEC2RoleforSSM**\. Use the search box to find the policy, if necessary\. This policy is not necessary if you will start and configure the agent only through the command line\.

1. Choose **Next: Review**\.

1. Confirm that the correct policies are listed, and choose **Create user**\.

1. Next to the name of the new user, choose **Show**\. Copy the access key and secret key to a file so that you can use them when installing the agent, and choose **Close**\. 
