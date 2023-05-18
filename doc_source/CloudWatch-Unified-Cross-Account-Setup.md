# Link monitoring accounts with source accounts<a name="CloudWatch-Unified-Cross-Account-Setup"></a>

The topics in this section explain how to set up links between monitoring accounts and source accounts\.

We recommend that you create a new AWS account to serve as the monitoring account for your organization\.

**Contents**
+ [Necessary permissions](#CloudWatch-Unified-Cross-Account-Setup-permissions)
+ [Setup overview](#CloudWatch-Unified-Cross-Account-Setup-overview)
+ [Step 1: Set up a monitoring account](#Unified-Cross-Account-Setup-ConfigureMonitoringAccount)
+ [Step 2: \(Optional\) Download an AWS CloudFormation template or URL](#Unified-Cross-Account-Setup-TemplateOrURL)
+ [Step 3: Link the source accounts](#Unified-Cross-Account-Setup-ConfigureSourceAccount)
  + [Use an AWS CloudFormation template to set up all accounts in an organization or an organizational unit as source accounts](#Unified-Cross-Account-SetupSource-OrgTemplate)
  + [Use an AWS CloudFormation template to set up individual source accounts](#Unified-Cross-Account-SetupSource-SingleTemplate)
  + [Use a URL to set up individual source accounts](#Unified-Cross-Account-SetupSource-SingleURL)

## Necessary permissions<a name="CloudWatch-Unified-Cross-Account-Setup-permissions"></a>

To create links between a monitoring account and a source account, you must be signed in with certain permissions\. 
+ **To set up a monitoring account** – You must have either full administrator access in the monitoring account, or you must sign in to that account with the following permissions:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AllowSinkModification",
              "Effect": "Allow",
              "Action": [
                  "oam:CreateSink",
                  "oam:DeleteSink",
                  "oam:PutSinkPolicy",
                  "oam:TagResource"
              ],
              "Resource": "*"
          },
          {
              "Sid": "AllowReadOnly",
              "Effect": "Allow",
              "Action": ["oam:Get*", "oam:List*"],
              "Resource": "*"
          }
      ]
  }
  ```
+ **Source account, scoped to a specific monitoring account** – To create, update, and manage links for just one specified monitoring account, you must sign in to account with at least the following permissions\. In this example, the monitoring account is `999999999999`\.

  If the link will not share all three resource types \(metrics, logs, and traces\), you can omit `cloudwatch:Link`, `logs:Link`, or `xray:Link` as needed\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Action": [
                  "oam:CreateLink",
                  "oam:UpdateLink",
                  "oam:DeleteLink",
                  "oam:GetLink",
                  "oam:TagResource"
              ],
              "Effect": "Allow",
              "Resource": "arn:*:oam:*:*:link/*"
          },
          {
              "Action": [
                  "oam:CreateLink",
                  "oam:UpdateLink"
              ],
              "Effect": "Allow",
              "Resource": "arn:*:oam:*:*:sink/*",
              "Condition": {
                  "StringEquals": {
                      "aws:ResourceAccount": [
                          "999999999999"
                      ]
                  }
              }
          },
          {
              "Action": "oam:ListLinks",
              "Effect": "Allow",
              "Resource": "*"
          },
          {
              "Action": "cloudwatch:Link",
              "Effect": "Allow",
              "Resource": "*"
          },
          {
              "Action": "logs:Link",
              "Effect": "Allow",
              "Resource": "*"
          },
          {
              "Action": "xray:Link",
              "Effect": "Allow",
              "Resource": "*"
          }
      ]
  }
  ```
+ **Source account, with permissions to link to any monitoring account** – To create a link to any existing monitoring account sink and share metrics, log groups, and traces, you must sign in to the source account with full administrator permissions or sign in there with the following permissions

  If the link will not share all three resource types \(metrics, logs, and traces\), you can omit `cloudwatch:Link`, `logs:Link`, or `xray:Link` as needed\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [{
              "Effect": "Allow",
              "Action": [
                  "oam:CreateLink",
                  "oam:UpdateLink"
              ],
              "Resource": [
                  "arn:aws:oam:*:*:link/*",
                  "arn:aws:oam:*:*:sink/*"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "oam:List*",
                  "oam:Get*"
              ],
              "Resource": "*"
          },
          {
              "Effect": "Allow",
              "Action": [
                  "oam:DeleteLink",
                  "oam:GetLink",
                  "oam:TagResource"
              ],
              "Resource": "arn:aws:oam:*:*:link/*"
          },
          {
              "Action": "cloudwatch:Link",
              "Effect": "Allow",
              "Resource": "*"
          },
          {
              "Action": "logs:Link",
              "Effect": "Allow",
              "Resource": "*"
          },
          {
              "Action": "xray:Link",
              "Effect": "Allow",
              "Resource": "*"
          }
      ]
  }
  ```

## Setup overview<a name="CloudWatch-Unified-Cross-Account-Setup-overview"></a>

The following high\-level steps show you how to set up CloudWatch cross\-account observability\.

**Note**  
We recommend creating a new AWS account to use as your organization's monitoring account\.

1. Set up a dedicated monitoring account\.

1. \(Optional\) Download an AWS CloudFormation template or copy a URL to link source accounts\.

1. Link source accounts to the monitoring account\.

After completing these steps, you can use the monitoring account to view the observability data of the source accounts\.

## Step 1: Set up a monitoring account<a name="Unified-Cross-Account-Setup-ConfigureMonitoringAccount"></a>

Follow the steps in this section to set up an AWS account as a monitoring account for CloudWatch cross\-account observability\.

**Prerequisites**
+ **If you're setting up accounts in an AWS Organizations organization as the source accounts** – Get the organization path or organization ID\.
+ **If you're not using Organizations for the source accounts** – Get the account IDs of the source accounts\.

To set up an account as a monitoring account, you must have certain permissions\. For more information, see [Necessary permissions](#CloudWatch-Unified-Cross-Account-Setup-permissions)\.

**To set up a monitoring account**

1. Sign in to the account that you want to use as a monitoring account\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Settings**\.

1. By **Monitoring account configuration**, choose **Configure**\.

1. For **Select data**, choose whether this monitoring account will be able to view **Logs**, **Metrics**, and **Traces** data from the source accounts it is linked to\.

1. For **List source accounts**, enter the source accounts that this monitoring account will view\. To identify the source accounts, enter individual account IDs, organization paths, or organization IDs\. If you enter an organization path or organization ID, this monitoring account is allowed to view observability data from all linked accounts in that organization\.

   Separate the entries in this list with commas\.

1. For **Define a label to identify your source account**, specify whether to use account names or email addresses to identify the source accounts when you use the monitoring account to view them\.

1. Choose **Configure**\.

**Important**  
The link between the monitoring and source accounts is not complete until you configure the source accounts\. For more information, see the following sections\.

## Step 2: \(Optional\) Download an AWS CloudFormation template or URL<a name="Unified-Cross-Account-Setup-TemplateOrURL"></a>

To link source accounts to a monitoring account, we recommend using an AWS CloudFormation template or a URL\. 
+ **If you are linking an entire organization** – CloudWatch provides an AWS CloudFormation template\.
+ **If you are linking individual accounts** – Use either an AWS CloudFormation template or a URL that CloudWatch provides\.

To use an AWS CloudFormation template, you must download it during these steps\. After you link the monitoring account with at least one source account, the AWS CloudFormation template is no longer available to download\.

**To download an AWS CloudFormation template or copy a URL for linking source accounts to the monitoring account**

1. Sign in to the account that you want to use as a monitoring account\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Settings**\.

1. By **Monitoring account configuration**, choose **Resources to link accounts**\.

1. Do one of the following:
   + Choose **AWS organization** to get a template to use to link accounts in an organization to this monitoring account\.
   + Choose **Any account** to get a template or URL for setting up individual accounts as source accounts\.

1. Do one of the following:
   + If you chose **AWS organization**, choose **Download CloudFormation template**\. 
   + If you chose **Any account**, choose either **Download CloudFormation template** or **Copy URL**\.

1. \(Optional\) Repeat steps 5\-6 to download both the AWS CloudFormation template and the URL\.

## Step 3: Link the source accounts<a name="Unified-Cross-Account-Setup-ConfigureSourceAccount"></a>

Use the steps in these sections to link source accounts to a monitoring account\.

To link monitoring accounts with source accounts, you must have certain permissions\. For more information, see [Necessary permissions](#CloudWatch-Unified-Cross-Account-Setup-permissions)\.

### Use an AWS CloudFormation template to set up all accounts in an organization or an organizational unit as source accounts<a name="Unified-Cross-Account-SetupSource-OrgTemplate"></a>

These steps assume that you already downloaded the necessary AWS CloudFormation template by performing the steps in [Step 2: \(Optional\) Download an AWS CloudFormation template or URL](#Unified-Cross-Account-Setup-TemplateOrURL)\.

**To use an AWS CloudFormation template to link accounts in an organization or organizational unit to the monitoring account**

1. Sign in to the organization's management account\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the left navigation bar, choose **StackSets**\.

1. Check that you are signed in to the Region that you want, then choose **Create StackSet**\.

1. Choose **Next**\.

1. Choose **Template is ready** and choose **Upload a template file**\.

1. Choose **Choose file**, choose the template that you downloaded from the monitoring account, and choose **Open**\.

1. Choose **Next**\.

1. For **Specify StackSet details**, enter a name for the StackSet and choose **Next**\.

1. For **Add stacks to stack set**, choose **Deploy new stacks**\. 

1. For **Deployment targets**, choose whether to deploy to the entire organization or to specified organizational units\.

1. For **Specify regions**, choose which Regions to deploy CloudWatch cross\-account observability to\.

1. Choose **Next**\.

1. On the **Review** page, confirm your selected options and choose **Submit**\.

1. In the **Stack instances** tab, refresh the screen until you see that your stack instances have the status **CREATE\_COMPLETE**\.

### Use an AWS CloudFormation template to set up individual source accounts<a name="Unified-Cross-Account-SetupSource-SingleTemplate"></a>

These steps assume that you already downloaded the necessary AWS CloudFormation template by performing the steps in [Step 2: \(Optional\) Download an AWS CloudFormation template or URL](#Unified-Cross-Account-Setup-TemplateOrURL)\.

**To use an AWS CloudFormation template to set up individual source accounts for CloudWatch cross\-account observability**

1. Sign in to the source account\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the left navigation bar, choose **Stacks**\.

1. Check that you are signed in to the Region that you want, then choose **Create stack**, **With new resources \(standard\)**\.

1. Choose **Next**\.

1. Choose **Upload a template file**\.

1. Choose **Choose file**, choose the template that you downloaded from the monitoring account, and choose **Open**\.

1. Choose **Next**\.

1. For **Specify stack details**, enter a name for the stack and choose **Next**\.

1. On the **Configure stack options** page, choose **Next**\.

1. On the **Review** page, choose **Submit**\.

1. On the status page for your stack, refresh the screen until you see that your stack has the status **CREATE\_COMPLETE**\.

1. To use this same template to link more source accounts to this monitoring account, sign out of this account and sign in to the next source account\. Then repeat steps 2\-12\.

### Use a URL to set up individual source accounts<a name="Unified-Cross-Account-SetupSource-SingleURL"></a>

These steps assume that you already copied the necessary URL by performing the steps in [Step 2: \(Optional\) Download an AWS CloudFormation template or URL](#Unified-Cross-Account-Setup-TemplateOrURL)\.

**To use a URL to link individual source accounts to the monitoring account**

1. Sign in to the account that you want to use as a source account\.

1. Enter the URL that you copied from the monitoring account\.

   You see the CloudWatch settings page, with some information filled in\.

1. For **Select data**, choose whether this source account will share **Logs**, **Metrics**, and **Traces** data to this monitoring account\.

1. Do not change the ARN in **Enter monitoring account configuration ARN**\.

1. The **Define a label to identify your source account** section is pre\-filled with the label choice from the monitoring account\. Optionally, choose **Edit** to change it\.

1. Choose **Link**\.

1. Enter **Confirm** in the box and choose **Confirm**\.

1. To use this same URL to link more source accounts to this monitoring account, sign out of this account and sign in to the next source account\. Then repeat steps 2\-7\.