# Using service\-linked roles for CloudWatch Application Insights<a name="CHAP_using-service-linked-roles-appinsights"></a>

CloudWatch Application Insights uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to CloudWatch Application Insights\. Service\-linked roles are predefined by CloudWatch Application Insights and include all of the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up CloudWatch Application Insights easier because you don’t have to manually add the necessary permissions\. CloudWatch Application Insights defines the permissions of its service\-linked roles, and unless defined otherwise, only CloudWatch Application Insights can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes** in the **Service\-Linked Role** column\. Choose a **Yes** link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for CloudWatch Application Insights<a name="service-linked-role-permissions"></a>

CloudWatch Application Insights uses the service\-linked role named **AWSServiceRoleForApplicationInsights**\. Application Insights uses this role to perform operations such as analyzing the Resource Groups of the customer, creating CloudFormation stacks to create alarms on metrics, and configuring the CloudWatch Agent on EC2 instances\.

The role permissions policy allows CloudWatch Application Insights to complete the following actions on all resources:
+ `cloudwatch:DescribeAlarmHistory`
+  `cloudwatch:DescribeAlarms`
+  `cloudwatch:GetMetricData`
+ `cloudwatch:ListMetrics`
+ `cloudwatch:PutMetricAlarm`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:PutAnomalyDetector`
+ `cloudwatch:DeleteAnomalyDetector`
+ `cloudwatch:DescribeAnomalyDetectors`
+ `logs:FilterLogEvents`
+ `logs:GetLogEvents` 
+  `logs:DescribeLogStreams`
+  `logs:DescribeLogGroups`
+ `events:DescribeRule` 
+ `cloudFormation:DescribeStacks` 
+ `cloudFormation:ListStackResources` 
+ `tag:GetResources` 
+ `resource-groups:ListGroupResources` 
+ `resource-groups:GetGroupQuery` 
+ `resource-groups:GetGroup` 
+ `elasticloadbalancing:DescribeLoadBalancers` 
+ `elasticloadbalancing:DescribeTargetGroups` 
+ `elasticloadbalancing:DescribeTargetHealth` 
+ `autoscaling:DescribeAutoScalingGroups` 
+ `ssm:GetOpsItem` 
+ `ssm:CreateOpsItem` 
+ `ssm:DescribeOpsItems` 
+ `ssm:UpdateOpsItem` 
+ `ssm:DescribeInstanceInformation` 
+ `ssm:ListCommandInvocations` 
+ `ec2:DescribeInstances` 
+ `ec2:DescribeVolumes` 
+ `ec2:DescribeVolumeStatus` 
+ `rds:DescribeDBInstances` 
+ `rds:DescribeDBClusters` 
+ `lambda:GetFunctionConfiguration` 
+ `lambda:ListEventSourceMappings` 
+ `xray:GetServiceGraph` 
+ `xray:GetTraceSummaries` 
+ `xray:GetTimeSeriesServiceStatistics` 
+ `xray:GetTraceGraph` 
+ `dynamodb:DescribeTable` 
+ `dynamodb:DescribeContributorInsights` 
+ `dynamodb:DescribeTimeToLive` 
+ `application-autoscaling:DescribeScalableTargets` 
+ `s3:GetMetricsConfiguration` 
+ `s3:GetReplicationConfiguration` 
+ `states:DescribeExecution` 
+ `states:DescribeStateMachine` 
+ `states:GetExecutionHistory` 
+ `apigateway:GET` 

The role permissions policy allows CloudWatch Application Insights to complete the following actions on specified resources\. These policies are used to manage resources created by or related to CloudWatch Application Insights in your account\.

```
[
  {
    "Effect": "Allow",
    "Action": [
      "cloudFormation:CreateStack",
      "cloudFormation:UpdateStack",
      "cloudFormation:DeleteStack",
      "cloudFormation:DescribeStackResources"
    ],
    "Resource": [
      "arn:aws:cloudformation:*:*:stack/ApplicationInsights-*"
    ]
  },
  {
    "Effect": "Allow",
    "Action": [
      "events:PutRule",
      "events:PutTargets",
      "events:RemoveTargets",
      "events:DeleteRule"
    ],
    "Resource": [
      "arn:aws:events:*:*:rule/AmazonCloudWatch-ApplicationInsights-*"
    ]
  },
  {
    "Effect": "Allow",
    "Action": "ssm:SendCommand",
    "Resource": [
      "arn:aws:ec2:*:*:instance/*",
      "arn:aws:ssm:*:*:document/AWSEC2-CheckPerformanceCounterSets",
      "arn:aws:ssm:*:*:document/AWS-ConfigureAWSPackage",
      "arn:aws:ssm:*:*:document/AWSEC2-DetectWorkload"
    ]
  },
  {
    "Effect": "Allow",
    "Action": [
      "ssm:PutParameter",
      "ssm:DeleteParameter",
      "ssm:AddTagsToResource",
      "ssm:RemoveTagsFromResource",
      "ssm:GetParameters"
    ],
    "Resource": "arn:aws:ssm:*:*:parameter/AmazonCloudWatch-ApplicationInsights-*"
  },
  {
    "Effect": "Allow",
    "Action": [
      "ssm:CreateAssociation",
      "ssm:UpdateAssociation",
      "ssm:DeleteAssociation",
      "ssm:DescribeAssociation"
    ],
    "Resource": [
      "arn:aws:ec2:*:*:instance/*",
      "arn:aws:ssm:*:*:association/*",
      "arn:aws:ssm:*:*:managed-instance/*",
      "arn:aws:ssm:*:*:document/AWSEC2-ApplicationInsightsCloudwatchAgentInstallAndConfigure",
      "arn:aws:ssm:*:*:document/AWS-ConfigureAWSPackage",
      "arn:aws:ssm:*:*:document/AmazonCloudWatch-ManageAgent"
    ]
  },
  {
    "Effect": "Allow",
    "Action": "ssm:AddTagsToResource",
    "Resource": "arn:aws:ssm:*:*:opsitem/*"
  }
]
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for CloudWatch Application Insights<a name="create-service-linked-role"></a>

