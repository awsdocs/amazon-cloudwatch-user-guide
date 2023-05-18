# Using Amazon CloudWatch dashboards<a name="CloudWatch_Dashboards"></a>

Amazon CloudWatch dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different Regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\.

With dashboards, you can create the following:
+ A single view for selected metrics and alarms to help you assess the health of your resources and applications across one or more Regions\. You can select the color used for each metric on each graph, so that you can easily track the same metric across multiple graphs\.
+ An operational playbook that provides guidance for team members during operational events about how to respond to specific incidents\.
+ A common view of critical resource and application measurements that can be shared by team members for faster communication flow during operational events\.

If you have multiple AWS accounts, you can set up *CloudWatch cross\-account observability* and then create rich cross\-account dashboards in your monitoring accounts\. These dashboards can include graphs of metrics from source accounts and CloudWatch Logs Insights widgets with queries of log groups from source accounts\. Additionally, alarms that you create in the monitoring account can watch metrics in source accounts\. For more information, see [CloudWatch cross\-account observability](CloudWatch-Unified-Cross-Account.md)\.

 You can create dashboards from the console or using the AWS CLI or `PutDashboard` API operation\. You can add dashboards to a favorites list, where you can access not only your favorited dashboards, but also your recently visited dashboards\. For more information, see [Add a dashboard to your favorites list](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/add-dashboard-to-favorites.html)\. 

To access CloudWatch dashboards, you need one of the following:
+ The `AdministratorAccess` policy
+ The `CloudWatchFullAccess` policy
+ A custom policy that includes one or more of these specific permissions:
  + `cloudwatch:GetDashboard` and `cloudwatch:ListDashboards` to be able to view dashboards
  + `cloudwatch:PutDashboard` to be able to create or modify dashboards
  + `cloudwatch:DeleteDashboards` to be able to delete dashboards

**Topics**
+ [Create a dashboard](create_dashboard.md)
+ [CloudWatch cross\-account observability dashboard](cloudwatch_crossaccount_dashboard.md)
+ [Cross\-account cross\-Region dashboards](cloudwatch_xaxr_dashboard.md)
+ [Creating and working with widgets on CloudWatch dashboards](create-and-work-with-widgets.md)
+ [Sharing dashboards](cloudwatch-dashboard-sharing.md)
+ [Use live data](cloudwatch-live-data.md)
+ [Viewing an animated dashboard](cloudwatch-animated-dashboard.md)
+ [Add a dashboard to your favorites list](add-dashboard-to-favorites.md)
+ [Change the period override setting or refresh interval](change_dashboard_refresh_interval.md)
+ [Change the time range or time zone format](change_dashboard_time_format.md)