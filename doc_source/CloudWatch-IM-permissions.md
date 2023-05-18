# IAM permissions for Amazon CloudWatch Internet Monitor<a name="CloudWatch-IM-permissions"></a>

To use Amazon CloudWatch Internet Monitor, users must have the correct permissions\.

For more information about security in Internet Monitor and Amazon CloudWatch, see [Identity and access management for Amazon CloudWatch](auth-and-access-control-cw.md)\.

## Permissions required to view a monitor<a name="CloudWatch-IM-permissions.ViewMonitor"></a>

To view a monitor for Amazon CloudWatch Internet Monitor in the AWS Management Console, you must be signed in as a user or role that has the following permissions:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "internetmonitor:Get*",
                "internetmonitor:List*",
                "logs:DescribeLogGroups",
                "logs:GetQueryResults",
                "logs:StartQuery",
                "logs:StopQuery",
                "cloudwatch:GetMetricData"
                ],
            "Resource": "*"
        }
    ]
}
```

## Permissions required to create a monitor<a name="CloudWatch-IM-permissions.CreateMonitor"></a>

To create a monitor in Amazon CloudWatch Internet Monitor, users must have permission to create a service\-linked role that is associated with Internet Monitor\. To learn more about the Internet Monitor service\-linked role, see [Using a service\-linked role for Amazon CloudWatch Internet Monitor](using-service-linked-roles-CWIM.md)\.

To create a monitor for Amazon CloudWatch Internet Monitor in the AWS Management Console, you must be signed in as a user or role that has the permissions included in the following policy\.

**Note**  
If you create an identity\-based permissions policy that is more restrictive, users with that policy won't be able to create a monitor\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "internetmonitor:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/internetmonitor.amazonaws.com/AWSServiceRoleForInternetMonitor",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName": "internetmonitor.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:AttachRolePolicy",
                "iam:PutRolePolicy"
            ],
            "Resource": "arn:aws:iam::*:role/aws-service-role/internetmonitor.amazonaws.com/AWSServiceRoleForInternetMonitor"
        },
        {
            "Action": [
                "ec2:DescribeVpcs",
                "workspaces:DescribeWorkspaceDirectories",
                "cloudfront:GetDistribution"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```