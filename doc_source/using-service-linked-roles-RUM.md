# Using service\-linked roles for CloudWatch RUM<a name="using-service-linked-roles-RUM"></a>

CloudWatch RUM uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to RUM\. Service\-linked roles are predefined by RUM and include all the permissions that the service requires to call other AWS services on your behalf\. 

RUM defines the permissions of these service\-linked roles, and unless defined otherwise, only RUM can assume the role\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete the roles only after first deleting their related resources\. This restriction protects your RUM resources because you can't inadvertently remove permissions to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes** in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for RUM<a name="service-linked-role-permissions-RUM"></a>

RUM uses the service\-linked role named **AWSServiceRoleForCloudWatchRUM** – this role allows RUM to send AWS X\-Ray trace data into your account, for app monitors that you enable X\-Ray tracing for\.

The **AWSServiceRoleForCloudWatchRUM** service\-linked role trusts the X\-Ray service to assume the role\. X\-Ray sends the trace data to your account\.

The **AWSServiceRoleForCloudWatchRUM** service\-linked role has an IAM policy attached named **AmazonCloudWatchRUMServiceRolePolicy**\. This policy grants permission to CloudWatch RUM to publish monitoring data to other relevant AWS service\. It includes permissions that allow RUM to complete the following actions:
+ `xray:PutTraceSegments`

## Creating a service\-linked role for RUM<a name="create-service-linked-role-RUM"></a>

You do not need to manually create the service\-linked role for CloudWatch RUM\. The first time that you create an app monitor with X\-Ray tracing enabled, or update an app monitor to use X\-Ray tracing, RUM creates **AWSServiceRoleForCloudWatchRUM** for you\. 

