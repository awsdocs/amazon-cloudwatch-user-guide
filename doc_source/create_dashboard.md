# Create a CloudWatch Dashboard<a name="create_dashboard"></a>

To get started with CloudWatch dashboards, you must first create a dashboard\. You can create multiple dashboards\. There is no limit on the number of CloudWatch dashboards in your AWS account\. All dashboards are global, not region\-specific\.

The steps in this section are for creating a dashboard using the console\. You can also create a dashboard with the `PutDashboard` API, which uses a JSON string to define the dashboard contents\. To create a dashboard using `PutDashboard` and base this dashboard on an existing dashboard, choose **Actions**, **View/edit source** to display and copy the JSON string of a current dashboard to use for your new dashboard\.

For more information about creating a dashboard using the API, see [PutDashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutDashboard.html) in the Amazon CloudWatch API Reference\.

**To create a dashboard using the console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**, **Create dashboard**\.

1. In the **Create new dashboard** dialog box, type a name for the dashboard and choose **Create dashboard**\.

1. Do one of the following in the **Add to this dashboard** dialog box:
   + To add a graph to your dashboard, choose **Line** or **Stacked area** and choose **Configure**\. In the **Add metric graph** dialog box, select the metrics to graph and choose **Create widget**\. If a specific metric does not appear in the dialog box because it has not published data in more than 14 days, you can add it manually\. For more information, see [Graph Metrics Manually on a CloudWatch Dashboard](add_old_metrics_to_graph.md)\.
   + To add a number displaying a metric to the dashboard, choose **Number**, **Configure**\. In the **Add metric graph** dialog box, select the metrics to graph and choose **Create widget**\.
   + To add a text block to your dashboard, choose **Text**, **Configure**\. In the **New text widget** dialog box, for **Markdown**, add and format your text using [Markdown](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html)\. Choose **Create widget**\.

1. Optionally, choose **Add widget** and repeat step 4 to add another widget to the dashboard\. You can repeat this step multiple times\.

1. Choose **Save dashboard**\.