You don't need to manually create a service\-linked role\. When you create a new Application Insights application in the AWS Management Console, CloudWatch Application Insights creates the service\-linked role for you\. 

If you delete this service\-linked role, and then want to create it again, you can use the same process to recreate the role in your account\. When you create a new Application Insights application, CloudWatch Application Insights creates the service\-linked role for you again\. 

## Editing a service\-linked role for CloudWatch Application Insights<a name="edit-slr"></a>

CloudWatch Application Insights does not allow you to edit the AWSServiceRoleForApplicationInsights service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for CloudWatch Application Insights<a name="delete-service-linked-role"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you avoid having an unused entity that is not actively monitored or maintained\. However, you must delete all applications in Application Insights before you can manually delete the role\.

**Note**  
If the CloudWatch Application Insights service is using the role when you try to delete the resources, the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete CloudWatch Application Insights resources used by the AWSServiceRoleForApplicationInsights**
+ Delete all of your CloudWatch Application Insights applications\. For more information, see "Deleting Your Application\(s\)" in the CloudWatch Application Insights User Guide\. 

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForApplicationInsights service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for CloudWatch Application Insights service\-linked roles<a name="slr-regions"></a>

CloudWatch Application Insights supports using service\-linked roles in all of the AWS Regions where the service is available\.


****  

| Region name | Region identity | Support in CloudWatch Application Insights | 
| --- | --- | --- | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | 
| Asia Pacific \(Hong Kong\) | ap\-east\-1 | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | 
| Europe \(Paris\) | eu\-west\-3 | Yes | 
| Europe \(Stockholm\) | eu\-north\-1 | Yes | 
| Europe \(Milan\) | eu\-south\-1 | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | 
| US East \(Ohio\) | us\-east\-2 | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | 
| Africa \(Cape Town\) | af\-south\-1 | Yes | 
| Middle East \(Bahrain\) | me\-south\-1 | Yes | 
| AWS GovCloud \(US\-East\) | us\-gov\-east\-1 | Yes | 
| AWS GovCloud \(US\-West\) | us\-gov\-west\-1 | Yes | 