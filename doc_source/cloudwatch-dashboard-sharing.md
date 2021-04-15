# Sharing CloudWatch Dashboards<a name="cloudwatch-dashboard-sharing"></a>

You can share your CloudWatch dashboards with people who do not have direct access to your AWS account\. This enables you to share dashboards across teams, with stakeholders, and with people external to your organization\. You can even display dashboards on big screens in team areas, or embed them in Wikis and other webpages\.

When you share dashboards, you can designate who can view the dashboard in three ways:
+ Share a single dashboard and designate specific email addresses of the people who can view the dashboard\. Each of these users creates their own password that they must enter to view the dashboard\.
+ Share a single dashboard publicly, so that anyone who has the link can view the dashboard\.
+ Share all the CloudWatch dashboards in your account and specify a third\-party single sign\-on \(SSO\) provider for dashboard access\. All users who are members of this SSO provider's list can access all the dashboards in the account\. To enable this, you integrate the SSO provider with Amazon Cognito\. The SSO provider must support Security Assertion Markup Language \(SAML\)\. For more information about Amazon Cognito, see [ What is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)

## Permissions required for dashboard sharing<a name="share-cloudwatch-dashboard-email-addresses"></a>

To be able to share dashboards using any of the following methods and to see which dashboards have already been shared, you must be logged on to an IAM user or IAM role that has certain permissions\.