For more information, see [Creating a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\.

## Editing a service\-linked role for RUM<a name="edit-service-linked-role-RUM"></a>

CloudWatch RUM does not allow you to edit the **AWSServiceRoleForCloudWatchRUM** role\. After you create these roles, you cannot change their names because various entities might reference these roles\. However, you can edit the description of these roles using IAM\. 

### Editing a service\-linked role description \(IAM console\)<a name="edit-service-linked-role-iam-console-RUM"></a>

You can use the IAM console to edit the description of a service\-linked role\.

**To edit the description of a service\-linked role \(console\)**

1. In the navigation pane of the IAM console, choose **Roles**\.

1. Choose the name of the role to modify\.

1. To the far right of **Role description**, choose **Edit**\. 

1. Type a new description in the box, and choose **Save**\.

### Editing a service\-linked role description \(AWS CLI\)<a name="edit-service-linked-role-iam-cli-RUM"></a>

You can use IAM commands from the AWS Command Line Interface to edit the description of a service\-linked role\.

**To change the description of a service\-linked role \(AWS CLI\)**

1. \(Optional\) To view the current description for a role, use the following commands:

   ```
   $ aws iam [get\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/get-role.html) --role-name role-name
   ```

   Use the role name, not the ARN, to refer to roles with the AWS CLI commands\. For example, if a role has the following ARN: `arn:aws:iam::123456789012:role/myrole`, you refer to the role as **myrole**\.

1. To update a service\-linked role's description, use the following command:

   ```
   $ aws iam [update\-role\-description](https://docs.aws.amazon.com/cli/latest/reference/iam/update-role-description.html) --role-name role-name --description description
   ```

### Editing a service\-linked role description \(IAM API\)<a name="edit-service-linked-role-iam-api-RUM"></a>

You can use the IAM API to edit the description of a service\-linked role\.

**To change the description of a service\-linked role \(API\)**

1. \(Optional\) To view the current description for a role, use the following command:

   [GetRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetRole.html) 

1. To update a role's description, use the following command: 

   [UpdateRoleDescription](https://docs.aws.amazon.com/IAM/latest/APIReference/API_UpdateRoleDescription.html)

## Deleting a service\-linked role for RUM<a name="delete-service-linked-role-RUM"></a>

If you no longer have app monitors with X\-Ray enabled, we recommend that you delete the **AWSServiceRoleForCloudWatchRUM** role\.

That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can delete it\.

### Cleaning up a service\-linked role<a name="service-linked-role-review-before-delete"></a>

Before you can use IAM to delete a service\-linked role, you must first confirm that the role has no active sessions and remove any resources used by the role\.

**To check whether the service\-linked role has an active session in the IAM console**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. Choose the name \(not the check box\) of the **AWSServiceRoleForCloudWatchRUM** role\.

1. On the **Summary** page for the selected role, choose **Access Advisor** and review the recent activity for the service\-linked role\.
**Note**  
If you are unsure whether RUM is using the **AWSServiceRoleForCloudWatchRUM** role, try to delete the role\. If the service is using the role, then the deletion fails and you can view the Regions where the role is being used\. If the role is being used, then you must wait for the session to end before you can delete the role\. You cannot revoke the session for a service\-linked role\. 

### Deleting a service\-linked role \(IAM console\)<a name="delete-service-linked-role-iam-console"></a>

You can use the IAM console to delete a service\-linked role\.

**To delete a service\-linked role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\. Select the check box next to the name of the role you want to delete, not the name or row itself\. 

1. For **Role actions**, choose **Delete role**\.

1. In the confirmation dialog box, review the service last accessed data, which shows when each of the selected roles last accessed an AWS service\. This helps you to confirm whether the role is currently active\. To proceed, choose **Yes, Delete**\.

1. Watch the IAM console notifications to monitor the progress of the service\-linked role deletion\. Because the IAM service\-linked role deletion is asynchronous, the deletion task can succeed or fail after you submit the role for deletion\. If the task fails, choose **View details** or **View Resources** from the notifications to learn why the deletion failed\. If the deletion fails because there are resources in the service that are being used by the role, then the reason for the failure includes a list of resources\.

### Deleting a service\-linked role \(AWS CLI\)<a name="delete-service-linked-role-iam-cli-RUM"></a>

You can use IAM commands from the AWS Command Line Interface to delete a service\-linked role\.

**To delete a service\-linked role \(AWS CLI\)**

1. Because a service\-linked role cannot be deleted if it is being used or has associated resources, you must submit a deletion request\. That request can be denied if these conditions are not met\. You must capture the `deletion-task-id` from the response to check the status of the deletion task\. Type the following command to submit a service\-linked role deletion request:

   ```
   $ aws iam [delete\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/delete-service-linked-role.html) --role-name service-linked-role-name
   ```

1. Type the following command to check the status of the deletion task:

   ```
   $ aws iam [get\-service\-linked\-role\-deletion\-status](https://docs.aws.amazon.com/cli/latest/reference/iam/get-service-linked-role-deletion-status.html) --deletion-task-id deletion-task-id
   ```

   The status of the deletion task can be `NOT_STARTED`, `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. If the deletion fails, the call returns the reason that it failed so that you can troubleshoot\.

### Deleting a service\-linked role \(IAM API\)<a name="delete-service-linked-role-iam-api-RUM"></a>

You can use the IAM API to delete a service\-linked role\.

**To delete a service\-linked role \(API\)**

1. To submit a deletion request for a service\-linked role, call [DeleteServiceLinkedRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_DeleteServiceLinkedRole.html)\. In the request, specify the role name that you want to delete\.

   Because a service\-linked role cannot be deleted if it is being used or has associated resources, you must submit a deletion request\. That request can be denied if these conditions are not met\. You must capture the `DeletionTaskId` from the response to check the status of the deletion task\.

1. To check the status of the deletion, call [GetServiceLinkedRoleDeletionStatus](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetServiceLinkedRoleDeletionStatus.html)\. In the request, specify the `DeletionTaskId`\.

   The status of the deletion task can be `NOT_STARTED`, `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. If the deletion fails, the call returns the reason that it failed so that you can troubleshoot\.