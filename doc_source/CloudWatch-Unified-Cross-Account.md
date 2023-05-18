# CloudWatch cross\-account observability<a name="CloudWatch-Unified-Cross-Account"></a>

With Amazon CloudWatch cross\-account observability, you can monitor and troubleshoot applications that span multiple accounts within a Region\. Seamlessly search, visualize, and analyze your metrics, logs, and traces in any of the linked accounts without account boundaries\.

Set up one or more AWS accounts as *monitoring accounts* and link them with multiple *source accounts*\. A monitoring account is a central AWS account that can view and interact with observability data generated from source accounts\. A source account is an individual AWS account that generates observability data for the resources that reside in it\. Source accounts share their observability data with the monitoring account\. The shared observability data can include the following types of telemetry:
+ Metrics in Amazon CloudWatch
+ Log groups in Amazon CloudWatch Logs
+ Traces in AWS X\-Ray

To create links between monitoring accounts and source accounts, you can use the CloudWatch console\. Alternatively, use the *Observability Access Manager* commands in the AWS CLI and API\. For more information, see [Observability Access Manager API Reference](https://docs.aws.amazon.com/OAM/latest/APIReference/Welcome.html)\.

A *sink* is a resource that represents an attachment point in a monitoring account\. Source accounts can link to the sink to share observability data\. Each sink is managed by the monitoring account where it is located\. An *observability link* is a resource that represents the link established between a source account and a monitoring account\. Links are managed by the source account\.

For a video demonstration of setting up CloudWatch cross\-account observability, see the following video\.



The next topic explains how to set up CloudWatch cross\-account observability in both monitoring accounts and source accounts\. For information about the cross\-account cross\-Region CloudWatch dashboard, see [Cross\-account cross\-Region CloudWatch console](Cross-Account-Cross-Region.md)\.

**Use Organizations for source accounts**

There are two options for linking source accounts to your monitoring account\. You can use one or both options\.
+ Use AWS Organizations to link accounts in an organization or organizational unit to the monitoring account\. 
+ Connect individual AWS accounts to the monitoring account\.

We recommend that you use Organizations so that new AWS accounts created later in the organization are automatically onboarded to cross\-account observability as source accounts\. 

**Details about linking monitoring accounts and source accounts**
+ Each monitoring account can be linked to as many as 100,000 source accounts\.
+ Each source account can share data with as many as five monitoring accounts\.
+ You can set up a single account as both a monitoring account and a source account\. If you do, this account sends only the observability data from itself to the its linked monitoring account\. It does not relay the data from its source accounts\.
+ A monitoring account specifies which telemetry types can be shared with it\. A source account specifies which telemetry types it wants to share\.
  + If there are more telemetry types selected in the *monitoring account* than in the source account, the accounts are linked\. Only the data types that are selected in both accounts are shared\.
  + If there are more telemetry types selected in the *source account* than in the monitoring account, the link creation fails and nothing is shared\.
+ To remove a link between accounts, do so from the source account\.
+ To delete the sink in a monitoring account, you must first remove all links to the monitoring account\.

**Pricing**

Cross\-account observability in CloudWatch comes with no extra cost for logs and metrics, and the first trace copy is free\. For more information about pricing, see [Amazon CloudWatch Pricing](http://aws.amazon.com/cloudwatch/pricing)\.

**Contents**
+ [Link monitoring accounts with source accounts](CloudWatch-Unified-Cross-Account-Setup.md)
  + [Necessary permissions](CloudWatch-Unified-Cross-Account-Setup.md#CloudWatch-Unified-Cross-Account-Setup-permissions)
  + [Setup overview](CloudWatch-Unified-Cross-Account-Setup.md#CloudWatch-Unified-Cross-Account-Setup-overview)
  + [Step 1: Set up a monitoring account](CloudWatch-Unified-Cross-Account-Setup.md#Unified-Cross-Account-Setup-ConfigureMonitoringAccount)
  + [Step 2: \(Optional\) Download an AWS CloudFormation template or URL](CloudWatch-Unified-Cross-Account-Setup.md#Unified-Cross-Account-Setup-TemplateOrURL)
  + [Step 3: Link the source accounts](CloudWatch-Unified-Cross-Account-Setup.md#Unified-Cross-Account-Setup-ConfigureSourceAccount)
    + [Use an AWS CloudFormation template to set up all accounts in an organization or an organizational unit as source accounts](CloudWatch-Unified-Cross-Account-Setup.md#Unified-Cross-Account-SetupSource-OrgTemplate)
    + [Use an AWS CloudFormation template to set up individual source accounts](CloudWatch-Unified-Cross-Account-Setup.md#Unified-Cross-Account-SetupSource-SingleTemplate)
    + [Use a URL to set up individual source accounts](CloudWatch-Unified-Cross-Account-Setup.md#Unified-Cross-Account-SetupSource-SingleURL)
+ [Manage monitoring accounts and source accounts](Unified-Cross-Account-Manage.md)
  + [Link more source accounts to an existing monitoring account](Unified-Cross-Account-Manage.md#Unified-Cross-Account-Setup-AddSourceAccounts)
  + [Remove the link between a monitoring account and source account](Unified-Cross-Account-Manage.md#Unified-Cross-Account-Setup-UnlinkAccount)
  + [View information about a monitoring account](Unified-Cross-Account-Manage.md#Unified-Cross-Account-Setup-ManageMonitoringAccount)