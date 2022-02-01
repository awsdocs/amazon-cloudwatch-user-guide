# Add or remove a graph from a CloudWatch dashboard<a name="add_remove_graph_dashboard"></a>

You can add graphs containing one or more metrics to your dashboard for the resources you monitor\. You can remove the graphs when they're no longer needed\.

**To add a graph to a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Choose **Add widget**\.

1. Choose the type of graph you want: **Line**, **Stacked area**, **Bar**, or **Pie**\. Then choose **Next**\.

1. Choose **Metrics** and then choose **Configure**\.

1. In the **All metrics** tab, select the metrics to graph\. If a specific metric doesn't appear in the dialog box because it hasn't published data in more than 14 days, you can add it manually\. For more information, see [Graph metrics manually on a CloudWatch dashboard](add_old_metrics_to_graph.md)\.

1. \(Optional\) To change the type of graph, choose **Graph options**\. You can then choose between a line graph, stacked area chart, bar chart, pie chart, or number\.

1. <a name="dynamic-labels"></a>\(Optional\) As you choose metrics to graph, you can specify a dynamic label to appear on the graph legend for each metric\. Dynamic labels display a statistic about the metric and automatically update when the dashboard or graph is refreshed\. To add a dynamic label, choose **Graphed metrics** and then **Dynamic labels**\.

   By default, the dynamic values you add to the label appear at the beginning of the label\. You can then choose the **Label** value for the metric to edit the label\. For more information, see [Using dynamic labels](graph-dynamic-labels.md)\.

1. <a name="horizontal-annotations2"></a>\(Optional\) As you choose metrics to graph, you can change their color on the graph\. To do so, choose **Graphed metrics** and select the color square next to the metric to display a color picker box\. Choose another color square in the color picker\. Click outside the color picker to see your new color on the graph\. Alternatively, in the color picker, you can enter the six\-digit standard HTML hex color code for the color you want and press **Enter**\.

1. \(Optional\) To view more information about the metric being graphed, hover over the legend\.

1. \(Optional\) To change the widget type, hover over the title area of the graph and choose **Widget actions**, **Widget type**\.

1. \(Optional\) To change the statistic used for a metric, choose **Graphed metrics**, **Statistic**, and select the statistic you want to use\. For more information, see [Statistics](cloudwatch_concepts.md#Statistic)\.

1. \(Optional\) To change the time range shown on the graph, choose either **custom** at the top of the graph or one of the time periods to the left of **custom**\.

1. <a name="horizontal-annotations"></a> \(Optional\) Horizontal annotations help dashboard users quickly see when a metric has spiked to a certain level, or whether the metric is within a predefined range\. To add a horizontal annotation, choose **Graph options**, **Add horizontal annotation**:

   1. For **Label**, enter a label for the annotation\.

   1. For **Value**, enter the metric value where the horizontal annotation appears\.

   1. For **Fill**, specify whether to use fill shading with this annotation\. For example, choose `Above` or `Below` for the corresponding area to be filled\. If you specify `Between`, another `Value` field appears, and the area of the graph between the two values is filled\.

   1. For **Axis**, specify whether the numbers in `Value` refer to the metric associated with the left y\-axis or the right y\-axis, if the graph includes multiple metrics\.

      You can change the fill color of an annotation by choosing the color square in the left column of the annotation\. 

   Repeat these steps to add multiple horizontal annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, choose **x** in the **Actions** column\.

1. <a name="vertical-annotations"></a> \(Optional\) Vertical annotations help you mark milestones in a graph, such as operational events or the beginning and end of a deployment\. To add a vertical annotation, choose **Graph options**, **Add vertical annotation**:

   1. For **Label**, enter a label for the annotation\. To show only the date and time on the annotation, keep the **Label** field blank\.

   1. For **Date**, specify the date and time where the vertical annotation appears\.

   1. For **Fill**, specify whether to use fill shading before or after a vertical annotation or between two vertical annotations\. For example, choose `Before` or `After` for the corresponding area to be filled\. If you specify `Between`, another `Date` field appears, and the area of the graph between the two values is filled\.

   Repeat these steps to add multiple vertical annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, choose **x** in the **Actions** column\.

1. Choose **Create widget**\.

1. Choose **Save dashboard**\.

**To remove a graph from a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Hover over the title of the graph and choose **Widget actions** and then **Delete**\.

1. Choose **Save dashboard**\. If you attempt to navigate away from the dashboard before you save your changes, you're prompted to either save or discard your changes\.