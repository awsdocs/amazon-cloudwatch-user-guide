# Sharing CloudWatch dashboards<a name="cloudwatch-dashboard-sharing"></a>

You can share your CloudWatch dashboards with people who do not have direct access to your AWS account\. This enables you to share dashboards across teams, with stakeholders, and with people external to your organization\. You can even display dashboards on big screens in team areas, or embed them in Wikis and other webpages\.

**Warning**  
All people who you share the dashboard with are granted the permissions listed in [Permissions that are granted to people who you share the dashboard with](#share-cloudwatch-dashboard-iamrole) for the account\. If you share the dashboard publicly, then everyone who has the link to the dashboard has these permissions\.  
The `cloudwatch:GetMetricData` and `ec2:DescribeTags` permissions cannot be scoped down to specific metrics or EC2 instances, so the people with access to the dashboard can query all CloudWatch metrics and the names and tags of all EC2 instances in the account\.

When you share dashboards, you can designate who can view the dashboard in three ways:
+ Share a single dashboard and designate specific email addresses of the people who can view the dashboard\. Each of these users creates their own password that they must enter to view the dashboard\.
+ Share a single dashboard publicly, so that anyone who has the link can view the dashboard\.
+ Share all the CloudWatch dashboards in your account and specify a third\-party single sign\-on \(SSO\) provider for dashboard access\. All users who are members of this SSO provider's list can access all the dashboards in the account\. To enable this, you integrate the SSO provider with Amazon Cognito\. The SSO provider must support Security Assertion Markup Language \(SAML\)\. For more information about Amazon Cognito, see [ What is Amazon Cognito?](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html)

**Important**  
Do not modify resource names and identifiers that are created by the dashboard sharing process\. This includes Amazon Cognito and IAM resources\. Modifying these resources can cause unexpected and incorrect functionality of shared dashboards\.

**Note**  
If you share a dashboard that has metric widgets with alarm annotations, the people that you share the dashboard with will not see those widgets\. They will instead see a blank widget with text saying that the widget is not available\. You will still see metric widgets with alarm annotations when you view the dashboard yourself\.

## Permissions required to share a dashboard<a name="share-cloudwatch-permissions-required"></a>

To be able to share dashboards using any of the following methods and to see which dashboards have already been shared, you must be signed on as a user or with an IAM role that has certain permissions\.

To be able to share dashboards, your user or IAM role must include the permissions included in the following policy statement:

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
        "arn:aws:iam::*:role/service-role/CWDBSharing*",
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
},
{
    "Effect": "Allow",
    "Action": [
        "cloudwatch:GetDashboard",
    ],
    "Resource": [
        "*"
        // or the ARNs of dashboards that you want to share
    ]
}
```

To be able to see which dashboards are shared, but not be able to share dashboards, a user or an IAM role can include a policy statement similar to the following:

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
},
{
    "Effect": "Allow",
    "Action": [
        "cloudwatch:ListDashboards",
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
The `cloudwatch:GetMetricData` and `ec2:DescribeTags` permissions cannot be scoped down to specific metrics or EC2 instances, so the people with access to the dashboard can query all CloudWatch metrics and the names and tags of all EC2 instances in the account\.

When you share a dashboard, by default the permissions that CloudWatch creates restrict access to only the alarms and Contributor Insights rules that are on the dashboard when it is shared\. If you add new alarms or Contributor Insights rules to the dashboard and want them to also be seen by the people who you shared the dashboard with, you must update the policy to allow these resources\.

## Share a single dashboard with specific users<a name="share-cloudwatch-dashboard-email-addresses"></a>

Use the steps in this section to share a dashboard with specific email addresses that you choose\.

**Note**  
By default, any CloudWatch Logs widgets on the dashboard are not visible to people who you share the dashboard with\. For more information, see [Allowing people that you share with to see logs table widgets](#share-cloudwatch-dashboard-logwidget)\.  
By default, any composite alarm widgets on the dashboard are not visible to people who you share the dashboard with\. For more information, see [Allowing people that you share with to see composite alarms](#share-cloudwatch-dashboard-composite-alarms)\.

**To share a dashboard with specific users**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of your dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Next to **Share your dashboard and require a username and password**, choose **Start sharing**\.

1. Under **Add email addresses**, enter the email addresses that you want to share the dashboard with\.

1. When you have all the email addresses entered, read the agreement and select the confirmation box\. Then choose **Preview policy**\.

1. Confirm that the resources that will be shared are what you want, and choose **Confirm and generate shareable link**\.

1. On the next page, choose **Copy link to clipboard**\. You can then paste this link into email and send it to the invited users\. They automatically receive a separate email with their user name and a temporary password to use to connect to the dashboard\.

## Share a single dashboard publicly<a name="share-cloudwatch-dashboard-public"></a>

Follow the steps in this section to share a dashboard publicly\. This can be useful to display the dashboard on a big screen in a team room, or embed it in a Wiki page\.

**Important**  
Sharing a dashboard publicly makes it accessible to anyone who has the link, with no authentication\. Do this only for dashboards that do not contain sensitive information\.

**Note**  
By default, any CloudWatch Logs widgets on the dashboard are not visible to people who you share the dashboard with\. For more information, see [Allowing people that you share with to see logs table widgets](#share-cloudwatch-dashboard-logwidget)\.  
By default, any composite alarm widgets on the dashboard are not visible to people who you share the dashboard with\. For more information, see [Allowing people that you share with to see composite alarms](#share-cloudwatch-dashboard-composite-alarms)\.

**To share a dashboard publicly**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of your dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Next to **Share your dashboard publicly**, choose **Start sharing**\.

1. Enter **Confirm** in the text box\.

1. Read the agreement and select the confirmation box\. Then choose **Preview policy**\.

1. Confirm that the resources that will be shared are what you want, and choose **Confirm and generate shareable link**\.

1. On the next page, choose **Copy link to clipboard**\. You can then share this link\. Anyone you share the link with can access the dashboard, without providing credentials\.

## Share all CloudWatch dashboards in the account by using SSO<a name="share-cloudwatch-dashboard-sso"></a>

Use the steps in this section to share all the dashboards in your account with users by using single sign\-on \(SSO\)\.

**Note**  
By default, any CloudWatch Logs widgets on the dashboard are not visible to people who you share the dashboard with\. For more information, see [Allowing people that you share with to see logs table widgets](#share-cloudwatch-dashboard-logwidget)\.  
By default, any composite alarm widgets on the dashboard are not visible to people who you share the dashboard with\. For more information, see [Allowing people that you share with to see composite alarms](#share-cloudwatch-dashboard-composite-alarms)\.

**To share your CloudWatch dashboards with users who are in an SSO provider's list**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of your dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Choose **Go to CloudWatch Settings**\.

1. If the SSO provider that you want isn't listed in **Available SSO providers**, choose **Manage SSO providers** and follow the instructions in [Set up SSO for CloudWatch dashboard sharing](#share-cloudwatch-dashboards-setup-SSO)\.

   Then return to the CloudWatch console and refresh the browser\. The SSO provider that you enabled should now appear in the list\.

1. Choose the SSO provider that you want in the **Available SSO providers** list\.

1. Choose **Save changes**\.

## Set up SSO for CloudWatch dashboard sharing<a name="share-cloudwatch-dashboards-setup-SSO"></a>

To set up dashboard sharing through a third\-party single sign\-on provider that supports SAML, follow these steps\. 

**Important**  
We strongly recommend that you do not share dashboards using a non\-SAML SSO provider\. Doing so causes a risk of inadvertently allowing third parties to access your account's dashboards\.

**To set up an SSO provider to enable dashboard sharing**

1. Integrate the SSO provider with Amazon Cognito\. For more information, see [ Integrating Third\-Party SAML Identity Providers with Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-pools-integrating-3rd-party-saml-providers.html)\.

1. Download the metadata XML file from your SSO provider\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Settings**\.

1. In the **Dashboard sharing** section, choose **Configure**\.

1. Choose **Manage SSO providers**\.

   This opens the Amazon Cognito console in the US East \(N\. Virginia\) Region \(us\-east\-1\)\. If you don't see any **User Pools**, the Amazon Cognito console might have opened in a different Region\. If so, change the Region to **US East \(N\. Virginia\) us\-east\-1** and proceed with the next steps\.

1. Choose the **CloudWatchDashboardSharing** pool\.

1. In the navigation pane, choose **Identity providers**\.

1. Choose **SAML**\.

1. Enter a name for your SSO provider in **Provider name**\.

1. Choose **Select file**, and select the metadata XML file that you downloaded in step 1\.

1. Choose **Create provider**\.

## See how many of your dashboards are shared<a name="share-cloudwatch-dashboard-how-many"></a>

You can use the CloudWatch console to see how many of your CloudWatch dashboards are currently being shared with others\.

**To see how many of your dashboards are being shared**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Settings**\.

1. The **Dashboard sharing** section displays how many dashboards are shared\.

1. To see which dashboards are shared, choose ***number* dashboards shared** under **Username and password** and under **Public dashboards\.**

## See which of your dashboards are shared<a name="share-cloudwatch-dashboard-list"></a>

You can use the CloudWatch console to see which of your dashboards are currently being shared with others\.

**To see which of your dashboards are being shared**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. In the list of dashboards, see the **Share** column\. Dashboards that have the icon in this column filled in are currently being shared\.

1. To see which users a dashboard is being shared with, choose the dashboard name, and then choose **Actions**, **Share dashboard**\.

   The **Share dashboard *dashboard name*** page displays how the dashboard is being shared\. If you want, you can stop sharing the dashboard by choosing **Stop sharing**\.

## Stop sharing one or more dashboards<a name="share-cloudwatch-dashboard-stop"></a>

You can stop sharing a single shared dashboard, or stop sharing all shared dashboards at once\.

**To stop sharing a single dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the shared dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Choose **Stop sharing**\.

1. In the confirmation box, choose **Stop sharing**\.

**To stop sharing all shared dashboards**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Settings**\.

1. In the **Dashboard sharing** section, choose **Stop sharing all dashboards**\.

1. In the confirmation box, choose **Stop sharing all dashboards**\.

## Review shared dashboard permissions and change permission scope<a name="share-cloudwatch-dashboard-review-permissions"></a>

Use the steps in this section if you want to review the permissions of the users of your shared dashboards, or change the scope of shared dashboard permissions\.

**To review shared dashboard permissions**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the shared dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Under **Resources**, choose **IAM Role**\.

1. In the IAM console, choose the displayed policy\.

1. \(Optional\) To limit which alarms that shared dashboard users can see, choose **Edit policy** and move the `cloudwatch:DescribeAlarms` permission from its current position to a new `Allow` statement that lists the ARNs of only the alarms that you want to be seen by shared dashboard users\. See the following example\.

   ```
   {
      "Effect": "Allow",
       "Action": "cloudwatch:DescribeAlarms",
       "Resource": [
           "AlarmARN1",
           "AlarmARN2"
       ]
   }
   ```

   If you do this, be sure to remove the `cloudwatch:DescribeAlarms` permission from a section of the current policy that looks like this:

   ```
   { 
      "Effect": "Allow",
       "Action": [
           "cloudwatch:GetInsightRuleReport",
           "cloudwatch:GetMetricData",
           "cloudwatch:DescribeAlarms",
           "ec2:DescribeTags"
       ],
       "Resource": "*"
   }
   ```

1. \(Optional\) To limit the scope of what Contributor Insights rules that shared dashboard users can see, choose **Edit policy** and move the `cloudwatch:GetInsightRuleReport` from its current position to a new `Allow` statement that lists the ARNs of only the Contributor Insights rules that you want to be seen by shared dashboard users\. See the following example\.

   ```
   {
      "Effect": "Allow",
       "Action": "cloudwatch:GetInsightRuleReport",
       "Resource": [
           "PublicContributorInsightsRuleARN1",
           "PublicContributorInsightsRuleARN2"
       ]
   }
   ```

   If you do this, be sure to remove `cloudwatch:GetInsightRuleReport` from a section of the current policy that looks like this:

   ```
   {
               "Effect": "Allow",
               "Action": [
                   "cloudwatch:GetInsightRuleReport",
                   "cloudwatch:GetMetricData",
                   "cloudwatch:DescribeAlarms",
                   "ec2:DescribeTags"
               ],
               "Resource": "*"
           }
   ```

## Allowing people that you share with to see composite alarms<a name="share-cloudwatch-dashboard-composite-alarms"></a>

When you share a dashboard, by default the composite alarm widgets on the dashboard are not visible to the people who you share the dashboard with\. For composite alarm widgets to be visible, you need to add a `DescribeAlarms: *` permission to the dashboard sharing policy\. That permission would look like this:

```
{
    "Effect": "Allow",
    "Action": "cloudwatch:DescribeAlarms",
    "Resource": "*"
}
```

**Warning**  
The preceding policy statement give access to all alarms in the account\. To reduce the scope of `cloudwatch:DescribeAlarms`, you must use a `Deny` statement\. You can add a `Deny` statement to the policy and specify the ARNs of the alarms that you want to lock down\. That deny statement should look similar to the following:  

```
{
    "Effect": "Allow",
    "Action": "cloudwatch:DescribeAlarms",
    "Resource": "*"
},
{   
    "Effect": "Deny",
    "Action": "cloudwatch:DescribeAlarms",
    "Resource": [
        "SensitiveAlarm1ARN",
        "SensitiveAlarm1ARN"
    ]
}
```

## Allowing people that you share with to see logs table widgets<a name="share-cloudwatch-dashboard-logwidget"></a>

When you share a dashboard, by default the CloudWatch Logs Insights widgets that are on the dashboard are not visible to the people who you share the dashboard with\. This affects both CloudWatch Logs Insights widgets that exist now and any that are added to the dashboard after you share it\.

If you want these people to be able to see CloudWatch Logs widgets, you must add permissions to the IAM role for dashboard sharing\. 

**To allow the people that you share a dashboard with to see the CloudWatch Logs widgets**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the shared dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Under **Resources**, choose **IAM Role**\.

1. In the IAM console, choose the displayed policy\. 

1. Choose **Edit policy** and add the following statement\. In the new statement, we recommend that you specify the ARNs of only the log groups that you want shared\. See the following example\.

   ```
   {
               "Effect": "Allow",
               "Action": [
                   "logs:FilterLogEvents",
                   "logs:StartQuery",
                   "logs:StopQuery",
                   "logs:GetLogRecord"
               ],
               "Resource": [
                   "SharedLogGroup1ARN",
                   "SharedLogGroup2ARN"
              ]
           },
   ```

1. Choose **Save Changes**\.

If your IAM policy for dashboard sharing already includes those four permissions with `*` as the resource, we strongly recommend that you change the policy and specify only the ARNs of the log groups that you want shared\. For example, if your `Resource` section for these permissions was the following:

```
"Resource": "*"
```

Change the policy to specify only the ARNs of the log groups that you want shared, as in the following example:

```
"Resource": [
  "SharedLogGroup1ARN",
  "SharedLogGroup2ARN"
]
```

## Allowing people that you share with to see custom widgets<a name="share-cloudwatch-dashboard-customwidget"></a>

When you share a dashboard, by default the custom widgets that are on the dashboard are not visible to the people who you share the dashboard with\. This affects both custom widgets that exist now and any that are added to the dashboard after you share it\.

If you want these people to be able to see custom widgets, you must add permissions to the IAM role for dashboard sharing\. 

**To allow the people that you share a dashboard with to see the custom widgets**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the shared dashboard\.

1. Choose **Actions**, **Share dashboard**\.

1. Under **Resources**, choose **IAM Role**\.

1. In the IAM console, choose the displayed policy\. 

1. Choose **Edit policy** and add the following statement\. In the new statement, we recommend that you specify the ARNs of only the Lambda functions that you want shared\. See the following example\.

   ```
   {
     "Sid": "Invoke",
     "Effect": "Allow",
     "Action": [
         "lambda:InvokeFunction"
     ],
     "Resource": [
       "LambdaFunction1ARN",
       "LambdaFunction2ARN"
     ]
    }
   ```

1. Choose **Save Changes**\.

If your IAM policy for dashboard sharing already includes that permission with `*` as the resource, we strongly recommend that you change the policy and specify only the ARNs of the Lambda functions that you want shared\. For example, if your `Resource` section for these permissions was the following:

```
"Resource": "*"
```

Change the policy to specify only the ARNs of the custom widgets that you want shared, as in the following example:

```
"Resource": [
  "LambdaFunction1ARN",
  "LambdaFunction2ARN"
]
```