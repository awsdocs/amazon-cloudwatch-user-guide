# Identity and access management for Amazon CloudWatch<a name="auth-and-access-control-cw"></a>

AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use CloudWatch resources\. IAM is an AWS service that you can use with no additional charge\.

**Topics**
+ [Audience](#security_iam_audience)
+ [Authenticating with identities](#security_iam_authentication)
+ [Managing access using policies](#security_iam_access-manage)
+ [How Amazon CloudWatch works with IAM](security_iam_service-with-iam-cw.md)
+ [Resource\-based policy examples for Amazon CloudWatch](security_iam_resource-based-policy-examples.md)
+ [Identity\-based policy examples for Amazon CloudWatch](security_iam_id-based-policy-examples.md)
+ [Troubleshooting Amazon CloudWatch identity and access](security_iam_troubleshoot.md)
+ [CloudWatch dashboard permissions update](dashboard-permissions-update.md)
+ [AWS managed \(predefined\) policies for CloudWatch](#managed-policies-cloudwatch)
+ [Customer managed policy examples](#customer-managed-policies-cw)
+ [CloudWatch updates to AWS managed policies](#security-iam-awsmanpol-updates)
+ [Using condition keys to limit access to CloudWatch namespaces](iam-cw-condition-keys-namespace.md)
+ [Using condition keys to limit Contributor Insights users' access to log groups](iam-cw-condition-keys-contributor.md)
+ [Using condition keys to limit alarm actions](iam-cw-condition-keys-alarm-actions.md)
+ [Using service\-linked roles for CloudWatch](using-service-linked-roles.md)
+ [Using service\-linked roles for CloudWatch RUM](using-service-linked-roles-RUM.md)
+ [Using service\-linked roles for CloudWatch Application Insights](CHAP_using-service-linked-roles-appinsights.md)
+ [AWS managed policies for Amazon CloudWatch Application Insights](security-iam-awsmanpol-appinsights.md)
+ [Amazon CloudWatch permissions reference](permissions-reference-cw.md)

## Audience<a name="security_iam_audience"></a>

How you use AWS Identity and Access Management \(IAM\) differs, depending on the work that you do in CloudWatch\.

**Service user** – If you use the CloudWatch service to do your job, then your administrator provides you with the credentials and permissions that you need\. As you use more CloudWatch features to do your work, you might need additional permissions\. Understanding how access is managed can help you request the right permissions from your administrator\. If you cannot access a feature in CloudWatch, see [Troubleshooting Amazon CloudWatch identity and access](security_iam_troubleshoot.md)\.

**Service administrator** – If you're in charge of CloudWatch resources at your company, you probably have full access to CloudWatch\. It's your job to determine which CloudWatch features and resources your service users should access\. You must then submit requests to your IAM administrator to change the permissions of your service users\. Review the information on this page to understand the basic concepts of IAM\. To learn more about how your company can use IAM with CloudWatch, see [How Amazon CloudWatch Internet Monitor works with IAM](security_iam_service-with-iam.md)\.

**IAM administrator** – If you're an IAM administrator, you might want to learn details about how you can write policies to manage access to CloudWatch\. To view example CloudWatch identity\-based policies that you can use in IAM, see [Identity\-based policy examples for Amazon CloudWatch](security_iam_id-based-policy-examples.md)\.

## Authenticating with identities<a name="security_iam_authentication"></a>

Authentication is how you sign in to AWS using your identity credentials\. You must be *authenticated* \(signed in to AWS\) as the AWS account root user, as an IAM user, or by assuming an IAM role\.

You can sign in to AWS as a federated identity by using credentials provided through an identity source\. AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\) users, your company's single sign\-on authentication, and your Google or Facebook credentials are examples of federated identities\. When you sign in as a federated identity, your administrator previously set up identity federation using IAM roles\. When you access AWS by using federation, you are indirectly assuming a role\.

Depending on the type of user you are, you can sign in to the AWS Management Console or the AWS access portal\. For more information about signing in to AWS, see [How to sign in to your AWS account](https://docs.aws.amazon.com/signin/latest/userguide/how-to-sign-in.html) in the *AWS Sign\-In User Guide*\.

If you access AWS programmatically, AWS provides a software development kit \(SDK\) and a command line interface \(CLI\) to cryptographically sign your requests using your credentials\. If you don't use AWS tools, you must sign requests yourself\. For more information about using the recommended method to sign requests yourself, see [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the *AWS General Reference*\.

Regardless of the authentication method that you use, you might be required to provide additional security information\. For example, AWS recommends that you use multi\-factor authentication \(MFA\) to increase the security of your account\. To learn more, see [Multi\-factor authentication](https://docs.aws.amazon.com/singlesignon/latest/userguide/enable-mfa.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide* and [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.

### AWS account root user<a name="security_iam_authentication-rootuser"></a>

  When you create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you don't use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html) in the *AWS Account Management Reference Guide*\. 

### Federated identity<a name="security_iam_authentication-federated"></a>

As a best practice, require human users, including users that require administrator access, to use federation with an identity provider to access AWS services by using temporary credentials\.

A *federated identity* is a user from your enterprise user directory, a web identity provider, the AWS Directory Service, the Identity Center directory, or any user that accesses AWS services by using credentials provided through an identity source\. When federated identities access AWS accounts, they assume roles, and the roles provide temporary credentials\.

For centralized access management, we recommend that you use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\. You can create users and groups in IAM Identity Center, or you can connect and synchronize to a set of users and groups in your own identity source for use across all your AWS accounts and applications\. For information about IAM Identity Center, see [What is IAM Identity Center?](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.

### IAM users and groups<a name="security_iam_authentication-iamuser"></a>

An *[IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html)* is an identity within your AWS account that has specific permissions for a single person or application\. Where possible, we recommend relying on temporary credentials instead of creating IAM users who have long\-term credentials such as passwords and access keys\. However, if you have specific use cases that require long\-term credentials with IAM users, we recommend that you rotate access keys\. For more information, see [Rotate access keys regularly for use cases that require long\-term credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#rotate-credentials) in the *IAM User Guide*\.

An [https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) is an identity that specifies a collection of IAM users\. You can't sign in as a group\. You can use groups to specify permissions for multiple users at a time\. Groups make permissions easier to manage for large sets of users\. For example, you could have a group named *IAMAdmins* and give that group permissions to administer IAM resources\.

Users are different from roles\. A user is uniquely associated with one person or application, but a role is intended to be assumable by anyone who needs it\. Users have permanent long\-term credentials, but roles provide temporary credentials\. To learn more, see [When to create an IAM user \(instead of a role\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html#id_which-to-choose) in the *IAM User Guide*\.

### IAM roles<a name="security_iam_authentication-iamrole"></a>

An *[IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)* is an identity within your AWS account that has specific permissions\. It is similar to an IAM user, but is not associated with a specific person\. You can temporarily assume an IAM role in the AWS Management Console by [switching roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html)\. You can assume a role by calling an AWS CLI or AWS API operation or by using a custom URL\. For more information about methods for using roles, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the *IAM User Guide*\.

IAM roles with temporary credentials are useful in the following situations:
+ **Federated user access** –  To assign permissions to a federated identity, you create a role and define permissions for the role\. When a federated identity authenticates, the identity is associated with the role and is granted the permissions that are defined by the role\. For information about roles for federation, see [ Creating a role for a third\-party Identity Provider](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\. If you use IAM Identity Center, you configure a permission set\. To control what your identities can access after they authenticate, IAM Identity Center correlates the permission set to a role in IAM\. For information about permissions sets, see [ Permission sets](https://docs.aws.amazon.com/singlesignon/latest/userguide/permissionsetsconcept.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\. 
+ **Temporary IAM user permissions** – An IAM user or role can assume an IAM role to temporarily take on different permissions for a specific task\.
+ **Cross\-account access** – You can use an IAM role to allow someone \(a trusted principal\) in a different account to access resources in your account\. Roles are the primary way to grant cross\-account access\. However, with some AWS services, you can attach a policy directly to a resource \(instead of using a role as a proxy\)\. To learn the difference between roles and resource\-based policies for cross\-account access, see [How IAM roles differ from resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.
+ **Cross\-service access** –  Some AWS services use features in other AWS services\. For example, when you make a call in a service, it's common for that service to run applications in Amazon EC2 or store objects in Amazon S3\. A service might do this using the calling principal's permissions, using a service role, or using a service\-linked role\. 
  + **Principal permissions** –  When you use an IAM user or role to perform actions in AWS, you are considered a principal\. Policies grant permissions to a principal\. When you use some services, you might perform an action that then triggers another action in a different service\. In this case, you must have permissions to perform both actions\. To see whether an action requires additional dependent actions in a policy, see [Actions, resources, and condition keys for Amazon CloudWatch](https://docs.aws.amazon.com/service-authorization/latest/reference/list_your_service.html) in the *Service Authorization Reference*\. 
  + **Service role** –  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 
  + **Service\-linked role** –  A service\-linked role is a type of service role that is linked to an AWS service\. The service can assume the role to perform an action on your behalf\. Service\-linked roles appear in your AWS account and are owned by the service\. An IAM administrator can view, but not edit the permissions for service\-linked roles\. 
+ **Applications running on Amazon EC2** –  You can use an IAM role to manage temporary credentials for applications that are running on an EC2 instance and making AWS CLI or AWS API requests\. This is preferable to storing access keys within the EC2 instance\. To assign an AWS role to an EC2 instance and make it available to all of its applications, you create an instance profile that is attached to the instance\. An instance profile contains the role and enables programs that are running on the EC2 instance to get temporary credentials\. For more information, see [Using an IAM role to grant permissions to applications running on Amazon EC2 instances](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html) in the *IAM User Guide*\. 

To learn whether to use IAM roles or IAM users, see [When to create an IAM role \(instead of a user\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html#id_which-to-choose_role) in the *IAM User Guide*\.

## Managing access using policies<a name="security_iam_access-manage"></a>

You control access in AWS by creating policies and attaching them to AWS identities or resources\. A policy is an object in AWS that, when associated with an identity or resource, defines their permissions\. AWS evaluates these policies when a principal \(user, root user, or role session\) makes a request\. Permissions in the policies determine whether the request is allowed or denied\. Most policies are stored in AWS as JSON documents\. For more information about the structure and contents of JSON policy documents, see [Overview of JSON policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#access_policies-json) in the *IAM User Guide*\.

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

By default, users and roles have no permissions\. To grant users permission to perform actions on the resources that they need, an IAM administrator can create IAM policies\. The administrator can then add the IAM policies to roles, and users can assume the roles\.

IAM policies define permissions for an action regardless of the method that you use to perform the operation\. For example, suppose that you have a policy that allows the `iam:GetRole` action\. A user with that policy can get role information from the AWS Management Console, the AWS CLI, or the AWS API\.

### Identity\-based policies<a name="security_iam_access-manage-id-based-policies"></a>

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as an IAM user, group of users, or role\. These policies control what actions users and roles can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

Identity\-based policies can be further categorized as *inline policies* or *managed policies*\. Inline policies are embedded directly into a single user, group, or role\. Managed policies are standalone policies that you can attach to multiple users, groups, and roles in your AWS account\. Managed policies include AWS managed policies and customer managed policies\. To learn how to choose between a managed policy or an inline policy, see [Choosing between managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#choosing-managed-or-inline) in the *IAM User Guide*\.

### Resource\-based policies<a name="security_iam_access-manage-resource-based-policies"></a>

Resource\-based policies are JSON policy documents that you attach to a resource\. Examples of resource\-based policies are IAM *role trust policies* and Amazon S3 *bucket policies*\. In services that support resource\-based policies, service administrators can use them to control access to a specific resource\. For the resource where the policy is attached, the policy defines what actions a specified principal can perform on that resource and under what conditions\. You must [specify a principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) in a resource\-based policy\. Principals can include accounts, users, roles, federated users, or AWS services\.

Resource\-based policies are inline policies that are located in that service\. You can't use AWS managed policies from IAM in a resource\-based policy\.

### Access control lists \(ACLs\)<a name="security_iam_access-manage-acl"></a>

Access control lists \(ACLs\) control which principals \(account members, users, or roles\) have permissions to access a resource\. ACLs are similar to resource\-based policies, although they do not use the JSON policy document format\.

Amazon S3, AWS WAF, and Amazon VPC are examples of services that support ACLs\. To learn more about ACLs, see [Access control list \(ACL\) overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html) in the *Amazon Simple Storage Service Developer Guide*\.

### Other policy types<a name="security_iam_access-manage-other-policies"></a>

AWS supports additional, less\-common policy types\. These policy types can set the maximum permissions granted to you by the more common policy types\. 
+ **Permissions boundaries** – A permissions boundary is an advanced feature in which you set the maximum permissions that an identity\-based policy can grant to an IAM entity \(IAM user or role\)\. You can set a permissions boundary for an entity\. The resulting permissions are the intersection of an entity's identity\-based policies and its permissions boundaries\. Resource\-based policies that specify the user or role in the `Principal` field are not limited by the permissions boundary\. An explicit deny in any of these policies overrides the allow\. For more information about permissions boundaries, see [Permissions boundaries for IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html) in the *IAM User Guide*\.
+ **Service control policies \(SCPs\)** – SCPs are JSON policies that specify the maximum permissions for an organization or organizational unit \(OU\) in AWS Organizations\. AWS Organizations is a service for grouping and centrally managing multiple AWS accounts that your business owns\. If you enable all features in an organization, then you can apply service control policies \(SCPs\) to any or all of your accounts\. The SCP limits permissions for entities in member accounts, including each AWS account root user\. For more information about Organizations and SCPs, see [How SCPs work](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_about-scps.html) in the *AWS Organizations User Guide*\.
+ **Session policies** – Session policies are advanced policies that you pass as a parameter when you programmatically create a temporary session for a role or federated user\. The resulting session's permissions are the intersection of the user or role's identity\-based policies and the session policies\. Permissions can also come from a resource\-based policy\. An explicit deny in any of these policies overrides the allow\. For more information, see [Session policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#policies_session) in the *IAM User Guide*\. 

### Multiple policy types<a name="security_iam_access-manage-multiple-policies"></a>

When multiple types of policies apply to a request, the resulting permissions are more complicated to understand\. To learn how AWS determines whether to allow a request when multiple policy types are involved, see [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) in the *IAM User Guide*\.

## AWS managed \(predefined\) policies for CloudWatch<a name="managed-policies-cloudwatch"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to CloudWatch\.

**Topics**
+ [CloudWatchFullAccess](#managed-policies-cloudwatch-CloudWatchFullAccess)
+ [CloudWatchReadOnlyAccess](#managed-policies-cloudwatch-CloudWatchReadOnlyAccess)
+ [CloudWatchActionsEC2Access](#managed-policies-cloudwatch-CloudWatchActionsEC2Access)
+ [CloudWatchAutomaticDashboardsAccess](#managed-policies-cloudwatch-CloudWatch-CloudWatchAutomaticDashboardsAccess)
+ [CloudWatchAgentServerPolicy](#managed-policies-cloudwatch-CloudWatchAgentServerPolicy)
+ [CloudWatchAgentAdminPolicy](#managed-policies-cloudwatch-CloudWatchAgentAdminPolicy)
+ [AWS managed \(predefined\) policies for CloudWatch cross\-account observability](#managed-policies-cloudwatch-crossaccount)
+ [AWS managed \(predefined\) policies for CloudWatch Synthetics](#managed-policies-cloudwatch-canaries)
+ [AWS managed \(predefined\) policies for Amazon CloudWatch RUM](#managed-policies-cloudwatch-RUM)
+ [AWS managed \(predefined\) policies for CloudWatch Evidently](#managed-policies-cloudwatch-evidently)
+ [AWS managed policy for AWS Systems Manager Incident Manager](#managed-policies-cloudwatch-incident-manager)

### CloudWatchFullAccess<a name="managed-policies-cloudwatch-CloudWatchFullAccess"></a>

The **CloudWatchFullAccess** policy grants full access to all CloudWatch and CloudWatch Logs actions and resources\.

It includes `autoscaling:Describe*` so that users with this policy can see the Auto Scaling actions that are associated with CloudWatch alarms\. It includes `sns:*` so that users with this policy can retrieve create Amazon SNS topics and associate them with CloudWatch alarms\. It includes IAM permissions so that users with this policy can view information about service\-linked roles associated with CloudWatch\. It includes the `oam:ListSinks` and `oam:ListAttachedLinks` permissions so that users with this policy can use the console to view data shared from source accounts in CloudWatch cross\-account observability\.

Its contents are as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [{
            "Effect": "Allow",
            "Action": [
                "autoscaling:Describe*",
                "cloudwatch:*",
                "logs:*",
                "sns:*",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:GetRole",
                "oam:ListSinks"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/events.amazonaws.com/AWSServiceRoleForCloudWatchEvents*",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": "events.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "oam:ListAttachedLinks"
            ],
            "Resource": "arn:aws:oam:*:*:sink/*"
        }
    ]
}
```

### CloudWatchReadOnlyAccess<a name="managed-policies-cloudwatch-CloudWatchReadOnlyAccess"></a>

The **CloudWatchReadOnlyAccess** policy grants read\-only access to CloudWatch\.

It includes some `logs:` permissions so that users with this policy can use the console to view CloudWatch Logs information and to use CloudWatch Logs Insights queries\. It includes `autoscaling:Describe*` so that users with this policy can see the Auto Scaling actions that are associated with CloudWatch alarms\. It includes `sns:Get*` and `sns:List*` so that users with this policy can retrieve information about the Amazon SNS topics that receive notifications about CloudWatch alarms\. It includes the `oam:ListSinks` and `oam:ListAttachedLinks` permissions so that users with this policy can use the console to view data shared from source accounts in CloudWatch cross\-account observability\.

The following is the content of the **CloudWatchReadOnlyAccess** policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "autoscaling:Describe*",
                "cloudwatch:Describe*",
                "cloudwatch:Get*",
                "cloudwatch:List*",
                "logs:Get*",
                "logs:List*",
                "logs:StartQuery",
                "logs:StopQuery",
                "logs:Describe*",
                "logs:TestMetricFilter",
                "logs:FilterLogEvents",
                "sns:Get*",
                "sns:List*",
                "oam:ListSinks"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "oam:ListAttachedLinks"
            ],
            "Resource": "arn:aws:oam:*:*:sink/*"
        }
     ]
}
```

### CloudWatchActionsEC2Access<a name="managed-policies-cloudwatch-CloudWatchActionsEC2Access"></a>

The **CloudWatchActionsEC2Access** policy grants read\-only access to CloudWatch alarms and metrics in addition to Amazon EC2 metadata\. It also grants access to the Stop, Terminate, and Reboot API actions for EC2 instances\.

The following is the content of the **CloudWatchActionsEC2Access** policy\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:Describe*",
        "ec2:Describe*",
        "ec2:RebootInstances",
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

### CloudWatchAutomaticDashboardsAccess<a name="managed-policies-cloudwatch-CloudWatch-CloudWatchAutomaticDashboardsAccess"></a>

The **CloudWatch\-CrossAccountAccess** managed policy is used by the **CloudWatch\-CrossAccountSharingRole** IAM role\. This role and policy enable users of cross\-account dashboards to view automatic dashboards in each account that is sharing dashboards\.

The following is the content of **CloudWatchAutomaticDashboardsAccess**:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "autoscaling:DescribeAutoScalingGroups",
        "cloudfront:GetDistribution",
        "cloudfront:ListDistributions",
        "dynamodb:DescribeTable",
        "dynamodb:ListTables",
        "ec2:DescribeInstances",
        "ec2:DescribeVolumes",
        "ecs:DescribeClusters",
        "ecs:DescribeContainerInstances",
        "ecs:ListClusters",
        "ecs:ListContainerInstances",
        "ecs:ListServices",
        "elasticache:DescribeCacheClusters",
        "elasticbeanstalk:DescribeEnvironments",
        "elasticfilesystem:DescribeFileSystems",
        "elasticloadbalancing:DescribeLoadBalancers",
        "kinesis:DescribeStream",
        "kinesis:ListStreams",
        "lambda:GetFunction",
        "lambda:ListFunctions",
        "rds:DescribeDBClusters",
        "rds:DescribeDBInstances",
        "resource-groups:ListGroupResources",
        "resource-groups:ListGroups",
        "route53:GetHealthCheck",
        "route53:ListHealthChecks",
        "s3:ListAllMyBuckets",
        "s3:ListBucket",
        "sns:ListTopics",
        "sqs:GetQueueAttributes",
        "sqs:GetQueueUrl",
        "sqs:ListQueues",
        "synthetics:DescribeCanariesLastRun",
        "tag:GetResources"
      ],
      "Effect": "Allow",
      "Resource": "*"
    },
    {
      "Action": [
        "apigateway:GET"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:apigateway:*::/restapis*"
      ]
    }
  ]
```

### CloudWatchAgentServerPolicy<a name="managed-policies-cloudwatch-CloudWatchAgentServerPolicy"></a>

The **CloudWatchAgentServerPolicy** policy can be used in IAM roles that are attached to Amazon EC2 instances to allow the CloudWatch agent to read information from the instance and write it to CloudWatch\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "ec2:DescribeVolumes",
                "ec2:DescribeTags",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams",
                "logs:DescribeLogGroups",
                "logs:CreateLogStream",
                "logs:CreateLogGroup"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter"
            ],
            "Resource": "arn:aws:ssm:*:*:parameter/AmazonCloudWatch-*"
        }
    ]
}
```

### CloudWatchAgentAdminPolicy<a name="managed-policies-cloudwatch-CloudWatchAgentAdminPolicy"></a>

The **CloudWatchAgentAdminPolicy** policy can be used in IAM roles that are attached to Amazon EC2 instances\. This policy allows the CloudWatch agent to read information from the instance and write it to CloudWatch, and also to write information to Parameter Store\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData",
                "ec2:DescribeTags",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams",
                "logs:DescribeLogGroups",
                "logs:CreateLogStream",
                "logs:CreateLogGroup"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter",
                "ssm:PutParameter"
            ],
            "Resource": "arn:aws:ssm:*:*:parameter/AmazonCloudWatch-*"
        }
    ]
}
```

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for CloudWatch actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

### AWS managed \(predefined\) policies for CloudWatch cross\-account observability<a name="managed-policies-cloudwatch-crossaccount"></a>

The policies in this section grant permissions related to CloudWatch cross\-account observability\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. 

#### CloudWatchCrossAccountSharingConfiguration<a name="managed-policies-cloudwatch-CloudWatchCrossAccountSharingConfiguration"></a>

The **CloudWatchCrossAccountSharingConfiguration** policy grants access to create, manage, and view Observability Access Manager links for sharing CloudWatch resources between accounts\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. The contents are as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:Link",
                "oam:ListLinks"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "oam:DeleteLink",
                "oam:GetLink",
                "oam:TagResource"
            ],
            "Resource": "arn:aws:oam:*:*:link/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "oam:CreateLink",
                "oam:UpdateLink"
            ],
            "Resource": [
                "arn:aws:oam:*:*:link/*",
                "arn:aws:oam:*:*:sink/*"
            ]
        }
    ]
}
```

#### OAMFullAccess<a name="managed-policies-cloudwatch-OAMFullAccess"></a>

The **OAMFullAccess** policy grants access to create, manage, and view Observability Access Manager sinks and links, which are used for CloudWatch cross\-account observability\. 

The **OAMFullAccess** policy by itself does not permit you to share observability data across links\. To create a link to share CloudWatch metrics, you also need either **CloudWatchFullAccess** or **CloudWatchCrossAccountSharingConfiguration**\. To create a link to share CloudWatch Logs log groups, you also need either **CloudWatchLogsFullAccess** or **CloudWatchLogsCrossAccountSharingConfiguration**\. To create a link to share X\-Ray traces, you also need either **AWSXRayFullAccess** or **AWSXRayCrossAccountSharingConfiguration**\.

For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. The contents are as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "oam:*"
            ],
            "Resource": "*"
        }
    ]
}
```

#### OAMReadOnlyAccess<a name="managed-policies-cloudwatch-OAMReadOnlyAccess"></a>

The **OAMReadOnlyAccess** policy grants read\-only access to Observability Access Manager resources, which are used for CloudWatch cross\-account observability\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. The contents are as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "oam:Get*",
                "oam:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

### AWS managed \(predefined\) policies for CloudWatch Synthetics<a name="managed-policies-cloudwatch-canaries"></a>

The **CloudWatchSyntheticsFullAccess** and **CloudWatchSyntheticsReadOnlyAccess** AWS managed policies are available for you to assign to users who will manage or use CloudWatch Synthetics\. The following additional policies are also relevant:
+ **AmazonS3ReadOnlyAccess** and **CloudWatchReadOnlyAccess** – These are necessary to be able to read all Synthetics data in the CloudWatch console\.
+ **AWSLambdaReadOnlyAccess** – To be able to view the source code used by canaries\.
+ **CloudWatchSyntheticsFullAccess** enables you to create canaries, Additionally, to create and delete canaries that have a new IAM role created for them, you also need the following inline policy statement:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "iam:CreateRole",
                  "iam:DeleteRole",
                  "iam:CreatePolicy",
                  "iam:DeletePolicy",
                  "iam:AttachRolePolicy",
                  "iam:DetachRolePolicy",
              ],
              "Resource": [
                  "arn:aws:iam::*:role/service-role/CloudWatchSyntheticsRole*",
                  "arn:aws:iam::*:policy/service-role/CloudWatchSyntheticsPolicy*"
              ]
          }
      ]
  }
  ```
**Important**  
Granting a user the `iam:CreateRole`, `iam:DeleteRole`, `iam:CreatePolicy`, `iam:DeletePolicy`, `iam:AttachRolePolicy`, and `iam:DetachRolePolicy` permissions gives that user full administrative access to create, attach, and delete roles and policies that have ARNs that match `arn:aws:iam::*:role/service-role/CloudWatchSyntheticsRole*` and `arn:aws:iam::*:policy/service-role/CloudWatchSyntheticsPolicy*`\. For example, a user with these permissions can create a policy that has full permissions for all resources, and attach that policy to any role that matches that ARN pattern\. Be very careful about who you grant these permissions to\.

  For information about attaching policies and granting permissions to users, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) and [To embed an inline policy for a user or role](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#embed-inline-policy-console)\.

#### CloudWatchSyntheticsFullAccess<a name="managed-policies-cloudwatch-CloudWatchSyntheticsFullAccess"></a>

The following is the content of the **CloudWatchSyntheticsFullAccess** policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "synthetics:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:PutEncryptionConfiguration"
            ],
            "Resource": [
                "arn:aws:s3:::cw-syn-results-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:ListRoles",
                "s3:ListAllMyBuckets",
                "xray:GetTraceSummaries",
                "xray:BatchGetTraces",
                "apigateway:GET"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetBucketLocation"
            ],
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:ListBucket"
            ],
            "Resource": "arn:aws:s3:::cw-syn-*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObjectVersion"
            ],
            "Resource": "arn:aws:s3:::aws-synthetics-library-*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/service-role/CloudWatchSyntheticsRole*"
            ],
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "lambda.amazonaws.com",
                        "synthetics.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetRole",
                "iam:ListAttachedRolePolicies"
            ],
            "Resource": [
                "arn:aws:iam::*:role/service-role/CloudWatchSyntheticsRole*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStatistics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:DeleteAlarms"
            ],
            "Resource": [
                "arn:aws:cloudwatch:*:*:alarm:Synthetics-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarms"
            ],
            "Resource": [
                "arn:aws:cloudwatch:*:*:alarm:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:CreateFunction",
                "lambda:AddPermission",
                "lambda:PublishVersion",
                "lambda:UpdateFunctionCode",
                "lambda:UpdateFunctionConfiguration",
                "lambda:GetFunctionConfiguration",
                "lambda:DeleteFunction"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:cwsyn-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:GetLayerVersion",
                "lambda:PublishLayerVersion",
                "lambda:DeleteLayerVersion"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:layer:cwsyn-*",
                "arn:aws:lambda:*:*:layer:Synthetics:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeVpcs",
                "ec2:DescribeSubnets",
                "ec2:DescribeSecurityGroups"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:ListTopics"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:CreateTopic",
                "sns:Subscribe",
                "sns:ListSubscriptionsByTopic"
            ],
            "Resource": [
                "arn:*:sns:*:*:Synthetics-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:ListAliases"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:DescribeKey"
            ],
            "Resource": "arn:aws:kms:*:*:key/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": "arn:aws:kms:*:*:key/*",
            "Condition": {
                "StringLike": {
                    "kms:ViaService": [
                        "s3.*.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

#### CloudWatchSyntheticsReadOnlyAccess<a name="managed-policies-cloudwatch-CloudWatchSyntheticsReadOnlyAccess"></a>

The following is the content of the **CloudWatchSyntheticsReadOnlyAccess** policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "synthetics:Describe*",
                "synthetics:Get*",
                "synthetics:List*",
                "lambda:GetFunctionConfiguration"
            ],
            "Resource": "*"
        }
    ]
}
```

### AWS managed \(predefined\) policies for Amazon CloudWatch RUM<a name="managed-policies-cloudwatch-RUM"></a>

The **AmazonCloudWatchRUMFullAccess** and **AmazonCloudWatchRUMReadOnlyAccess** AWS managed policies are available for you to assign to users who will manage or use CloudWatch RUM\.

#### AmazonCloudWatchRUMFullAccess<a name="managed-policies-CloudWatchRUMFullAccess"></a>

The following are the contents of the **AmazonCloudWatchRUMFullAccess** policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "rum:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetRole",
                "iam:CreateServiceLinkedRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/aws-service-role/rum.amazonaws.com/AWSServiceRoleForRealUserMonitoring"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/RUM-Monitor*"
            ],
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "cognito-identity.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarms"
            ],
            "Resource": "arn:aws:cloudwatch:*:*:alarm:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cognito-identity:CreateIdentityPool",
                "cognito-identity:ListIdentityPools",
                "cognito-identity:DescribeIdentityPool",
                "cognito-identity:GetIdentityPoolRoles",
                "cognito-identity:SetIdentityPoolRoles"
            ],
            "Resource": "arn:aws:cognito-identity:*:*:identitypool/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:DeleteLogGroup",
                "logs:PutRetentionPolicy",
                "logs:CreateLogStream"
            ],
            "Resource": "arn:aws:logs:*:*:log-group:*RUMService*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogDelivery",
                "logs:GetLogDelivery",
                "logs:UpdateLogDelivery",
                "logs:DeleteLogDelivery",
                "logs:ListLogDeliveries",
                "logs:DescribeResourcePolicies"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogGroups"
            ],
            "Resource": "arn:aws:logs:*:*:log-group::log-stream:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "synthetics:describeCanaries",
                "synthetics:describeCanariesLastRun"
            ],
            "Resource": "arn:aws:synthetics:*:*:canary:*"
        }
    ]
}
```

#### AmazonCloudWatchRUMReadOnlyAccess<a name="managed-policies-CloudWatchRUMReadOnlyAccess"></a>

The following are the contents of the **AmazonCloudWatchRUMReadOnlyAccess** policy\.

```
{

	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"rum:GetAppMonitor",
				"rum:GetAppMonitorData",
				"rum:ListAppMonitors",
				"rum:ListRumMetricsDestinations",
				"rum:BatchGetRumMetricDefinitions"
			],
			"Resource": "*"
		}
	]
}
```

#### AmazonCloudWatchRUMServiceRolePolicy<a name="managed-policies-AmazonCloudWatchRUMServiceRolePolicy"></a>

You can't attach **AmazonCloudWatchRUMServiceRolePolicy** to your IAM entities\. This policy is attached to a service\-linked role that allows CloudWatch RUM to publish monitoring data to other relevant AWS services\. For more information about this service linked role, see [Using service\-linked roles for CloudWatch RUM](using-service-linked-roles-RUM.md)\.

The complete contents of **AmazonCloudWatchRUMServiceRolePolicy** are as follows\.

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"xray:PutTraceSegments"
			],
			"Resource": [
				"*"
			]
		},
		{
			"Effect": "Allow",
			"Action": "cloudwatch:PutMetricData",
			"Resource": "*",
			"Condition": {
				"StringLike": {
					"cloudwatch:namespace": [
						"RUM/CustomMetrics/*",
						"AWS/RUM"
					]
				}
			}
		}
	]
}
```

### AWS managed \(predefined\) policies for CloudWatch Evidently<a name="managed-policies-cloudwatch-evidently"></a>

The **CloudWatchEvidentlyFullAccess** and **CloudWatchEvidentlyReadOnlyAccess** AWS managed policies are available for you to assign to users who will manage or use CloudWatch Evidently\.

#### CloudWatchEvidentlyFullAccess<a name="managed-policies-CloudWatchEvidentlyFullAccess"></a>

The following are the contents of the **CloudWatchEvidentlyFullAccess** policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "evidently:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:ListRoles"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:GetRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/service-role/CloudWatchRUMEvidentlyRole-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetBucketLocation",
                "s3:ListAllMyBuckets"
            ],
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:GetMetricData",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:DescribeAlarmHistory",
                "cloudwatch:DescribeAlarmsForMetric",
                "cloudwatch:ListTagsForResource"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarms",
                "cloudwatch:TagResource",
                "cloudwatch:UnTagResource"
            ],
            "Resource": [
                "arn:aws:cloudwatch:*:*:alarm:*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudtrail:LookupEvents"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricAlarm"
            ],
            "Resource": [
                "arn:aws:cloudwatch:*:*:alarm:Evidently-Alarm-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:ListTopics"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:CreateTopic",
                "sns:Subscribe",
                "sns:ListSubscriptionsByTopic"
            ],
            "Resource": [
                "arn:*:sns:*:*:Evidently-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogGroups"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

#### CloudWatchEvidentlyReadOnlyAccess<a name="managed-policies-CloudWatchEvidentlyReadOnlyAccess"></a>

The following are the contents of the **CloudWatchEvidentlyReadOnlyAccess** policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "evidently:GetExperiment",
                "evidently:GetFeature",
                "evidently:GetLaunch",
                "evidently:GetProject",
                "evidently:GetSegment",
                "evidently:ListExperiments",
                "evidently:ListFeatures",
                "evidently:ListLaunches",
                "evidently:ListProjects",
                "evidently:ListSegments",
                "evidently:ListSegmentReferencs"                               
            ],
            "Resource": "*"
        }
    ]
}
```

### AWS managed policy for AWS Systems Manager Incident Manager<a name="managed-policies-cloudwatch-incident-manager"></a>

The **AWSCloudWatchAlarms\_ActionSSMIncidentsServiceRolePolicy** policy is attached to a service\-linked role that allows CloudWatch to start incidents in AWS Systems Manager Incident Manager on your behalf\. For more information, see [Service\-linked role permissions for CloudWatch alarms Systems Manager Incident Manager actions](using-service-linked-roles.md#service-linked-role-permissions-incident-manager)\.

The policy has the following permission:
+ ssm\-incidents:StartIncident

## Customer managed policy examples<a name="customer-managed-policies-cw"></a>

In this section, you can find example user policies that grant permissions for various CloudWatch actions\. These policies work when you are using the CloudWatch API, AWS SDKs, or the AWS CLI\.

**Topics**
+ [Example 1: Allow user full access to CloudWatch](#full-access-example-cw)
+ [Example 2: Allow read\-only access to CloudWatch](#read-only-access-example-cw)
+ [Example 3: Stop or terminate an Amazon EC2 instance](#stop-terminate-example-cw)

### Example 1: Allow user full access to CloudWatch<a name="full-access-example-cw"></a>

To grant a user full access to CloudWatch, you can use grant them the **CloudWatchFullAccess** managed policy instead of creating a customer\-managed policy\. The contents of the **CloudWatchFullAccess** are listed in [CloudWatchFullAccess](#managed-policies-cloudwatch-CloudWatchFullAccess)\.

### Example 2: Allow read\-only access to CloudWatch<a name="read-only-access-example-cw"></a>

The following policy allows a user read\-only access to CloudWatch and view Amazon EC2 Auto Scaling actions, CloudWatch metrics, CloudWatch Logs data, and alarm\-related Amazon SNS data\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "autoscaling:Describe*",
        "cloudwatch:Describe*",
        "cloudwatch:Get*",
        "cloudwatch:List*",
        "logs:Get*",
        "logs:Describe*",
        "sns:Get*",
        "sns:List*"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

### Example 3: Stop or terminate an Amazon EC2 instance<a name="stop-terminate-example-cw"></a>

The following policy allows an CloudWatch alarm action to stop or terminate an EC2 instance\. In the sample below, the GetMetricData, ListMetrics, and DescribeAlarms actions are optional\. It is recommended that you include these actions to ensure that you have correctly stopped or terminated the instance\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement": [
 4.     {
 5.       "Action": [
 6.         "cloudwatch:PutMetricAlarm",
 7.         "cloudwatch:GetMetricData",
 8.         "cloudwatch:ListMetrics",
 9.         "cloudwatch:DescribeAlarms"
10.       ],
11.       "Resource": [
12.         "*"
13.       ],
14.       "Effect": "Allow"
15.     },
16.     {
17.       "Action": [
18.         "ec2:DescribeInstanceStatus",
19.         "ec2:DescribeInstances",
20.         "ec2:StopInstances",
21.         "ec2:TerminateInstances"
22.       ],
23.       "Resource": [
24.         "*"
25.       ],
26.       "Effect": "Allow"
27.     }
28.   ]
29. }
```

## CloudWatch updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for CloudWatch since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the CloudWatch Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [CloudWatchCrossAccountSharingConfiguration](#managed-policies-cloudwatch-CloudWatchCrossAccountSharingConfiguration) – New policy  |  CloudWatch added a new policy to enable you to manage CloudWatch cross\-account observability links that share CloudWatch metrics\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. \.  | November 27, 2022 | 
|  [OAMFullAccess](#managed-policies-cloudwatch-OAMFullAccess) – New policy  |  CloudWatch added a new policy to enable you to fully manage CloudWatch cross\-account observability links and sinks\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. \.  | November 27, 2022 | 
|  [OAMReadOnlyAccess](#managed-policies-cloudwatch-OAMReadOnlyAccess) – New policy  |  CloudWatch added a new policy to enable you to view information about CloudWatch cross\-account observability links and sinks\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\. \.  | November 27, 2022 | 
|  [CloudWatchFullAccess](#managed-policies-cloudwatch-CloudWatchFullAccess) – Update to an existing policy  |  CloudWatch added permissions to **CloudWatchFullAccess**\. The `oam:ListSinks` and `oam:ListAttachedLinks` permissions were added so that users with this policy can use the console to view data shared from source accounts in CloudWatch cross\-account observability\.  | November 27, 2022 | 
|  [CloudWatchReadOnlyAccess](#managed-policies-cloudwatch-CloudWatchReadOnlyAccess) – Update to an existing policy  |  CloudWatch added permissions to **CloudWatchReadOnlyAccess**\. The `oam:ListSinks` and `oam:ListAttachedLinks` permissions were added so that users with this policy can use the console to view data shared from source accounts in CloudWatch cross\-account observability\.  | November 27, 2022 | 
|  [AmazonCloudWatchRUMServiceRolePolicy](using-service-linked-roles-RUM.md#service-linked-role-permissions-RUM) – Update to an existing policy  |  CloudWatch RUM updated a condition key in **AmazonCloudWatchRUMServiceRolePolicy**\. The `"Condition": { "StringEquals": { "cloudwatch:namespace": "AWS/RUM" } }` condition key was changed to the following so that CloudWatch RUM can send custom metrics to custom metric namespaces\. <pre>"Condition": {<br />    "StringLike": {<br />		"cloudwatch:namespace": [<br />			"RUM/CustomMetrics/*",<br />			"AWS/RUM"<br />		]<br />	}<br />}<br />									<br />								</pre>  | February 2, 2023 | 
| [AmazonCloudWatchRUMReadOnlyAccess](#managed-policies-CloudWatchRUMReadOnlyAccess) – Updated policy  |  CloudWatch added permissions the **AmazonCloudWatchRUMReadOnlyAccess** policy\. The `rum:ListRumMetricsDestinations` and `rum:BatchGetRumMetricsDefinitions` permissions were added so that CloudWatch RUM can send extended metrics to CloudWatch and Evidently\.  | October 27, 2022 | 
|  [AmazonCloudWatchRUMServiceRolePolicy](using-service-linked-roles-RUM.md#service-linked-role-permissions-RUM) – Update to an existing policy  |  CloudWatch RUM added permissions to **AmazonCloudWatchRUMServiceRolePolicy**\. The `cloudwatch:PutMetricData` permission was added so that CloudWatch RUM can send extended metrics to CloudWatch\.  | October 26, 2022 | 
|  [CloudWatchEvidentlyReadOnlyAccess](#managed-policies-CloudWatchEvidentlyReadOnlyAccess) – Update to an existing policy  |  CloudWatch Evidently added permissions to **CloudWatchEvidentlyReadOnlyAccess**\. The `evidently:GetSegment`, `evidently:ListSegments`, and `evidently:ListSegmentReferences` permissions were added so that users with this policy can see Evidently audience segments that have been created\.  | August 12, 2022 | 
|  [CloudWatchSyntheticsFullAccess](#managed-policies-cloudwatch-CloudWatchSyntheticsFullAccess) – Update to an existing policy  |  CloudWatch Synthetics added permissions to **CloudWatchSyntheticsFullAccess**\. The `lambda:DeleteFunction` and `lambda:DeleteLayerVersion` permissions were added so that CloudWatch Synthetics can delete related resources when a canary is deleted\. The `iam:ListAttachedRolePolicies` was added so that customers can view the policies that are attached to a canary's IAM role\.  | May 6, 2022 | 
|  [AmazonCloudWatchRUMFullAccess](#managed-policies-CloudWatchRUMFullAccess) – New policy  |  CloudWatch added a new policy to enable full management of CloudWatch RUM\. CloudWatch RUM allows you to perform real user monitoring of your web application\. For more information, see [Use CloudWatch RUM](CloudWatch-RUM.md)\.  | November 29, 2021 | 
|  [AmazonCloudWatchRUMReadOnlyAccess](#managed-policies-CloudWatchRUMReadOnlyAccess) – New policy  |  CloudWatch added a new policy to enable read\-only access to CloudWatch RUM\. CloudWatch RUM allows you to perform real user monitoring of your web application\. For more information, see [Use CloudWatch RUM](CloudWatch-RUM.md)\.  | November 29, 2021 | 
|  [CloudWatchEvidentlyFullAccess](#managed-policies-CloudWatchEvidentlyFullAccess) – New policy  |  CloudWatch added a new policy to enable full management of CloudWatch Evidently\. CloudWatch Evidently allows you to perform A/B experiments of your web applications, and to roll them out gradually\. For more information, see [Perform launches and A/B experiments with CloudWatch Evidently](CloudWatch-Evidently.md)\.  | November 29, 2021 | 
|  [CloudWatchEvidentlyReadOnlyAccess](#managed-policies-CloudWatchEvidentlyReadOnlyAccess) – New policy  |  CloudWatch added a new policy to enable read\-only access to CloudWatch Evidently\. CloudWatch Evidently allows you to perform A/B experiments of your web applications, and to roll them out gradually\. For more information, see [Perform launches and A/B experiments with CloudWatch Evidently](CloudWatch-Evidently.md)\.  | November 29, 2021 | 
|  [**AWSServiceRoleForCloudWatchRUM**](using-service-linked-roles-RUM.md) – New managed policy  |  CloudWatch added a policy for a new service\-linked role to allow CloudWatch RUM to pubish monitoring data to other relevant AWS services\.  | November 29, 2021 | 
|  [CloudWatchSyntheticsFullAccess](#managed-policies-cloudwatch-CloudWatchSyntheticsFullAccess) – Update to an existing policy  |  CloudWatch Synthetics added permissions to **CloudWatchSyntheticsFullAccess**, and also changed the scope of one permission\. The `kms:ListAliases` permission was added so that users can list available AWS KMS keys that can be used to encrypt canary artifacts\. The `kms:DescribeKey` permission was added so that users can see the details of keys that will be used to encrypt for canary artifacts\. And the `kms:Decrypt` permission was added to enable users to decrypt canary artifacts\. This decryption ability is limited to use on resources within Amazon S3 buckets\. The `Resource` scope of the `s3:GetBucketLocation` permission was changed from `*` to `arn:aws:s3:::*`\.  | September 29, 2021 | 
|  [CloudWatchSyntheticsFullAccess](#managed-policies-cloudwatch-CloudWatchSyntheticsFullAccess) – Update to an existing policy  |  CloudWatch Synthetics added a permission to **CloudWatchSyntheticsFullAccess**\. The `lambda:UpdateFunctionCode` permission was added so that users with this policy can change the runtime version of canaries\.  | July 20, 2021 | 
|  [ AWSCloudWatchAlarms\_ActionSSMIncidentsServiceRolePolicy](#managed-policies-cloudwatch-incident-manager) – New managed policy  |  CloudWatch added a new managed IAM policy to allow CloudWatch to create incidents in AWS Systems Manager Incident Manager\.  | May 10, 2021 | 
|  [ CloudWatchAutomaticDashboardsAccess](#managed-policies-cloudwatch-CloudWatch-CloudWatchAutomaticDashboardsAccess) – Update to an existing policy  |  CloudWatch added a permission to the **CloudWatchAutomaticDashboardsAccess** managed policy\. The `synthetics:DescribeCanariesLastRun` permission was added to this policy to enable cross\-account dashboard users to see details about CloudWatch Synthetics canary runs\.  | April 20, 2021 | 
|  CloudWatch started tracking changes  |  CloudWatch started tracking changes for its AWS managed policies\.  | April 14, 2021 | 