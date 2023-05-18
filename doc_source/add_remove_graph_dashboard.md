# Add or remove a graph from a CloudWatch dashboard<a name="add_remove_graph_dashboard"></a>

 You can add graphs that contain one or more metrics to your CloudWatch dashboard\. The types of graphs that you can add to your dashboard include ***Line***, ***Stacked area***, ***Number***, ***Gauge***, ***Bar***, and ***Pie***\. You can remove graphs from your dashboard when you don't need them anymore\. The procedures in this section describe how to add and remove graphs from your dashboard\. For information about how to edit a graph on your dashboard, see [ Edit a graph on a CloudWatch dashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/edit_graph_dashboard.html)\. 

**To add a graph to a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  Choose the **\+** symbol, and then choose the type of graph that you want to add to your dashboard, then choose **Next**\. 

   1.  If you select ***Line***, ***Stacked area***, ***Bar***, or ***Pie***, choose **Metrics**\. 

1.  In the **Browse** tab, search or browse for the metrics to graph, and select the ones that you want\. 

1.  \(Optional\) To change your graph's time range, select one of the predefined time ranges in the upper\-right corner of your graph\. The time ranges span from 1 hour to 1 week \(***1h***, ***3h***, ***12h***, ***1d***, ***3d***, or ***1w***\)\. 

    To set your own time range, choose **Custom**\. 

   1. \(Optional\) To have this widget keep using this time range that you select, even if the time range for the rest of the dashboard is later changed, choose **Persist time range**\.

1.  \(Optional\) To change your graph's widget type, use the dropdown that's next to the predefined time ranges\. 

1.  \(Optional\) In **Graphed metrics**, you can add a dynamic label to your metric and change your metric's label, label color, statistic, and period\. You also can determine the position of labels on the Y\-axis from left to right\. 

   1.  To add a dynamic label, choose **Graphed metrics**, and then choose **Add dynamic labels**\. Dynamic labels display a statistic about your metric in the graph legend\. Dynamic labels update automatically when your dashboard or graph refreshes\. By default, the dynamic values that you add to labels appear at the beginning of your labels\. For more information, see [Use dynamic labels](graph-dynamic-labels.md)\. 

   1.  To change the color of a metric, choose the color square that's next to the metric\. 

   1.  To change the statistic, select the dropdown under ***Statistic***, and then choose a new value\. For more information, see [Statistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Statistic)\. 

   1.  To change the period, select the dropdown under the ***Period*** column, and then choose a new value\. 

1. If you are creating a gauge widget, you must choose the **Options** tab and specify the **Min** and **Max** values to use for the two ends of the gauge\.

1.  \(Optional\) To customize the Y\-axis, choose **Options**\. You can add a custom label under ***Left Y\-axis*** in the label field\. If your graph displays values on the right side of the Y\-axis, you can customize that label, too\. You also can set minimum and maximum limits on your Y\-axis values, so that your graph only displays the value ranges that you specify\. 

1.  \(Optional\) To add or edit horizontal annotations to line or stacked area graphs, or to add thresholds to gauge widgets, choose **Options**: 

   1.  To add a horizontal annotation or threshold, choose **Add horizontal annotation** or **Add threshold**\. 

   1.  For ***Label***, enter a label for the annotation then choose the check mark icon\. 

   1.  For ***Value***, choose the pen and paper icon that's next to the current value, and enter your new value\. After you enter your value, choose the check mark icon\.

   1.  For ***Fill***, select the dropdown and specify how your annotation will use shading\. You can choose ***None***, ***Above***, ***Between***, or ***Below***\. To change the fill color, choose the color square that's next to the annotation\. 

   1.  For ***Axis***, specify whether your annotation appears on the left or right side of the Y\-axis\. 

   1.  To hide an annotation, clear the check box that's next to the annotation you want to hide\. 

   1.  To delete an annotation, choose **X** under ***Actions***\. 
**Note**  
 You can repeat these steps to add multiple horizontal annotations or thresholds to the same graph or gauge\. 

1.  \(Optional\) To add or edit vertical annotations, choose **Options**: 

   1.  To add a vertical annotation, choose **Add vertical annotation**\. 

   1.  For ***Label***, choose the pen and paper icon that's next to the current annotation, and enter your new annotation\.  If you want to show only the date and time, leave the label field blank\. 

   1.  For ***Date***, choose the current date and time, and enter the new date and time\. 

   1.  For ***Fill***, select the dropdown, and specify how your annotation will use shading\. You can choose ***None***, ***Above***, ***Between***, or ***Below***\. To change the fill color, select the color square that's next to the annotation\.

   1.  To hide an annotation, clear the check box next to the annotation that you want to hide\. 

   1.  To delete an annotation, choose **X** under ***Actions***\. 
**Note**  
 You can repeat these steps to add multiple vertical annotations to the same graph\. 

1. Choose **Create widget**\.

1. Choose **Save dashboard**\.

**To remove a graph from a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  In the upper\-right corner of the graph that you want to remove, choose **Widget actions**, and then choose **Delete**\. 

1.  Choose **Save dashboard**\. 