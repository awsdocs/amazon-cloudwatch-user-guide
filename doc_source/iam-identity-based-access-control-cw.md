# Using identity\-based policies \(IAM policies\) for CloudWatch<a name="iam-identity-based-access-control-cw"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on CloudWatch resources\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your CloudWatch resources\. For more information, see [Access control](auth-and-access-control-cw.md#access-control-cw)\. 

The sections in this topic cover the following:
+ [Permissions required to use the CloudWatch console](#console-permissions-cw)
+ [AWS managed \(predefined\) policies for CloudWatch](#managed-policies-cloudwatch)
+ [Customer managed policy examples](#customer-managed-policies-cw)

The following shows an example of a permissions policy\.

```
 1. {
 2.   "Version": "2012-10-17",
 3.   "Statement":[{
 4.       "Effect":"Allow",
 5.       "Action":["cloudwatch:GetMetricData","cloudwatch:ListMetrics"],
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

This sample policy has one statement that grants permissions to a group for two CloudWatch actions \(`cloudwatch:GetMetricData`, and `cloudwatch:ListMetrics`\), but only if the group uses SSL with the request \(`"aws:SecureTransport":"true"`\)\. For more information about the elements within an IAM policy statement, see [Specifying policy elements: Actions, effects, and principals](iam-access-control-overview-cw.md#actions-effects-principals-cw) and [IAM Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html) in *IAM User Guide*\.

## Permissions required to use the CloudWatch console<a name="console-permissions-cw"></a>

For a user to work with the CloudWatch console, that user must have a minimum set of permissions that allow the user to describe other AWS resources in their account\. The CloudWatch console requires permissions from the following services:
+ Amazon EC2 Auto Scaling
+ CloudTrail
+ CloudWatch
+ CloudWatch Events
+ CloudWatch Logs
+ Amazon EC2
+ OpenSearch Service
+ IAM
+ Kinesis
+ Lambda
+ Amazon S3
+ Amazon SNS
+ Amazon SQS
+ Amazon SWF
+ X\-Ray, if you are using the ServiceLens feature

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can still use the CloudWatch console, also attach the `CloudWatchReadOnlyAccess` managed policy to the user, as described in [AWS managed \(predefined\) policies for CloudWatch](#managed-policies-cloudwatch)\.

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

## AWS managed \(predefined\) policies for CloudWatch<a name="managed-policies-cloudwatch"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. 

The following AWS managed policies, which you can attach to users in your account, are specific to CloudWatch\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for CloudWatch actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

**Topics**
+ [CloudWatchFullAccess](#managed-policies-cloudwatch-CloudWatchFullAccess)
+ [CloudWatchActionsEC2Access](#managed-policies-cloudwatch-CloudWatchActionsEC2Access)
+ [CloudWatchReadOnlyAccess](#managed-policies-cloudwatch-CloudWatchReadOnlyAccess)
+ [CloudWatchAutomaticDashboardsAccess](#managed-policies-cloudwatch-CloudWatch-CloudWatchAutomaticDashboardsAccess)
+ [CloudWatchAgentServerPolicy](#managed-policies-cloudwatch-CloudWatchAgentServerPolicy)
+ [CloudWatchAgentAdminPolicy](#managed-policies-cloudwatch-CloudWatchAgentAdminPolicy)
+ [AWS managed \(predefined\) policies for CloudWatch Synthetics](#managed-policies-cloudwatch-canaries)
+ [AWS managed \(predefined\) policies for Amazon CloudWatch RUM](#managed-policies-cloudwatch-RUM)
+ [AWS managed \(predefined\) policies for CloudWatch Evidently](#managed-policies-cloudwatch-evidently)
+ [AWS managed policy for AWS Systems Manager Incident Manager](#managed-policies-cloudwatch-incident-manager)

### CloudWatchFullAccess<a name="managed-policies-cloudwatch-CloudWatchFullAccess"></a>

The **CloudWatchFullAccess** policy grants full access to all CloudWatch actions and resources\. Its contents are as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "autoscaling:Describe*",
                "cloudwatch:*",
                "logs:*",
                "sns:*",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:GetRole"
            ],
            "Effect": "Allow",
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

### CloudWatchReadOnlyAccess<a name="managed-policies-cloudwatch-CloudWatchReadOnlyAccess"></a>

The **CloudWatchReadOnlyAccess** policy grants read\-only access to CloudWatch\.

The following is the content of the **CloudWatchReadOnlyAccess** policy\.

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
                "logs:List*",
                "logs:StartQuery",
                "logs:StopQuery",
                "logs:Describe*",
                "logs:TestMetricFilter",
                "logs:FilterLogEvents",
                "sns:Get*",
                "sns:List*"
            ],
            "Effect": "Allow",
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

### AWS managed \(predefined\) policies for CloudWatch Synthetics<a name="managed-policies-cloudwatch-canaries"></a>

The **CloudWatchSyntheticsFullAccess** and **CloudWatchSyntheticsReadOnlyAccess** AWS managed policies are available for you to assign to users who will manage or use CloudWatch Synthetics\. The following additional policies are also relevant:
+ **AmazonS3ReadOnlyAccess** and **CloudWatchReadOnlyAccess** – These are necessary to be able to read all Synthetics data in the CloudWatch console\.
+ **AWSLambdaReadOnlyAccess** – To be able to view the source code used by canaries\.
+ **CloudWatchSyntheticsFullAccess** enables you to create canaries, Additionally, to create a canary that will have a new IAM role created for it, you also need the following inline policy statement:

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
                "rum:Get*",
                "rum:List*"
            ],
           "Resource": "*"
        }
    ]
}
```

#### AmazonCloudWatchRUMServiceRolePolicy<a name="managed-policies-AmazonCloudWatchRUMServiceRolePolicy"></a>

You can't attach **AmazonCloudWatchRUMServiceRolePolicy** to your IAM entities\. This policy is attached to a service\-linked role that allows CloudWatch RUM to publish monitoring data to other relevant AWS services\. For more information about this service linked role, see [Using service\-linked roles for CloudWatch RUM](using-service-linked-roles-RUM.md)\.

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
                "evidently:ListExperiments",
                "evidently:ListFeatures",
                "evidently:ListLaunches",
                "evidently:ListProjects"
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
                "iam:GetRole"
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
                "lambda:GetFunctionConfiguration"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:cwsyn-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:GetLayerVersion",
                "lambda:PublishLayerVersion"
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
                "synthetics:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

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

## CloudWatch updates to AWS managed policies<a name="iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for CloudWatch since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the CloudWatch Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AmazonCloudWatchRUMFullAccess](#managed-policies-CloudWatchRUMFullAccess) – New policy  |  CloudWatch added a new policy to enable full management of CloudWatch RUM\. CloudWatch RUM allows you to perform real user monitoring of your web application\. For more information, see [Use CloudWatch RUM](CloudWatch-RUM.md)\.  | November 29, 2021 | 
|  [AmazonCloudWatchRUMReadOnlyAccess](#managed-policies-CloudWatchRUMReadOnlyAccess) – New policy  |  CloudWatch added a new policy to enable read\-only access to CloudWatch RUM\. CloudWatch RUM allows you to perform real user monitoring of your web application\. For more information, see [Use CloudWatch RUM](CloudWatch-RUM.md)\.  | November 29, 2021 | 
|  [CloudWatchEvidentlyFullAccess](#managed-policies-CloudWatchEvidentlyFullAccess) – New policy  |  CloudWatch added a new policy to enable full management of CloudWatch Evidently\. CloudWatch Evidently allows you to perform A/B experiments of your web applications, and to roll them out gradually\. For more information, see [Perform launches and A/B experiments with CloudWatch Evidently](CloudWatch-Evidently.md)\.  | November 29, 2021 | 
|  [CloudWatchEvidentlyReadOnlyAccess](#managed-policies-CloudWatchEvidentlyReadOnlyAccess) – New policy  |  CloudWatch added a new policy to enable read\-only access to CloudWatch Evidently\. CloudWatch Evidently allows you to perform A/B experiments of your web applications, and to roll them out gradually\. For more information, see [Perform launches and A/B experiments with CloudWatch Evidently](CloudWatch-Evidently.md)\.  | November 29, 2021 | 
|  [ **AWSServiceRoleForCloudWatchRUM**](using-service-linked-roles-RUM.md) – New managed policy  |  CloudWatch added a policy for a new service\-linked role to allow CloudWatch RUM to pubish monitoring data to other relevant AWS services\.  | November 29, 2021 | 
|  [CloudWatchSyntheticsFullAccess](#managed-policies-cloudwatch-CloudWatchSyntheticsFullAccess) – Update to an existing policy  |  CloudWatch Synthetics added permissions to **CloudWatchSyntheticsFullAccess**, and also changed the scope of one permission\. The `kms:ListAliases` permission was added so that users can list available AWS KMS keys that can be used to encrypt canary artifacts\. The `kms:DescribeKey` permission was added so that users can see the details of keys that will be used to encrypt for canary artifacts\. And the `kms:Decrypt` permission was added to enable users to decrypt canary artifacts\. This decryption ability is limited to use on resources within Amazon S3 buckets\. The `Resource` scope of the `s3:GetBucketLocation` permission was changed from `*` to `arn:aws:s3:::*`\.  | September 29, 2021 | 
|  [CloudWatchSyntheticsFullAccess](#managed-policies-cloudwatch-CloudWatchSyntheticsFullAccess) – Update to an existing policy  |  CloudWatch Synthetics added a permission to **CloudWatchSyntheticsFullAccess**\. The `lambda:UpdateFunctionCode` permission was added so that users with this policy can change the runtime version of canaries\.  | July 20, 2021 | 
|  [ AWSCloudWatchAlarms\_ActionSSMIncidentsServiceRolePolicy](#managed-policies-cloudwatch-incident-manager) – New managed policy  |  CloudWatch added a new managed IAM policy to allow CloudWatch to create incidents in AWS Systems Manager Incident Manager\.  | May 10, 2021 | 
|  [ CloudWatchAutomaticDashboardsAccess](#managed-policies-cloudwatch-CloudWatch-CloudWatchAutomaticDashboardsAccess) – Update to an existing policy  |  CloudWatch added a permission to the **CloudWatchAutomaticDashboardsAccess** managed policy\. The `synthetics:DescribeCanariesLastRun` permission was added to this policy to enable cross\-account dashboard users to see details about CloudWatch Synthetics canary runs\.  | April 20, 2021 | 
|  CloudWatch started tracking changes  |  CloudWatch started tracking changes for its AWS managed policies\.  | April 14, 2021 | 