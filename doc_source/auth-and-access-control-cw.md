# Identity and Access Management for Amazon CloudWatch<a name="auth-and-access-control-cw"></a>

Access to Amazon CloudWatch requires credentials\. Those credentials must have permissions to access AWS resources, such as retrieving CloudWatch metric data about your cloud resources\. The following sections provide details about how you can use [AWS Identity and Access Management \(IAM\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) and CloudWatch to help secure your resources by controlling who can access them:
+ [Authentication](#authentication-cw)
+ [Access Control](#access-control-cw)

## Authentication<a name="authentication-cw"></a>

You can access AWS as any of the following types of identities:
+ **AWS account root user** – When you sign up for AWS, you provide an email address and password that is associated with your AWS account\. These are your *AWS account user credentials* and they provide complete access to all of your AWS resources\.
**Important**  
For security reasons, we recommend that you use the AWS account user credentials only to create an *administrator*, which is an *IAM user* with full permissions to your AWS account\. Then, you can use this administrator to create other IAM users and roles with limited permissions\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users) and [Creating an Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.
+ **IAM user** – An [IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) is an identity within your AWS account that has specific custom permissions \(for example, permissions to view metrics in CloudWatch\)\. You can use an IAM user name and password to sign in to secure AWS webpages like the [AWS Management Console](https://console.aws.amazon.com/), [AWS Discussion Forums](https://forums.aws.amazon.com/), or the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

   

  In addition to a user name and password, you can also generate [access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) for each user\. You can use these keys when you access AWS services programmatically, either through [one of the several SDKs](https://aws.amazon.com/tools/) or by using the [AWS Command Line Interface \(CLI\)](https://aws.amazon.com/cli/)\. The SDK and AWS CLI tools use the access keys to cryptographically sign your request\. If you don’t use the AWS tools, you must sign the request yourself\. CloudWatch supports *Signature Version 4*, a protocol for authenticating inbound API requests\. For more information about authenticating requests, see [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.

   
+ **IAM role** – An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is another IAM identity you can create in your account that has specific permissions\. It is similar to an *IAM user*, but it is not associated with a specific person\. An IAM role enables you to obtain temporary access keys that can be used to access AWS services and resources\. IAM roles with temporary credentials are useful in the following situations:

   
  + **Federated user access** – Instead of creating an IAM user, you can use preexisting identities from AWS Directory Service, your enterprise user directory, or a web identity provider \(IdP\)\. These are known as *federated users*\. AWS assigns a role to a federated user when access is requested through an [IdP](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html)\. For more information, see [Federated Users and Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html#intro-access-roles) in the *IAM User Guide*\.

     
  + **Cross\-account access** – You can use an IAM role in your account to grant another AWS account permissions to access your account’s resources\. For an example, see [Tutorial: Delegate Access Across AWS Accounts Using IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) in the *IAM User Guide*\.

     
  + **AWS service access** – You can use an IAM role in your account to grant an AWS service the permissions needed to access your account’s resources\. For example, you can create a role that allows Amazon Redshift to access an Amazon S3 bucket on your behalf and then load data stored in the bucket into an Amazon Redshift cluster\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

      
  + **Applications running on Amazon EC2** – Instead of storing access keys within the EC2 instance for use by applications running on the instance and making API requests, you can use an IAM role to manage temporary credentials for these applications\. To assign an AWS role to an EC2 instance and make it available to all of its applications, you can create an instance profile that is attached to the instance\. An instance profile contains the role and enables programs running on the EC2 instance to get temporary credentials\. For more information, see [Using Roles for Applications on Amazon EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) in the *IAM User Guide*\.

    

## Access Control<a name="access-control-cw"></a>

You can have valid credentials to authenticate your requests, but unless you have permissions you cannot create or access CloudWatch resources\. For example, you must have permissions to create CloudWatch dashboard widgets, view metrics, and so on\.

The following sections describe how to manage permissions for CloudWatch\. We recommend that you read the overview first\.
+ [Overview of Managing Access Permissions to Your CloudWatch Resources](iam-access-control-overview-cw.md)
+ [Using Identity\-Based Policies \(IAM Policies\) for CloudWatch](iam-identity-based-access-control-cw.md)
+ [Amazon CloudWatch Permissions Reference](permissions-reference-cw.md)