To be able to share dashboards, your IAM user or IAM role must include the permissions included in the following policy statement:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:CreateRole",
        "iam:CreatePolicy",
        "iam:AttachRolePolicy",
        "iam:PassRole"
    ],
    "Resource": [
        "arn:aws:iam::*:role/service-role/CloudWatchDashboard*",
        "arn:aws:iam::*:policy/*"
    ]
},
{
    "Effect": "Allow",
    "Action": [
        "cognito-idp:*",
        "cognito-identity:*",
    ],
    "Resource": [
        "*"
    ]
}
```

To be able to see which dashboards are shared, but not be able to share dashboards, your IAM user or IAM role can include a policy statement similar to the following:

```
{
    "Effect": "Allow",
    "Action": [
        "cognito-idp:*",
        "cognito-identity:*"
    ],
    "Resource": [
        "*"
    ]
}
```

## Permissions that are granted to people who you share the dashboard with<a name="share-cloudwatch-dashboard-iamrole"></a>

When you share a dashboard, CloudWatch creates an IAM role in the account which gives the following permissions to the people who you share the dashboard with:
+ `cloudwatch:GetInsightRuleReport`
+ `cloudwatch:GetMetricData`
+ `cloudwatch:DescribeAlarms`
+ `ec2:DescribeTags`

**Warning**  
All people who you share the dashboard with are granted these permissions for the account\. If you share the dashboard publicly, then everyone who has the link to the dashboard has these permissions\.  
The `cloudwatch:GetMetricData` and `ec2:DescribeTags` can't be scoped down to specific metrics or EC2 instances, so the people with access to the dashboard can query all CloudWatch metrics and the names and tags of all EC2 instances in the account\.

For additional security, you can modify the IAM role used for dashboard sharing to narrow the scope of the `cloudwatch:GetInsightRuleReport` and `cloudwatch:DescribeAlarms` permissions\. Typically, you will do this to deny access to alarms or Contributor Insights rules that are not on shared dashboards\. But if you do deny access to a widget that is on a shared dashboard, then that widget will not appear when people that you have shared the dashboard with view the dashboard\.

 To narrow the scope of the IAM role, do the following after the dashboard is shared:

**To modify the IAM role for a shared dashboard to narrow the scope of some permissions**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the shared dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Under **Resources**, choose **IAM Role**\.

1. In the IAM console, choose the displayed policy\. 

1. Do one of the following:
   + Choose **Edit policy** and then add a statement with an **Allow** effect that lists only the ARNs of the alarms and Contributor Insights rules that are to be shared\. See the following example\.

     ```
     {
                 "Effect": "Allow",
                 "Action": [ 
                    "cloudwatch:DescribeAlarms", 
                    "cloudwatch:DescribeInsightRules" 
                 ],
                 "Resource": [
                     "PublicAlarmARN",
                     "PublicContributorInsightsRuleARN"
                 ]
             },
     ```
   + Choose **Edit policy** and then add a statement with a **Deny** effect that lists the ARNs of the alarms and Contributor Insights rules to be locked down\. See the following example\.

     ```
     {
                 "Effect": "Deny",
                 "Action": [ 
                    "cloudwatch:DescribeAlarms", 
                    "cloudwatch:DescribeInsightRules" 
                 ],
                 "Resource": [
                     "SensitiveAlarmARN",
                     "SensitiveContributorInsightsRuleARN"
                 ]
             },
     ```

1. Choose **Save Changes**\.

## Share a single dashboard with specific users<a name="share-cloudwatch-dashboard-email-addresses"></a>

Use the steps in this section to share a dashboard with specific email addresses that you choose\.

**To share a dashboard with specific users**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of your dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Next to **Share your dashboard and require a username and password**, choose **Start sharing**\.

1. Under **Add email addresses**, enter the email addresses that you want to share the dashboard with\.

1. To include CloudWatch Logs Insights widgets as part of the shared dashboard, choose **Enable sharing log widgets**\. If you don't choose this, any CloudWatch Logs widgets on the dashboard are not visible to people who access the dashboard by sharing\. For more information, see [Sharing dashboards with logs table widgets](#share-cloudwatch-dashboard-logwidget)\.

1. When you have all the email addresses entered, read the agreement and select the confirmation box\. Then choose **Save and generate shareable link**\.

1. On the next page, choose **Copy link to clipboard**\. You can then paste this link into email and send it to the invited users\. They automatically receive a separate email with their username and a temporary password to use to connect to the dashboard\.

## Share a single dashboard publicly<a name="share-cloudwatch-dashboard-public"></a>

Use the steps in this section to share a dashboard publicly\. This can be useful to put the dashboard on a big screen in a team room, or embed it in a Wiki page\.

**Important**  
Sharing a dashboard publicly makes it accessible to anyone who has the link, with no authentication\. Do this only for dashboards that do not contain sensitive information\.

**To share a dashboard publicly**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of your dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Next to **Share your dashboard publicly**, choose **Start sharing**\.

1. Enter **Confirm** in the text box\.

1. To include CloudWatch Logs Insights widgets as part of the shared dashboard, choose **Enable sharing log widgets**\. If you don't choose this, any CloudWatch Logs widgets on the dashboard are not visible to people who access the dashboard by sharing\. For more information, see [Sharing dashboards with logs table widgets](#share-cloudwatch-dashboard-logwidget)\.

1. Read the agreement and select the confirmation box\. Then choose **Save and generate shareable link**\.

1. On the next page, choose **Copy link to clipboard**\. You can then share this link\. Anyone you share the link with can access the dashboard, without providing credentials\.

## Share all CloudWatch dashboards in the account by using SSO<a name="share-cloudwatch-dashboard-sso"></a>

Use the steps in this section to share all the dashboards in your account with users by using single sign\-on \(SSO\)\.

**To share your CloudWatch dashboards with users who are in an SSO provider's list**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of your dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Choose **Go to CloudWatch Settings**\.

1. If the SSO provider that you want isn't listed in **Available SSO providers**, choose **Manage SSO providers** and follow the instructions in [Set up SSO for CloudWatch dashboard Sharing](#share-cloudwatch-dashboards-setup-SSO)\.

   Then return to the CloudWatch console and refresh the browser\. The SSO provider that you enabled should now appear in the list\.

1. Choose the SSO provider that you want in the **Available SSO providers** list\.

1. To include CloudWatch Logs Insights widgets as part of the shared dashboard, choose **Enable sharing log widgets**\. If you don't choose this, any CloudWatch Logs widgets on the dashboard are not visible to people who access the dashboard by sharing\. For more information, see [Sharing dashboards with logs table widgets](#share-cloudwatch-dashboard-logwidget)\.

1. Choose **Save changes**\.

## Set up SSO for CloudWatch dashboard Sharing<a name="share-cloudwatch-dashboards-setup-SSO"></a>

To set up dashboard sharing through a third\-party single sign\-on provider that supports SAML, follow these steps\. 

**Important**  
We strongly recommend that you do not share dashboards using a non\-SAML SSO provider\. Doing so causes a risk of inadvertently allowing third\-parties to access your account's dashboards\.

**To set up an SSO provider to enable dashboard sharing**

1. Integrate the SSO provider with Amazon Cognito\. For more information, see [ Integrating Third\-Party SAML Identity Providers with Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-integrating-3rd-party-saml-providers.html)\.

1. Download the metadata XML file from your SSO provider\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Settings**\.

1. In the **Dashboard sharing** section, choose **Configure**\.

1. Choose **Manage SSO providers**\.

   This opens the Amazon Cognito console in N\. Virginia \(us\-east\-1\)\. If you don't see any **User Pools**, the Amazon Cognito console might have opened in a different Region\. If so, change the Region to **US East \(N\. Virginia\) us\-east\-1** and proceed with the next steps\.

1. Choose the **CloudWatchDashboardSharing** pool\.

1. In the navigation pane, choose **Identity providers**\.

1. Choose **SAML**\.

1. Enter a name for your SSO provider in **Provider name**\.

1. Choose **Select file**, and select the metadata XML file that you downloaded in step 1\.

1. Choose **Create provider**\.

## Sharing dashboards with logs table widgets<a name="share-cloudwatch-dashboard-logwidget"></a>

When you share a dashboard, you choose whether to make any CloudWatch Logs Insights widgets on the dashboard visible to people who access the dashboard by the share\. This choice affects both CloudWatch Logs Insights widgets that exist now and any that are added to the dashboard after you share it\.

If you choose to allow the sharing of CloudWatch Logs widgets, CloudWatch grants the following additional permissions to the IAM role for dashboard sharing\. By default, these permissions aply to all log groups in the account\.
+ `logs:FilterLogEvents`
+ `logs:StartQuery`
+ `logs:StopQuery`
+ `logs:GetLogRecord`

For additional security, you can choose not to include CloudWatch Logs widgets in dashboard sharing, or you can modify the IAM role used for dashboard sharing to lock down sensitive log groups while allowing other log groups to be visible in the shared dashboard\. To modify this IAM role, do the following after the dashboard is shared:

**To modify the IAM role for a shared dashboard to lock down specified log groups**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the shared dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Under **Resources**, choose **IAM Role**\.

1. In the IAM console, choose the displayed policy\. 

1. Choose **Edit policy** and then add a statement with a **Deny** effect that lists the ARNs of the log groups to be locked down\. See the following example\.

   ```
   {
               "Effect": "Deny",
               "Action": "*",
               "Resource": [
                   "LogGroup1ARN",
                   "LogGroup2ARN"
               ]
           },
   ```

1. Choose **Save Changes**\.