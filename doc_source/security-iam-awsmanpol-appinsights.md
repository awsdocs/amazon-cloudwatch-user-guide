# AWS managed policies for Amazon CloudWatch Application Insights<a name="security-iam-awsmanpol-appinsights"></a>







To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ViewOnlyAccess** AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.







**Topics**
+ [AWS managed policy: CloudWatchApplicationInsightsFullAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsFullAccess)
+ [AWS managed policy: CloudWatchApplicationInsightsReadOnlyAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsReadOnlyAccess)
+ [AWS managed policy: CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy)
+ [Application Insights updates to AWS managed policies](#security-iam-awsmanpol-appinsights-updates)



**Topics**
+ [AWS managed policy: CloudWatchApplicationInsightsFullAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsFullAccess)
+ [AWS managed policy: CloudWatchApplicationInsightsReadOnlyAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsReadOnlyAccess)
+ [AWS managed policy: CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy)
+ [Application Insights updates to AWS managed policies](#security-iam-awsmanpol-appinsights-updates)

## AWS managed policy: CloudWatchApplicationInsightsFullAccess<a name="security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsFullAccess"></a>





You can attach the `CloudWatchApplicationInsightsFullAccess` policy to your IAM identities\.







This policy grants administrative permissions that allow full access to Application Insights functionality\.



**Permissions details**

This policy includes the following permissions\.




+ `applicationinsights` – Allows full access to Application Insights functionality\.
+ `iam` – Allows Application Insights to create the service\-linked role, AWSServiceRoleForApplicationInsights\. This is required so that Application Insights can perform operations such as analyze the resource groups of a customer, create CloudFormation stacks to create alarms on metrics, and configure the CloudWatch Agent on EC2 instances\. For more information, see [Using service\-linked roles for CloudWatch Application Insights](CHAP_using-service-linked-roles-appinsights.md)\.



```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "applicationinsights:*",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeVolumes",
        "rds:DescribeDBInstances",
        "rds:DescribeDBClusters",
        "sqs:ListQueues",
        "elasticloadbalancing:DescribeLoadBalancers",
        "elasticloadbalancing:DescribeTargetGroups",
        "elasticloadbalancing:DescribeTargetHealth",
        "autoscaling:DescribeAutoScalingGroups",
        "lambda:ListFunctions",
        "dynamodb:ListTables",
        "s3:ListAllMyBuckets",
        "sns:ListTopics",
        "states:ListStateMachines",
        "apigateway:GET",
        "ecs:ListClusters",
        "ecs:DescribeTaskDefinition",
        "ecs:ListServices",
        "ecs:ListTasks",
        "eks:ListClusters",
        "eks:ListNodegroups",
        "fsx:DescribeFileSystems",
        "logs:DescribeLogGroups"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole"
      ],
      "Resource": [
        "arn:aws:iam::*:role/aws-service-role/application-insights.amazonaws.com/AWSServiceRoleForApplicationInsights"
      ],
      "Condition": {
        "StringEquals": {
          "iam:AWSServiceName": "application-insights.amazonaws.com"
        }
      }
    }
  ]
}
```

## AWS managed policy: CloudWatchApplicationInsightsReadOnlyAccess<a name="security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsReadOnlyAccess"></a>





You can attach the `CloudWatchApplicationInsightsReadOnlyAccess` policy to your IAM identities\.



This policy grants administrative permissions that allow read\-only access to all Application Insights functionality\.



**Permissions details**

This policy includes the following permissions\.




+ `applicationinsights` – Allows read\-only access to Application Insights functionality\.



```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "applicationinsights:Describe*",
                "applicationinsights:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: CloudwatchApplicationInsightsServiceLinkedRolePolicy<a name="security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy"></a>







You can't attach CloudwatchApplicationInsightsServiceLinkedRolePolicy to your IAM entities\. This policy is attached to a service\-linked role that allows Application Insights to monitor customer resources\. For more information, see [Using service\-linked roles for CloudWatch Application Insights](CHAP_using-service-linked-roles-appinsights.md)\.





## Application Insights updates to AWS managed policies<a name="security-iam-awsmanpol-appinsights-updates"></a>



View details about updates to AWS managed policies for Application Insights since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Application Insights [Document history](DocumentHistory.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [CloudWatchApplicationInsightsFullAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsFullAccess) – Update to an existing policy  |  Application Insights added a new permission to describe log groups\. This permissions is required for Amazon CloudWatch Application Insights to ensure that the correct permissions for monitoring log groups are in an account when creating a new application\.  | January 24, 2022 | 
|  [CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy) – Update to an existing policy  |  Application Insights added new permissions to create and delete CloudWatch Log Subscription Filters\. These permissions are required for Amazon CloudWatch Application Insights to create Subscription Filters to facilitate log monitoring of resources within configured applications\.   | January 24, 2022 | 
|  [CloudWatchApplicationInsightsFullAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsFullAccess) – Update to an existing policy  |  Application Insights added new permissions to describe target groups and target health for Elastic Load Balancers\. These permissions are required for Amazon CloudWatch Application Insights to create account\-based applications by querying all of the supported resources in an account\.  | November 4, 2021 | 
|  [CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy) – Update to an existing policy  |  Application Insights added new permissions to run the `AmazonCloudWatch-ManageAgent` SSM document on Amazon EC2 instances\. This permissions is required for Amazon CloudWatch Application Insights to clean up CloudWatch agent configuration files created by Application Insights\.  | September 30, 2021 | 
|  [CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy) – Update to an existing policy  |  Application Insights added new permissions to support account\-based application monitoring to onboard and monitor all supported resources in your account\. These permissions are required for Amazon CloudWatch Application Insights to query, tag resources, and create groups for these resources\. Application Insights added new permissions to support monitoring of SNS topics\.  These permissions are required for Amazon CloudWatch Application Insights to gather metadata from SNS resources to configure monitoring for SNS topics\.  | September 15, 2021 | 
|  [CloudWatchApplicationInsightsFullAccess](#security-iam-awsmanpol-appinsights-CloudWatchApplicationInsightsFullAccess) – Update to an existing policy  |  Application Insights added new permissions to describe and list supported resources\. These permissions are required for Amazon CloudWatch Application Insights to create account\-based applications by querying all of the supported resources in an account\.  | September 15, 2021 | 
|  [CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy) – Update to an existing policy  |  Application Insights added new permissions to describe FSx resources\. These permissions are required for Amazon CloudWatch Application Insights to read customer FSx resource configurations, and to help customers automatically set up best practice FSx monitoring with CloudWatch\.  | August 31, 2021 | 
|  [CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy) – Update to an existing policy  |  Application Insights added new permissions to describe and list ECS and EKS service resources\. This permission is required for Amazon CloudWatch Application Insights to read customer container resources configuration, and to help customers automatically set up best practice container monitoring with CloudWatch\.  | May 18, 2021 | 
|  [CloudwatchApplicationInsightsServiceLinkedRolePolicy](#security-iam-awsmanpol-appinsights-CloudwatchApplicationInsightsServiceLinkedRolePolicy) – Update to an existing policy  |  Application Insights added new permissions to allow OpsCenter to tag OpsItems using the `ssm:AddTagsToResource` action on resources with the `opsitem` resource type\. This permission is required by OpsCenter\. Amazon CloudWatch Application Insights creates OpsItems so that the customer can resolve problems using [AWS SSM OpsCenter](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html)\.  | April 13, 2021 | 
|  Application Insights started tracking changes  |  Application Insights started tracking changes for its AWS managed policies\.  | April 13, 2021 | 