# Getting Started with Amazon CloudWatch Application Insights for \.NET and SQL Server<a name="appinsights-getting-started"></a>

To get started on CloudWatch Application Insights for \.NET and SQL Server, verify that you have met the prerequisites outlined below and have created an IAM policy\. Then, you can get started using the console link to enable CloudWatch Application Insights for \.NET and SQL Server\. To configure your application resources, follow the steps under [Setting Up Your Application](appinsights-setting-up.md)\.

**Topics**
+ [Accessing CloudWatch Application Insights for \.NET and SQL Server](#appinsights-accessing)
+ [Prerequisites](#appinsights-prereqs)
+ [IAM Policy](#appinsights-iam)
+ [Setting Up Your Application](appinsights-setting-up.md)

## Accessing CloudWatch Application Insights for \.NET and SQL Server<a name="appinsights-accessing"></a>

If you have access to CloudWatch Application Insights for \.NET and SQL Server, you can manage it through one of the following interfaces:
+ **CloudWatch console**: To add monitors for your application, choose **Settings** in the left navigation pane of the [CloudWatch console](http://console.aws.amazon.com/cloudwatch)\. From the **Settings** page, choose **Application Insights for \.NET and SQL Server**\. After your application is configured, you can use the [ CloudWatch console](https://console.aws.amazon.com/cloudwatch) to view and analyze problems that are detected\.
+ **AWS Command Line Interface \(AWS CLI\)**: You can use the AWS CLI to access AWS API operations\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) in the *AWS Command Line Interface User Guide*\. For the API reference, see the Amazon CloudWatch Application Insights for \.NET and SQL Server API Reference\. 

## Prerequisites<a name="appinsights-prereqs"></a>

You must complete the following prerequisites to configure an application with CloudWatch Application Insights:
+ **AWS Systems Manager enablement: **You must install Systems Manager Agent \(SSM Agent\), and your instances must be SSM enabled\. For steps on how to install the SSM Agent, see [Setting Up AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html)\.
+ **EC2 Instance Role: **You must attach the `AmazonEC2RoleforSSM` to enable Systems Manager \(see [Using Identity\-based Policies \(IAM Policies\) for AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html)\) and the `CloudWatchAgentServerPolicy` to enable instance metrics and logs to be emitted through CloudWatch\. See [Create IAM Roles and Users for Use With CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html) for more information\.
+ **AWS Resource Group: **To onboard your \.NET and SQL Server applications to CloudWatch Application Insights for \.NET and SQL Server, you must create a Resource Group that includes all associated AWS resources used by your application stack\. This includes application load balancers, EC2 instances running IIS and web front‐end, \.NET worker tiers, and your SQL Server database\. CloudWatch Application Insights for \.NET and SQL Server automatically includes Auto Scaling groups using the same tags or CloudFormation stacks as your Resource Group, because Auto Scaling groups are not currently supported by Resource Groups\. For more information, see [Getting Started with AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted.html)\.
+ **IAM permissions: **For non‐Admin users, you must create an AWS Identity and Access Management \(IAM\) Policy and attach it to your user identity\. See [IAM Policy](#appinsights-iam)\.
+ **Service\-linked role: **CloudWatch Application Insights for \.NET and SQL Server uses AWS Identity and Access Management \(IAM\) service‐linked roles\. You don’t need to manually create a service‐linked role\. Instead, it is created for you when you create your first CloudWatch Application Insights for \.NET and SQL Server application in the AWS Management Console\. For more information, see [Using Service\-Linked Roles for CloudWatch Application Insights for \.NET and SQL Server](CHAP_using-service-linked-roles-appinsights.md)\.

## IAM Policy<a name="appinsights-iam"></a>

To use CloudWatch Application Insights for \.NET and SQL Server, you must create an [Identity and Access Management \(IAM\) policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) and attach it to your IAM user identity\. The IAM policy defines the user permissions\.

**To create an IAM policy using the console**  
To create an IAM policy using the IAM console, follow these steps\.

1. Go to the [IAM console](https://console.aws.amazon.com/iam/home)\. In the left navigation pane, select **Policies**\.

1. At the top of the page, select **Create policy**\.

1. Select the **JSON** tab\.

1. Copy and paste the following JSON document under the **JSON** tab\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": [
                   "applicationinsights:*",
                   "iam:CreateServiceLinkedRole",
                   "iam:ListRoles"
               ],
               "Effect": "Allow",
               "Resource": "*"
           }
       ]
   }
   ```

1. Select **Review Policy**\.

1. Enter a **Name** for the policy, for example, “AppInsightsPolicy\.” Optionally, enter a **Description**\.

1. Select **Create Policy**\.

1. Select **Users** from the left navigation pane\.

1. Select the **User name** of the user to which you would like to attach the policy\.

1. Select **Add permissions**\.

1. Select **Attach existing policies directly**\.

1. Search for the policy that you just created, and select the check box to the left of the policy name\.

1. Select **Next: Review**\.

1. Make sure that the correct policy is listed, and select **Add permissions**\.

1. Make sure that you log in with the user associated with the policy that you just created when you use CloudWatch Application Insights for \.NET and SQL Server\.

**To create an IAM policy using the AWS CLI**  
To create an IAM policy using the AWS CLI, run the [create\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/create-policy.html) operation from the command line using the JSON document above as a file in your current folder\. 

**To create an IAM policy using AWS Tools for Windows PowerShell**  
To create an IAM policy using the AWS Tools for Windows PowerShell, run the [New\-IAMPolicy](https://docs.aws.amazon.com/powershell/latest/reference/items/New-IAMPolicy.html) cmdlt using the JSON document above as a file in your current folder\. 