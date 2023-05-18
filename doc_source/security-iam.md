# Identity and Access Management for Amazon CloudWatch Internet Monitor<a name="security-iam"></a>

AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use Internet Monitor resources\. IAM is an AWS service that you can use with no additional charge\.

**Important**  
**Internet Monitor resource changes on February 24, 2023**  
If you created IAM policies that included Internet Monitor resources before February 24, 2023, be aware of the following changes to Internet Monitor resources and resource types\.  
**HealthEvents** resource was renamed to **HealthEvent**\.
The ARN and Regex formats for the **HealthEvent** resource were updated\.
The ARN and Regex formats for the **Monitor** resource were updated\.
Resource\-level permissions for the **GetHealthEvent** action are now supported only on the **HealthEvent** resource type\. They're not supported on the **Monitor** resource\.
**TagResource**, **UntagResource**, and **ListTagsForResource** for the **Monitor** resource type were updated to be required\.
To see more information about the actions, resources, and condition keys that you can specify in policies to manage access to AWS resources in Internet Monitor, see [Actions, resources, and condition keys for Amazon CloudWatch Internet Monitor](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncloudwatchinternetmonitor.html)\.

**Topics**
+ [How Internet Monitor works with IAM](security_iam_service-with-iam.md)
+ [IAM permissions](CloudWatch-IM-permissions.md)
+ [Using a service\-linked role for Internet Monitor](using-service-linked-roles-CWIM.md)