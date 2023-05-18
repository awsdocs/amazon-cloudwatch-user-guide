# Creating a CloudWatch dashboard<a name="create_dashboard"></a>

 To get started, create a CloudWatch dashboard\. You can create multiple dashboards, and you can add dashboards to a favorites list\. You aren't limited to the number of dashboards that you can have in your AWS account\. All dashboards are global\. They are not Region\-specific\. 

 The following procedure shows you how to create a dashboard from the CloudWatch console\. You can use the `PutDashboard` API operation to create a dashboard from the command line interface\. The API operation contains a JSON string that defines your dashboard content\. For more information about creating a dashboard with the `PutDashboard` API operation, see [PutDashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutDashboard.html) in the *Amazon CloudWatch API Reference*\. 

**Tip**  
 If you're creating a new dashboard with the `PutDashboard` API operation, you can use the JSON string from a dashboard that already exists\. 

**To create a dashboard from the console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose **Create dashboard**\. 

1. In the **Create new dashboard** dialog box, enter a name for the dashboard, and then choose **Create dashboard**\. 

    If you use the name **CloudWatch\-Default** or **CloudWatch\-Default\-*ResourceGroupName***, the dashboard appears in the overview of the CloudWatch home page under **Default Dashboard**\. For more information, see [Getting started with Amazon CloudWatch](GettingStarted.md)\. 

1.  In the **Add to this dashboard** dialog box, do one of the following: 
   +  To add a graph to the dashboard, choose **Line** or **Stacked area**, and then choose **Configure**\. In the **Add metric graph** dialog box, select the metric\(s\) to graph, and then choose **Create widget**\. If a metric doesn't appear in the dialog box because it hasn't published data in more than 14 days, you can add it manually\. For more information, see [Graph metrics manually on a CloudWatch dashboard](add_old_metrics_to_graph.md)\. 
   +  To add a number displaying a metric to the dashboard, choose **Number**, and then choose **Configure**\. In the **Add metric graph** dialog box, select the metric\(s\) to graph, and then choose **Create widget**\.
   +  To add a text block to the dashboard, choose **Text**, and then choose **Configure**\. In the **New text widget** dialog box, for **Markdown**, format your text using [Markdown](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html), and then choose **Create widget**\. 

1.  \(Optional\) Choose **Add widget**, and then repeat step 4 to add another widget to the dashboard\. You can repeat this step multiple times\. 

1.  Choose **Save dashboard**\. 