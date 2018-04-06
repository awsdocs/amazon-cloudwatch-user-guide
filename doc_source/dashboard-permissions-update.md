# CloudWatch Dashboard Permissions Update<a name="dashboard-permissions-update"></a>

On April 1, 2018, the permissions required to access CloudWatch dashboards will change\. Currently, the **cloudwatch:GetMetricData** permission is required to view CloudWatch dashboards, and the **cloudwatch:PutMetricData** permission is required to create or modify dashboards\. Beginning on April 1, dashboard access in the CloudWatch console will instead require newer permissions that were introduced in 2017 to support dashboard API operations:

+ **cloudwatch:GetDashboard**

+ **cloudwatch:ListDashboards**

+ **cloudwatch:PutDashboard**

+ **cloudwatch:DeleteDashboards**

To check whether you will have access to CloudWatch dashboards after the change, choose **Check permissions** in update messages in the CloudWatch console\. If that check shows that you will not have these permissions after the update, you should use the IAM console to fix your permissions before April 1\.

To retain access to CloudWatch dashboards, you need one of the following:

+ The **AdministratorAccess** policy\.

+ The **CloudWatchFullAccess** policy\.

+ A custom policy that includes one or more of these specific permissions:

  + `cloudwatch:GetDashboard` and `cloudwatch:ListDashboards` to be able to view dashboards

  + `cloudwatch:PutDashboard` to be able to create or modify dashboards

  + `cloudwatch:DeleteDashboards` to be able to delete dashboards

For more information for changing permissions for an IAM user using policies, see [Changing Permissions for an IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html)\.

For more information about CloudWatch permissions, see [Amazon CloudWatch Permissions Reference](permissions-reference-cw.md)\.

For more information about dashboard API operations, see [PutDashboard](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutDashboard.html) in the Amazon CloudWatch API Reference\.