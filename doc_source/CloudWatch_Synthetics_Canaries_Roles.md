# Necessary Roles and Permissions for CloudWatch Canaries<a name="CloudWatch_Synthetics_Canaries_Roles"></a>


****  

|  | 
| --- |
| CloudWatch Synthetics is in open preview\. The preview is currently available in the following Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries_Roles.html) The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [cw\-synthetics@amazon\.com](mailto:cw-synthetics@amazon.com) | 

To view canary details and the results of canary test runs, you need to be logged on as an IAM user that has either the **CloudWatchSyntheticsFullAccess** or the **CloudWatchSyntheticsReadOnlyAccess** attached\. To read all Synthetics data in the console, you also need the **AmazonS3ReadOnlyAccess**, **CloudWatchReadOnlyAccess** policies\. To be able to view the source code used by canaries, you also need the **AWSLambdaReadOnlyAccess** policy\.

To be able to create canaries, you must be logged on as an IAM user that has the **CloudWatchSyntheticsFullAccess** policy or a similar set of permissions\. Additionally, if the canary you create will have a new IAM role created for it, you also need the following inline policy statement:

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

**CloudWatchSyntheticsFullAccess**

The contents of the **CloudWatchSyntheticsFullAccess** policy are as follows:

```
{ 
   "Version":"2012-10-17",
   "Statement":[ 
      { 
         "Effect":"Allow",
         "Action":[ 
            "synthetics:*"
         ],
         "Resource":"*"
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "s3:CreateBucket",
            "s3:PutBucketEncryption",
            "s3:PutEncryptionConfiguration"
         ],
         "Resource":[ 
            "arn:aws:s3:::cw-syn-results-*"
         ]
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "iam:ListRoles",
            "s3:ListAllMyBuckets",
            "s3:GetBucketLocation"
         ],
         "Resource":"*"
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "s3:GetObject",
            "s3:ListBucket"
         ],
         "Resource":"arn:aws:s3:::cw-syn-*"
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "s3:GetObjectVersion"
         ],
         "Resource":"arn:aws:s3:::aws-synthetics-library-*"
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "iam:PassRole"
         ],
         "Resource":[ 
            "arn:aws:iam::*:role/service-role/CloudWatchSyntheticsRole*"
         ],
         "Condition":{ 
            "StringEquals":{ 
               "iam:PassedToService":[ 
                  "lambda.amazonaws.com",
                  "synthetics.amazonaws.com"
               ]
            }
         }
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "iam:GetRole"
         ],
         "Resource":[ 
         "arn:aws:iam::*:role/service-role/CloudWatchSyntheticsRole*"
         ]
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "cloudwatch:GetMetricData",
            "cloudwatch:GetMetricStatistics"
         ],
         "Resource":"*"
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "cloudwatch:PutMetricAlarm",
            "cloudwatch:DeleteAlarms"
         ],
         "Resource":[ 
            "arn:aws:cloudwatch:*:*:alarm:Synthetics-*"
         ]
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "cloudwatch:DescribeAlarms"
         ],
         "Resource":[ 
            "arn:aws:cloudwatch:*:*:alarm:*"
         ]
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "lambda:CreateFunction",
            "lambda:AddPermission",
            "lambda:PublishVersion",
            "lambda:UpdateFunctionConfiguration"
         ],
         "Resource":[ 
            "arn:aws:lambda:*:*:function:cwsyn-*"
         ]
      },
      { 
         "Effect":"Allow",
         "Action":[ 
            "lambda:GetLayerVersionByArn",
            "lambda:GetLayerVersion",
            "lambda:PublishLayerVersion"
         ],
         "Resource":[ 
            "arn:aws:lambda:*:*:layer:cwsyn-*",
            "arn:aws:lambda:*:*:layer:Synthetics:*"
         ]
      }
   ]
}
```

**CloudWatchSyntheticsReadOnlyAccess**

The contents of the **CloudWatchSyntheticsReadOnlyAccess** policy are as follows:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "synthetics:Describe*",
                "synthetics:Get*"
            ],
            "Resource": "*"
        }
    ]
}
```