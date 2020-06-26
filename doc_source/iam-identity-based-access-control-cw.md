# Using Identity\-Based Policies \(IAM Policies\) for CloudWatch<a name="iam-identity-based-access-control-cw"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on CloudWatch resources\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your CloudWatch resources\. For more information, see [Access Control](auth-and-access-control-cw.md#access-control-cw)\. 

The sections in this topic cover the following:
+ [Permissions Required to Use the CloudWatch Console](#console-permissions-cw)
+ [AWS Managed \(Predefined\) Policies for CloudWatch](#managed-policies-cloudwatch)
+ [Customer Managed Policy Examples](#customer-managed-policies-cw)

The following shows an example of a permissions policy\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement":[{
 4.       "Effect":"Allow",
 5.       "Action":["cloudwatch:GetMetricStatistics","cloudwatch:ListMetrics"],
 6.       "Resource":"*",
 7.       "Condition":{
 8.          "Bool":{
 9.             "aws:SecureTransport":"true"
10.             }
11.          }
12.       }
13.    ]
14. }
```

This sample policy has one statement that grants permissions to a group for two CloudWatch actions \(`cloudwatch:GetMetricStatisticsdata`, and `cloudwatch:ListMetrics`\), but only if the group uses SSL with the request \(`"aws:SecureTransport":"true"`\)\. For more information about the elements within an IAM policy statement, see [Specifying Policy Elements: Actions, Effects, and Principals](iam-access-control-overview-cw.md#actions-effects-principals-cw) and [IAM Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html) in *IAM User Guide*\.

## Permissions Required to Use the CloudWatch Console<a name="console-permissions-cw"></a>

For a user to work with the CloudWatch console, that user must have a minimum set of permissions that allow the user to describe other AWS resources in their AWS account\. The CloudWatch console requires permissions from the following services:
+ Amazon EC2 Auto Scaling
+ CloudTrail
+ CloudWatch
+ CloudWatch Events
+ CloudWatch Logs
+ Amazon EC2
+ Amazon ES
+ IAM
+ Kinesis
+ Lambda
+ Amazon S3
+ Amazon SNS
+ Amazon SQS
+ Amazon SWF
+ X\-Ray, if you are using the ServiceLens feature

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can still use the CloudWatch console, also attach the `CloudWatchReadOnlyAccess` managed policy to the user, as described in [AWS Managed \(Predefined\) Policies for CloudWatch](#managed-policies-cloudwatch)\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the CloudWatch API\.

The full set of permissions required to work with the CloudWatch console are listed below:
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

## AWS Managed \(Predefined\) Policies for CloudWatch<a name="managed-policies-cloudwatch"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to CloudWatch:
+ **CloudWatchFullAccess** – Grants full access to CloudWatch\.
+ **CloudWatchReadOnlyAccess** – Grants read\-only access to CloudWatch\.
+ **CloudWatchActionsEC2Access** – Grants read\-only access to CloudWatch alarms and metrics in addition to Amazon EC2 metadata\. Grants access to the Stop, Terminate, and Reboot API actions for EC2 instances\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for CloudWatch actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

### AWS Managed \(Predefined\) Policies for CloudWatch Synthetics<a name="managed-policies-cloudwatch-canaries"></a>

The following AWS managed policies, which you can attach to users in your account, are specific to CloudWatch Synthetics:
+ **CloudWatchSyntheticsFullAccess** or **CloudWatchSyntheticsReadOnlyAccess** – To view canary details and the results of canary test runs\.
+ **AmazonS3ReadOnlyAccess** and **CloudWatchReadOnlyAccess** – To be able to read all Synthetics data in the CloudWatch console\.
+ **AWSLambdaReadOnlyAccess** – To be able to view the source code used by canaries\.
+ **CloudWatchSyntheticsFullAccess** – Enables you to create canaries, Additionally, to create a canary that will have a new IAM role created for it, you also need the following inline policy statement:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "iam:CreateRole",
                  "iam:CreatePolicy",
                  "iam:AttachRolePolicy"
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
Granting a user the `iam:CreateRole`, `iam:CreatePolicy`, and `iam:AttachRolePolicy` permissions gives that user full administrative access to your AWS account\. For example, a user with these permissions can create a policy that has full permissions for all resources, and attach that policy to any role\. Be very careful about who you grant these permissions to\.

  For information about attaching policies and granting permissions to users, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) and [To embed an inline policy for a user or role](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#embed-inline-policy-console)\.

## Customer Managed Policy Examples<a name="customer-managed-policies-cw"></a>

In this section, you can find example user policies that grant permissions for various CloudWatch actions\. These policies work when you are using the CloudWatch API, AWS SDKs, or the AWS CLI\.

**Topics**
+ [Example 1: Allow User Full Access to CloudWatch](#full-access-example-cw)
+ [Example 2: Allow Read\-Only Access to CloudWatch](#read-only-access-example-cw)
+ [Example 3: Stop or Terminate an Amazon EC2 Instance](#stop-terminate-example-cw)

### Example 1: Allow User Full Access to CloudWatch<a name="full-access-example-cw"></a>

The following policy allows a user to access all CloudWatch actions, CloudWatch Logs actions, Amazon SNS actions, and read\-only access to Amazon EC2 Auto Scaling\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "autoscaling:Describe*",
        "cloudwatch:*",
        "logs:*",
        "sns:*"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

### Example 2: Allow Read\-Only Access to CloudWatch<a name="read-only-access-example-cw"></a>

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

### Example 3: Stop or Terminate an Amazon EC2 Instance<a name="stop-terminate-example-cw"></a>

The following policy allows an CloudWatch alarm action to stop or terminate an EC2 instance\. In the sample below, the GetMetricStatistics, ListMetrics, and DescribeAlarms actions are optional\. It is recommended that you include these actions to ensure that you have correctly stopped or terminated the instance\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement": [
 4.     {
 5.       "Action": [
 6.         "cloudwatch:PutMetricAlarm",
 7.         "cloudwatch:GetMetricStatistics",
 8.         "cloudwatch:ListMetrics",
 9.         "cloudwatch:DescribeAlarms"
10.       ],
11.       "Sid": "00000000000000",
12.       "Resource": [
13.         "*"
14.       ],
15.       "Effect": "Allow"
16.     },
17.     {
18.       "Action": [
19.         "ec2:DescribeInstanceStatus",
20.         "ec2:DescribeInstances",
21.         "ec2:StopInstances",
22.         "ec2:TerminateInstances"
23.       ],
24.       "Sid": "00000000000000",
25.       "Resource": [
26.         "*"
27.       ],
28.       "Effect": "Allow"
29.     }
30.   ]
31. }
```