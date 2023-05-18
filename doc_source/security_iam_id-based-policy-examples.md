# Identity\-based policy examples for Amazon CloudWatch<a name="security_iam_id-based-policy-examples"></a>

By default, users and roles don't have permission to create or modify CloudWatch resources\. They also can't perform tasks by using the AWS Management Console, AWS Command Line Interface \(AWS CLI\), or AWS API\. To grant users permission to perform actions on the resources that they need, an IAM administrator can create IAM policies\. The administrator can then add the IAM policies to roles, and users can assume the roles\.

To learn how to create an IAM identity\-based policy by using these example JSON policy documents, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) in the *IAM User Guide*\.

For details about actions and resource types defined by CloudWatch, including the format of the ARNs for each of the resource types, see [Actions, resources, and condition keys for Amazon CloudWatch](https://docs.aws.amazon.com/service-authorization/latest/reference/list_your_service.html) in the *Service Authorization Reference*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the CloudWatch console](#security_iam_id-based-policy-examples-console)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete CloudWatch resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Using the CloudWatch console<a name="security_iam_id-based-policy-examples-console"></a>

To access the Amazon CloudWatch console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the CloudWatch resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(users or roles\) with that policy\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that they're trying to perform\.

To ensure that users and roles can still use the CloudWatch console, also attach the CloudWatch `ConsoleAccess` or `ReadOnly` AWS managed policy to the entities\. For more information, see [Adding permissions to a user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

### Permissions needed for CloudWatch console<a name="permissions-for-cloudwatch"></a>

The full set of permissions required to work with the CloudWatch console are listed below\. These permissions provide full write and read access to the CloudWatch console\.
+ application\-autoscaling:DescribeScalingPolicies
+ autoscaling:DescribeAutoScalingGroups
+ autoscaling:DescribePolicies
+ cloudtrail:DescribeTrails
+ cloudwatch:DeleteAlarms
+ cloudwatch:DescribeAlarmHistory
+ cloudwatch:DescribeAlarms
+ cloudwatch:GetMetricData
+ cloudwatch:GetMetricStatistics
+ cloudwatch:ListMetrics
+ cloudwatch:PutMetricAlarm
+ cloudwatch:PutMetricData
+ ec2:DescribeInstances
+ ec2:DescribeTags
+ ec2:DescribeVolumes
+ es:DescribeElasticsearchDomain
+ es:ListDomainNames
+ events:DeleteRule
+ events:DescribeRule
+ events:DisableRule
+ events:EnableRule
+ events:ListRules
+ events:PutRule
+ iam:AttachRolePolicy
+ iam:CreateRole
+ iam:GetPolicy
+ iam:GetPolicyVersion
+ iam:GetRole
+ iam:ListAttachedRolePolicies
+ iam:ListRoles
+ kinesis:DescribeStream
+ kinesis:ListStreams
+ lambda:AddPermission
+ lambda:CreateFunction
+ lambda:GetFunctionConfiguration
+ lambda:ListAliases
+ lambda:ListFunctions
+ lambda:ListVersionsByFunction
+ lambda:RemovePermission
+ logs:CancelExportTask
+ logs:CreateExportTask
+ logs:CreateLogGroup
+ logs:CreateLogStream
+ logs:DeleteLogGroup
+ logs:DeleteLogStream
+ logs:DeleteMetricFilter
+ logs:DeleteRetentionPolicy
+ logs:DeleteSubscriptionFilter
+ logs:DescribeExportTasks
+ logs:DescribeLogGroups
+ logs:DescribeLogStreams
+ logs:DescribeMetricFilters
+ logs:DescribeQueries
+ logs:DescribeSubscriptionFilters
+ logs:FilterLogEvents
+ logs:GetLogGroupFields
+ logs:GetLogRecord
+ logs:GetLogEvents
+ logs:GetQueryResults
+ logs:PutMetricFilter
+ logs:PutRetentionPolicy
+ logs:PutSubscriptionFilter
+ logs:StartQuery
+ logs:StopQuery
+ logs:TestMetricFilter
+ s3:CreateBucket
+ s3:ListBucket
+ sns:CreateTopic
+ sns:GetTopicAttributes
+ sns:ListSubscriptions
+ sns:ListTopics
+ sns:SetTopicAttributes
+ sns:Subscribe
+ sns:Unsubscribe
+ sqs:GetQueueAttributes
+ sqs:GetQueueUrl
+ sqs:ListQueues
+ sqs:SetQueueAttributes
+ swf:CreateAction
+ swf:DescribeAction
+ swf:ListActionTemplates
+ swf:RegisterAction
+ swf:RegisterDomain
+ swf:UpdateAction

Additionally, to view the service map in ServiceLens, you need `AWSXrayReadOnlyAccess`