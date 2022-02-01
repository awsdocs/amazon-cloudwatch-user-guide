# Edit a graph on a CloudWatch dashboard<a name="edit_graph_dashboard"></a>

You can edit a graph to change the title, statistic, or period, or to add or remove metrics\. If you have multiple metrics displayed on a graph, you can reduce the clutter by temporarily hiding the metrics that don't interest you\.

**To edit a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Hover over the title of the graph and choose **Widget actions**, **Edit**\.

1. To change the graph's title, select the title, enter a new title, and press **Enter**\.

1. To change the type of graph, choose **Graph options**\. You can then choose between a line graph, stacked area chart, bar chart, pie chart, or number\.

1. To change the time range shown on the graph, choose either **custom** at the top of the graph, or one of the time periods to the left of **custom**\.

1. To change the type of widget between separate lines on a graph, stacked lines on a graph, and a number, choose the box next to the right of **custom** and select either **Line**, **Stacked area**, or **Number**\.

1. In the lower half of the screen, in the **Graphed metrics** tab, you can change the dynamic label, colors, statistic, or period corresponding to a metric:

   1. \(Optional\) To specify a dynamic label for a metric, choose **Dynamic labels**\. Dynamic labels display a statistic about the metric, and automatically update when the dashboard or graph is refreshed\. 

      By default, the dynamic values you add to the label appear at the beginning of the label\. You can then choose the **Label** value for the metric to edit the label\. For more information, see [Using dynamic labels](graph-dynamic-labels.md)\.

   1. To change the color of one of the lines, select the color square next to the metric to display a color picker box\. Choose another color in the color picker, and click outside the color picker to see your new color on the graph\. Alternatively, in the color picker, you can enter the six\-digit HTML hex color code for the color you want and press **Enter**\.

   1. To change the statistic, choose **Statistic** in the lower half of the window, and choose the new statistic you want\. For more information, see [Statistics](cloudwatch_concepts.md#Statistic)\.

   1. To change the time period, which is next to **Statistic** in the lower half of the window, choose **Period** and select another value\. This new setting is used on the dashboard only if the period setting of the dashboard itself is set to `Auto`\. Otherwise, the period setting of the dashboard overrides the period setting for individual widgets\.

1. To add or edit horizontal annotations, choose **Graph options**:

   1. To add a horizontal annotation, choose **Add horizontal annotation**\.

   1. For **Label**, enter a label for the annotation\.

   1. For **Value**, enter the metric value where the horizontal annotation appears\.

   1. For **Fill**, specify how to use fill shading with this annotation\. For example, choose `Above` or `Below` for the corresponding area to be filled\. If you specify `Between`, another `Value` field appears, and the area of the graph between the two values is filled\.

      You can change the fill color of an annotation by choosing the color square in the left column of the annotation\. 

   1. For **Axis**, specify whether the numbers in `Value` refer to the metric associated with the left y\-axis or the right y\-axis, if the graph includes multiple metrics\.

   Repeat these steps to add multiple horizontal annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, choose the **x** in the **Actions** column\.

1. To add or edit vertical annotations, choose **Graph options**: **Add vertical annotation**:

   1. To add a vertical annotation, choose **Add vertical annotation**\.

   1. For **Label**, enter a label for the annotation\. To show only the date and time on the annotation, keep the **Label** field blank\.

   1. For **Date**, specify the date and time where the vertical annotation appears\.

   1. For **Fill**, specify whether to use fill shading before or after a vertical annotation, or between two vertical annotations\. For example, choose `Before` or `After` for the corresponding area to be filled\. If you specify `Between`, another `Date` field appears, and the area of the graph between the two values is filled\.

   Repeat these steps to add multiple vertical annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, choose **x** in the **Actions** column\.

1. To hide or change the position of the graph legend, hover over the title of the graph and choose **Widget actions**, **Edit**\. Hover over **Legend** and choose **Hidden**, **Bottom**, or **Right**\.

1. To customize the y\-axis, choose **Graph options**\. You can enter a custom label in **Label** under **Left Y Axis**\. If the graph also displays values on the right y\-axis, you can customize that label, too\. You can also set minimums and maximums on the y\-axis values, and the graph displays only the value range that you specify\.

1. When you're finished with your changes, choose **Update widget**\.

**To temporarily hide metrics for a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. In the graph's footer, hover over the colored square in the legend\. When it changes to an X, choose it\.

1. To restore the metric, choose the grayed\-out square and metric name\.