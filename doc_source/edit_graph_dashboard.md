# Edit a Graph on a CloudWatch Dashboard<a name="edit_graph_dashboard"></a>

You can edit a graph to change the title, statistic, or period, or to add or remove metrics\. If you have multiple metrics displayed on a graph, you can reduce clutter by temporarily hiding the metrics that don't interest you\.

**To edit a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Hover over the title of the graph and choose **Widget actions**, **Edit**\.

1. To change the graph's title, select the title, type a new title, and press ENTER\.

1. In the lower half of the screen, in the **Graphed metrics** tab, you can change the colors, statistic, or period:

   1. To change the color of one of the lines, select the color square next to the metric to display a color picker box\. Choose another color in the color picker, and then click outside the color picker to see your new color on the graph\. Alternatively, in the color picker, you can type the six\-digit HTML hex color code for the color you want and press ENTER\.

   1. To change the statistic, choose **Statistic** in the lower half of the window, and choose the new statistic you want\.

   1. To change the time period, which is next to **Statistic** in the lower half of the window, choose **Period**, and then select another value\. This new setting is used on the dashboard only if the period setting of the dashboard itself is set to `Auto`\. Otherwise, the period setting of the dashboard overrides the period setting for individual widgets\.

1. To add or edit horizontal annotations, choose **Graph options**:

   1. To add a horizontal annotation, choose **Add horizontal annotation**\.

   1. For **Label**, type a label for the annotation\.

   1. For **Value**, type the metric value where the horizontal annotation appears\.

   1. For **Fill**, specify how to use fill shading with this annotation\. For example, choose `Above` or `Below` for the corresponding area to be filled\. If you specify `Between`, another `Value` field appears, and the area of the graph between the two values is filled\.

      You can change the fill color of an annotation by choosing the color square in the left column of the annotation\. 

   1. For **Axis**, specify whether the numbers in `Value` refer to the metric associated with the left Y\-axis or the right Y\-axis, if the graph includes multiple metrics\.

   Repeat these steps to add multiple horizontal annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, click the **x** in the **Actions** column\.

1. When you're finished with your changes, choose **Update widget**\.

**To temporarily hide metrics on a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. In the graph's footer, hover over the colored square in the legend\. When it changes to an X, click it\.

1. To restore the metric, choose the grayed out square and metric name\.