# Add a metrics explorer widget to a CloudWatch dashboard<a name="add_metrics_explorer_dashboard"></a>

Metrics explorer widgets include graphs of multiple resources that have the same tag, or share the same resource property such as an instance type\. These widgets stay up to date, as resources that match are created or deleted\. Adding metrics explorer widgets to your dashboard helps you to troubleshoot your environment more efficiently\.

For example, you can monitor your fleet of EC2 instances by assigning tags that represent their environments, such as production or test\. You can then use these tags to filter and aggregate the operational metrics, such as `CPUUtilization`, to understand the health and performance of the EC2 instances that are associated with each tag\.

The following steps explain how to add a metrics explorer widget to a dashboard using the console\. You can also add it programmatically or by using AWS CloudFormation\. For more information, see [ Metrics Explorer Widget Object Definition](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/CloudWatch-Dashboard-Body-Structure.html#CloudWatch-Dashboard-Properties-Metric-Explorer-Object) and [ AWS::CloudWatch::Dashboard](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudwatch-dashboard.html)\.

**To add a metrics explorer widget to a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**\.

1. Choose the name of the dashboard where you want to add the metrics explorer widget\.

1. Choose **Add widget**\.

1. Choose **Explorer** and then choose **Next**\. 
**Note**  
You must be opted in to the new dashboard view to be able to add a Metrics Explorer widget\. To opt in, choose **Dashboards** in the navigation pane, then choose **try out the new interface** in the banner at the top of the page\.

1. Do one of the following:
   + To use a template, choose **Pre\-filled Explorer widget** and then select a template to use\.
   + To create a custom visualization, choose **Empty Explorer widget**\.

1. Choose **Create**\.

   If you used a template, the widget appears on your dashboard with the selected metrics\. If you're satisfied with the explorer widget and the dashboard, choose **Save dashboard**\.

   If you did not use a template, continue to the following steps\.

1. In the new widget under **Explorer**, in the **Metrics** box, choose a single metric or all the available metrics from a service\.

   After you choose a metric, you can optionally repeat this step to add more metrics\.

1. For each metric selected, CloudWatch displays the statistic that it will use immediately after the metric name\. To change this, choose the statistic name and then choose the statistic that you want\.

1. Under **From**, choose a tag or a resource property to filter your results\.

   After you do this, you can optionally repeat this step to choose more tags or resource properties\.

   If you choose multiple values of the same property, such as two EC2 instance types, the explorer displays all the resources that match either chosen property\. It's treated as an OR operation\.

   If you choose different properties or tags, such as the **Production** tag and the M5 instance type, only the resources that match all of these selections are displayed\. This is treated as an AND operation\.

1. \(Optional\) For **Aggregate by**, choose a statistic to use to aggregate the metrics\. Then, next to **for**, choose how to aggregate the metric from the list\. You can aggregate together all the resources that are currently displayed, or aggregate by a single tag or resource property\.

   Depending on how you choose to aggregate, the result may be a single time series or multiple time series\. 

1. Under **Split by**, you can choose to split a single graph with multiple time series into multiple graphs\. The split can be made by a variety of criteria, which you choose under **Split by**\.

1. Under **Graph options**, you can refine the graph by changing the period, the type of graph, the legend placement, and the layout\.

1. If you're satisfied with the explorer widget and the dashboard, choose **Save dashboard**\.