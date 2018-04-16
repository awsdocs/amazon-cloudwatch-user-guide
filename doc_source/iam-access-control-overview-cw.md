# Overview of Managing Access Permissions to Your CloudWatch Resources<a name="iam-access-control-overview-cw"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\), and some services \(such as AWS Lambda\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [CloudWatch Resources and Operations](#CloudWatch_ARN_Format)
+ [Understanding Resource Ownership](#understanding-resource-ownership-cw)
+ [Managing Access to Resources](#managing-access-resources-cw)
+ [Specifying Policy Elements: Actions, Effects, and Principals](#actions-effects-principals-cw)
+ [Specifying Conditions in a Policy](#policy-conditions-cw)

## CloudWatch Resources and Operations<a name="CloudWatch_ARN_Format"></a>

CloudWatch doesn't have any specific resources for you to control access to\. Therefore, there are no CloudWatch Amazon Resource Names \(ARNs\) for you to use in an IAM policy\. For example, you can't give a user access to CloudWatch data for only a specific set of EC2 instances or a specific load balancer\. Permissions granted using IAM cover all the cloud resources you use or monitor with CloudWatch\. In addition, you can't use IAM roles with the CloudWatch command line tools\.

You use an **\*** \(asterisk\) as the resource when writing a policy to control access to CloudWatch actions\. For example:

```
{
  "Version": "2012-10-17",
  "Statement":[{
      "Effect":"Allow",
      "Action":["cloudwatch:GetMetricStatistics","cloudwatch:ListMetrics"],
      "Resource":"*",
      "Condition":{
         "Bool":{
            "aws:SecureTransport":"true"
            }
         }
      }
   ]
   }
```

For more information about ARNs, see [ARNs](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_Identifiers.html#Identifiers_ARNs) in *IAM User Guide*\. For information about CloudWatch Logs ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-cloudwatch-logs) in the *Amazon Web Services General Reference*\. For an example of a policy that covers CloudWatch actions, see [Using Identity\-Based Policies \(IAM Policies\) for CloudWatch](iam-identity-based-access-control-cw.md)\.


| Action | ARN \(with region\) | ARN \(for use with IAM role\) | 
| --- | --- | --- | 
|   `Stop`   |  arn:aws:automate:us\-east\-1:ec2:stop  |  arn:aws:swf:us\-east\-1:*customer\-account*:action/actions/AWS\_EC2\.InstanceId\.Stop/1\.0  You must create at least one stop alarm using the Amazon EC2 or CloudWatch console to create the **EC2ActionsAccess** IAM role\. After this IAM role is created, you can create stop alarms using the AWS CLI\.  | 
|   `Terminate`   |  arn:aws:automate:us\-east\-1:ec2:terminate  |  arn:aws:swf:us\-east\-1:*customer\-account*:action/actions/AWS\_EC2\.InstanceId\.Terminate/1\.0 You must create at least one terminate alarm using the Amazon EC2 or CloudWatch console to create the **EC2ActionsAccess** IAM role\. After this IAM role is created, you can create terminate alarms using the AWS CLI\.  | 
|   `Reboot`   |  n/a  |  arn:aws:swf:us\-east\-1:*customer\-account*:action/actions/AWS\_EC2\.InstanceId\.Reboot/1\.0 You must create at least one reboot alarm using the Amazon EC2 or CloudWatch console to create the **EC2ActionsAccess** IAM role\. After this IAM role is created, you can create reboot alarms using the AWS CLI\.  | 
|   `Recover`   |  arn:aws:automate:us\-east\-1:ec2:recover  |  n/a  | 

## Understanding Resource Ownership<a name="understanding-resource-ownership-cw"></a>

The AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(that is, the AWS account root user, an IAM user, or an IAM role\) that authenticates the resource creation request\. CloudWatch does not have any resources that you can own\.

## Managing Access to Resources<a name="managing-access-resources-cw"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of CloudWatch\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as identity\-based policies \(IAM policies\) and policies attached to a resource are referred to as resource\-based policies\. CloudWatch supports only identity\-based policies\.

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#identity-based-policies-cw)
+ [Resource\-Based Policies \(IAM Policies\)](#resource-based-policies-cw)

### Identity\-Based Policies \(IAM Policies\)<a name="identity-based-policies-cw"></a>

You can attach policies to IAM identities\. For example, you can do the following:
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to create an Amazon CloudWatch resource, such as metrics, you can attach a permissions policy to a user or group that the user belongs to\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in account A can create a role to grant cross\-account permissions to another AWS account \(for example, account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in account A\.

  1. Account A administrator attaches a trust policy to the role identifying account B as the principal who can assume the role\. 

  1. Account B administrator can then delegate permissions to assume the role to any users in account B\. Doing this allows users in account B to create or access resources in account A\. The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

For more information about using identity\-based policies with CloudWatch, see [Using Identity\-Based Policies \(IAM Policies\) for CloudWatch](iam-identity-based-access-control-cw.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\.

### Resource\-Based Policies \(IAM Policies\)<a name="resource-based-policies-cw"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an Amazon S3 bucket to manage access permissions to that bucket\. CloudWatch doesn't support resource\-based policies\.

## Specifying Policy Elements: Actions, Effects, and Principals<a name="actions-effects-principals-cw"></a>

For each CloudWatch resource, the service defines a set of API operations\. To grant permissions for these API operations, CloudWatch defines a set of actions that you can specify in a policy\. Some API operations can require permissions for more than one action in order to perform the API operation\. For more information about resources and API operations, see [CloudWatch Resources and Operations](#CloudWatch_ARN_Format) and CloudWatch [Actions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_Operations.html)\.

The following are the basic policy elements:
+ **Resource** – Use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. CloudWatch does not have any resources for you to control using policies resources, so use the wildcard character \(\*\) in IAM policies\. For more information, see [CloudWatch Resources and Operations](#CloudWatch_ARN_Format)\.
+ **Action** – Use action keywords to identify resource operations that you want to allow or deny\. For example, the `cloudwatch:ListMetrics` permission allows the user permissions to perform the `ListMetrics` operation\.
+ **Effect** – You specify the effect, either allow or deny, when the user requests the specific action\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. CloudWatch doesn't support resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM JSON Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the CloudWatch API actions and the resources that they apply to, see [Amazon CloudWatch Permissions Reference](permissions-reference-cw.md)\.

## Specifying Conditions in a Policy<a name="policy-conditions-cw"></a>

When you grant permissions, you can use the access policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. For a list of context keys supported by each AWS service and a list of AWS\-wide policy keys, see [AWS Service Actions and Condition Context Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditions.html) and [Global and IAM Condition Context Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.