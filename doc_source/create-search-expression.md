# Create a CloudWatch graph with a search expression<a name="create-search-expression"></a>

On the CloudWatch console, you can access search capability when you add a graph to a dashboard, or by using the **Metrics** view\. 

You can't create an alarm based on a **SEARCH** expression\. This is because search expressions return multiple time series, and an alarm based on a math expression can watch only one time series\.

**To add a graph with a search expression to an existing dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Choose **Add widget**\.

1. Choose either **Line** or **Stacked area** and choose **Configure**\.

1. On the **Graphed metrics** tab, choose **Add a math expression**\.

1.  For **Details**, enter the search expression that you want\. For example, **SEARCH\('\{AWS/EC2,InstanceId\} MetricName="CPUUtilization"', 'Average'\)** 

1. \(Optional\) To add another search expression or math expression to the graph, choose **Add a math expression**

1. \(Optional\) After you add a search expression, you can specify a dynamic label to appear on the graph legend for each metric\. Dynamic labels display a statistic about the metric and automatically update when the dashboard or graph is refreshed\. To add a dynamic label, choose **Graphed metrics** and then **Dynamic labels**\.

   By default, the dynamic values you add to the label appear at the beginning of the label\. You can then click the **Label** value for the metric to edit the label\. For more information, see [Use dynamic labels](graph-dynamic-labels.md)\.

1. \(Optional\) To add a single metric to the graph, choose the **All metrics** tab and drill down to the metric you want\.

1. \(Optional\) To change the time range shown on the graph, choose either **custom** at the top of the graph or one of the time periods to the left of **custom**\.

1. <a name="horizontal-annotations"></a> \(Optional\) Horizontal annotations help dashboard users quickly see when a metric has spiked to a certain level or whether the metric is within a predefined range\. To add a horizontal annotation, choose **Graph options** and then **Add horizontal annotation**:

   1. For **Label**, enter a label for the annotation\.

   1. For **Value**, enter the metric value where the horizontal annotation appears\.

   1. For **Fill**, specify whether to use fill shading with this annotation\. For example, choose `Above` or `Below` for the corresponding area to be filled\. If you specify `Between`, another `Value` field appears, and the area of the graph between the two values is filled\.

   1. For **Axis**, specify whether the numbers in `Value` refer to the metric associated with the left y\-axis or the right y\-axis if the graph includes multiple metrics\.

      You can change the fill color of an annotation by choosing the color square in the left column of the annotation\. 

   Repeat these steps to add multiple horizontal annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, choose **x** in the **Actions** column\.

1. <a name="vertical-annotations"></a> \(Optional\) Vertical annotations help you mark milestones in a graph, such as operational events or the beginning and end of a deployment\. To add a vertical annotation, choose **Graph options** and then **Add vertical annotation**:

   1. For **Label**, enter a label for the annotation\. To show only the date and time on the annotation, keep the **Label** field blank\.

   1. For **Date**, specify the date and time where the vertical annotation appears\.

   1. For **Fill**, specify whether to use fill shading before or after a vertical annotation or between two vertical annotations\. For example, choose `Before` or `After` for the corresponding area to be filled\. If you specify `Between`, another `Date` field appears, and the area of the graph between the two values is filled\.

   Repeat these steps to add multiple vertical annotations to the same graph\.

   To hide an annotation, clear the check box in the left column for that annotation\.

   To delete an annotation, choose **x** in the **Actions** column\.

1. Choose **Create widget**\.

1. Choose **Save dashboard**\.

**To use the Metrics view to graph searched metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In the search field, enter the tokens to search for: for example, **cpuutilization t2\.small**\.

   Results that match your search appear\.

1. To graph all of the metrics that match your search, choose **Graph search**\.

   or

   To refine your search, choose one of the namespaces that appeared in your search results\.

1. If you selected a namespace to narrow your results, you can do the following: 

   1. To graph one or more metrics, select the check box next to each metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To refine your search, hover over a metric name and choose **Add to search** or **Search for this only**\.

   1. To view help for a metric, select the metric name and choose **What is this?**\.

   The selected metrics appear on the graph\.

1. \(Optional\) Select one of the buttons in the search bar to edit that part of the search term\.

1. \(Optional\) To add the graph to a dashboard, choose **Actions** and then **Add to dashboard**\.