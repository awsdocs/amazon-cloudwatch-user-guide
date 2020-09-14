# Using Amazon CloudWatch Dashboards<a name="CloudWatch_Dashboards"></a>

Amazon CloudWatch dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different Regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\.

With dashboards, you can create the following:
+ A single view for selected metrics and alarms to help you assess the health of your resources and applications across one or more regions\. You can select the color used for each metric on each graph, so that you can easily track the same metric across multiple graphs\.

  You can create dashboards that display graphs and other widgets from multiple AWS accounts and multiple Regions\. For more information, see [Cross\-Account Cross\-Region CloudWatch Console](Cross-Account-Cross-Region.md)\.
+ An operational playbook that provides guidance for team members during operational events about how to respond to specific incidents\.
+ A common view of critical resource and application measurements that can be shared by team members for faster communication flow during operational events\.

You can create dashboards by using the console, the AWS CLI, or the `PutDashboard` API\.

To access CloudWatch dashboards, you need one of the following:
+ The AdministratorAccess policy
+ The CloudWatchFullAccess policy
+ A custom policy that includes one or more of these specific permissions:
  + `cloudwatch:GetDashboard` and `cloudwatch:ListDashboards` to be able to view dashboards
  + `cloudwatch:PutDashboard` to be able to create or modify dashboards
  + `cloudwatch:DeleteDashboards` to be able to delete dashboards

**Topics**
+ [Create a Dashboard](create_dashboard.md)
+ [Cross\-Account Cross\-Region Dashboards](cloudwatch_xaxr_dashboard.md)
+ [Creating and working with widgets on CloudWatch dashboards](create-and-work-with-widgets.md)
+ [Sharing Dashboards](cloudwatch-dashboard-sharing.md)
+ [Use Live Data](cloudwatch-live-data.md)
+ [Add a Dashboard to Your Favorites List](add-dashboard-to-favorites.md)
+ [Change the Period Override Setting or Refresh Interval](change_dashboard_refresh_interval.md)
+ [Change the Time Range or Time Zone Format](change_dashboard_time_format.md)