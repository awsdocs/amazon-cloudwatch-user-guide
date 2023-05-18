# Using a service\-linked role for Amazon CloudWatch Internet Monitor<a name="using-service-linked-roles-CWIM"></a>

Amazon CloudWatch Internet Monitor uses an AWS Identity and Access Management \(IAM\)[ service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Internet Monitor\. The service\-linked role is predefined by Internet Monitor and includes all the permissions that the service requires to call other AWS services on your behalf\. 

Internet Monitor defines the permissions of the service\-linked role, and unless defined otherwise, only Internet Monitor can assume the role\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete the role only after first deleting its related resources\. This restriction protects your Internet Monitor resources because you can't inadvertently remove permissions to access the resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes** in the **Service\-linked role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Internet Monitor<a name="service-linked-role-permissions-CWIM"></a>

Internet Monitor uses the service\-linked role named **AWSServiceRoleForInternetMonitor**\. This role allows Internet Monitor to access resources in your account, such as Amazon Virtual Private Cloud resources, Amazon CloudFront distributions, and Amazon WorkSpaces directories, so that you can select them when you create a monitor\.

The **AWSServiceRoleForInternetMonitor** service\-linked role trusts the service to assume the role:
+ `internetmonitor.amazonaws.com`

The **AWSServiceRoleForInternetMonitor** service\-linked role has an IAM policy attached named **CloudWatchInternetMonitorServiceRolePolicy**\. This policy grants permission to Internet Monitor to access AWS services on your behalf\. It includes permissions that allow Internet Monitor to complete the following actions:

```
{
    "Version": "2012-10-17"
    "Statement": [
        {
            "Action": [
                "ec2:DescribeVpcs",
                "workspaces:DescribeWorkspaceDirectories",
                "cloudfront:GetDistribution"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "logs:CreateLogGroup",
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:log-group:/aws/internet-monitor/*"
        },
        {
            "Action": [
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:logs:*:*:log-group:/aws/internet-monitor/*:log-stream:*"
        },
        {
            "Action": "cloudwatch:PutMetricData",
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "cloudwatch:namespace": "AWS/InternetMonitor"
                }
            },
            "Resource": "*"
        }
    ],
}
```

## Creating a service\-linked role for Internet Monitor<a name="create-service-linked-role-CWIM"></a>

You do not need to manually create the service\-linked role for Internet Monitor\. The first time that you create a monitor, Internet Monitor creates **AWSServiceRoleForInternetMonitor** for you\.

For more information, see [Creating a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.

## Editing a service\-linked role for Internet Monitor<a name="edit-service-linked-role-CWIM"></a>

After Internet Monitor creates a service\-linked role in your account, you cannot change the name of the role because various entities might reference the role\. You can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Internet Monitor<a name="delete-service-linked-role-CWIM"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete the role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the resources for the service\-linked role before you can manually delete it\.

After you've removed your resources from your monitors in Internet Monitor and then deleted the monitors, you can delete the service\-linked role **AWSServiceRoleForInternetMonitor**\.

**Note**  
If the Internet Monitor service is using the role when you try to delete it, then the deletion might fail\. If that happens, wait for a few minutes and then try again\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the **AWSServiceRoleForInternetMonitor** service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Updates to the Internet Monitor service\-linked role<a name="security-iam-awsmanpol-updates-cwim"></a>

For updates to **AWSServiceRoleForInternetMonitor**, the AWS managed policy for the Internet Monitor service\-linked role, see the [AWS managed policies updates table](auth-and-access-control-cw.md#security-iam-awsmanpol-updates)\. For automatic alerts about changes to this page, subscribe to the RSS feed on the CloudWatch Document history page\.