# Edit a graph on a CloudWatch dashboard<a name="edit_graph_dashboard"></a>

 You can edit the graphs that you add to your CloudWatch dashboard\. You can change a graph's title, statistic, or period\. You can add, update, and remove metrics from your graphs\. If your graph contains more than one metric, you can reduce clutter by hiding metrics that you aren't using\. The procedures in this section describe how to edit a graph on your dashboard\. For information about creating a graph, see [Add or remove a graph from a CloudWatch dashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/add_remove_graph_dashboard.html)\. 

------
#### [  New interface  ]

**To edit a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  In the upper\-right corner of the graph that you want to edit, choose **Widget actions**, and then choose **Edit**\. 

1.  To change the graph's title, choose the pen and paper icon that's next to the current title\. Enter the new title, and then choose **Apply**\. 

1.  \(Optional\) To change your graph's time range, select one of the predefined time ranges in the upper\-right corner of your graph\. The time ranges span from 1 hour to 1 week \(***1h***, ***3h***, ***12h***, ***1d***, ***3d***, or ***1w***\)\. 

    To set your own time range, choose **Custom**\. 

   1. \(Optional\) To have this widget keep using this time range that you select, even if the time range for the rest of the dashboard is later changed, choose **Persist time range**\.

1.  To change your graph's widget type, use dropdown that's next to the predefined time ranges\. 

1.  In **Graphed metrics**, you can add a dynamic label to your metric and change your metric's label, label color, statistic, and period\. You also can determine the position of labels on the Y\-axis from left to right\. 

   1.  To add a dynamic label for a metric, choose **Dynamic labels**\. Dynamic labels display a statistic about the metric in the graph legend\. Dynamic labels update automatically when your dashboard or graph refreshes\. By default, the dynamic values that you add to labels appear at the beginning of the labels\. For more information, see [Use dynamic labels](graph-dynamic-labels.md)\. 

   1.  To change the color of a metric, choose the color square that's next to the metric\. 

   1.  To change the statistic, choose the statistic value under the ***Statistic*** column, and then choose a new value\. For more information, see [Statistics](cloudwatch_concepts.md#Statistic)\. 

   1.  To change the period, choose the period value under the ***Period*** column, and then choose a new value\. 

1.  To add or edit horizontal annotations, choose **Options**: 

   1.  To add a horizontal annotation, choose **Add horizontal annotation**\. 

   1.  For **Label**, choose the pen and paper icon next to the current annotation\. Then enter your new annotation\. After you enter your annotation, choose the check mark icon\. 

   1.  For **Value**, choose the pen and paper icon next to the current metric value\. Then enter your new metric value\. After you enter your value, select the check mark icon\. 

   1.  For **Fill**, choose the dropdown under the column, and then specify how your annotation will use shading\. You can choose **None**, **Above**, **Between**, or **Below**\. If you choose **Between**, another new label and value field appears\. 
**Tip**  
 You can change the fill color by choosing the colored square next to the annotation\. 

   1.  For **Axis**, specify whether your annotation appears on the left or right side of the Y\-axis\. 

   1.  To hide an annotation, deselect the check box next to the annotation that you want to hide on the graph\. 

   1.  To delete an annotation, choose **X** under the **Actions** column\. 
**Note**  
 You can repeat these steps to add multiple horizontal annotations to the same graph\. 

1.  To add or edit vertical annotations, choose **Options**: 

   1.  To add a vertical annotation, choose **Add vertical annotation**\. 

   1.  For **Label**, choose the pen and paper icon next to the current annotation\. Then enter your new annotation\. After you enter your annotation, choose the check mark icon\. 
**Tip**  
 To show only the date and time, leave the label field blank\. 

   1.  For **Date**, choose the current date and time\. Then enter the new date and time\. 

   1.  For **Fill**, choose the dropdown under the column, and then specify how your annotation will use shading\. You can choose **None**, **Above**, **Between**, or **Below**\. If you choose **Between**, a new label and value field appears\. 
**Tip**  
 You can change the fill color by choosing the color square next to the annotation\. 
**Note**  
 You can repeat these steps to add multiple vertical annotations to the same graph\. 

   1.  To hide an annotation, deselect the check box next to the annotation that you want to hide on the graph\. 

   1.  To delete an annotation, choose **X** under the **Actions** column\. 

1.  To customize the Y\-axis, choose **Options**\. Under ***Left Y\-axis***, you can enter a custom label for **Label**\. If the graph displays values on the right Y\-axis, you can customize that label, too\. You also can set minimums and maximums on the Y\-axis values, so that the graph displays only the value range that you specify\. 

1.  When you finish making changes, choose **Update widget**\. 

**To hide or change the position of a graph legend**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  In the upper\-right corner of the graph that you want to edit, choose **Widget actions**\. Choose **Legend** and select **Hidden**, **Bottom**, or **Right**\. 

**To temporarily hide metrics for a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  Select the color square for the metric that you want to hide in the graph's footer\. An ***X*** appears in the color square when you hover over it, and the square turns gray when you choose it\. 

1.  To restore the hidden metric, clear the **X** in the gray square\. 

------
#### [  Original interface  ]

**To edit a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  Hover over the upper\-right corner of the graph that you want to edit\. Choose **Widget actions**, and then choose **Edit**\. 

1.  To change the graph's title, choose the pencil icon that's next to the current title, and then enter the new title\. 

1.  To change the graph's time range, choose one of the predefined time ranges in the upper\-right corner of the graph\. These span from 1 hour to 1 week \(***1h***, ***3h***, ***12h***, ***1d***, ***3d***, or ***1w***\)\. 

   1.  To set your own time range, choose **custom**\. 

1.  To change your graph's widget type, select the **Graph options** tab\. You can choose ***Line***, ***Stacked area***, ***Number***, ***Bar***, or ***Pie***\. 
**Tip**  
 You can change your graph's widget type by choosing the dropdown that's next to the predefined time ranges\. 

1.  In **Graphed metrics**, you can add a dynamic label to your metric and change your metric's label, label color, statistic, and period\. You also can determine the position of labels on the Y\-axis from left to right\. 

   1.  To add a dynamic label for a metric, choose **Dynamic labels**\. Dynamic labels display a statistic about the metric in the graph legend\. Dynamic labels update automatically when your dashboard or graph refreshes\. By default, the dynamic values that you add to labels appear at the beginning of the labels\. For more information, see [Use dynamic labels](graph-dynamic-labels.md)\. 

   1.  To change the color of a metric, choose the color square that's next to the metric\. 

   1.  To change the statistic, choose the statistic value under the ***Statistic*** column, and then choose a new value\. For more information, see [Statistics](cloudwatch_concepts.md#Statistic)\. 

   1.  To change the period, choose the period value under the ***Period*** column, and then choose a new value\. 

1.  To add or edit horizontal annotations, choose **Graph options**: 

   1.  To add a horizontal annotation, choose **Add horizontal annotation**\. 

   1.  For **Label**, choose the pencil icon next to the current annotation\. Then enter your new annotation\. After you enter your annotation, choose the check mark icon\. 

   1.  For **Value**, choose the pencil icon next to the current metric value\. Then enter your new metric value\. After you enter your value, select the check mark icon\. 

   1.  For **Fill**, choose the dropdown under the column, and then specify how your annotation will use shading\. You can choose **None**, **Above**, **Between**, or **Below**\. If you choose **Between**, a new label and value field appears\. 
**Tip**  
 You can change the fill color by choosing the color square next to the annotation\. 

   1.  For **Axis**, specify whether your annotation appears on the left or right side of the Y\-axis\. 

   1.  To hide an annotation, deselect the check box next to the annotation that you want to hide on the graph\. 

   1.  To delete an annotation, choose **X** under the **Actions** column\. 
**Note**  
 You can repeat these steps to add multiple horizontal annotations to the same graph\. 

1.  To add or edit vertical annotations, choose **Graph options**: 

   1.  To add a vertical annotation, choose **Add vertical annotation**\. 

   1.  For **Label**, choose the pencil icon next to the current annotation\. Then enter your new annotation\. After you enter your annotation, choose the check mark icon\. 
**Tip**  
 To show only the date and time, leave the label field blank\. 

   1.  For **Date**, choose the pencil icon next to the current date and time\. Then enter the new date and time\. 

   1.  For **Fill**, choose the dropdown under the column, and then specify how your annotation will use shading\. You can choose **None**, **Above**, **Between**, or **Below**\. If you choose **Between**, a new label and value field appears\. 
**Tip**  
 You can change the fill color by choosing the color square next to the annotation\. 
**Note**  
 You can repeat these steps to add multiple vertical annotations to the same graph\. 

   1.  To hide an annotation, deselect the check box next to the annotation that you want to hide on the graph\. 

   1.  To delete an annotation, choose **X** under the **Actions** column\. 

1.  To customize the Y\-axis, choose **Graph options**\. Under ***Left Y\-axis***, you can enter a custom label for **Label**\. If the graph displays values on the right Y\-axis, you can customize that label, too\. You also can set minimums and maximums on the Y\-axis values, so that the graph displays only the value range that you specify\. 

1.  When you finish making changes, choose **Update widget**\. 

**To hide or change the position of a graph legend**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  Hover over the upper\-right corner of the graph that you want to edit, and choose **Widget actions**\. Choose **Legend**, and select **Hidden**, **Bottom**, or **Right**\. 

**To temporarily hide metrics for a graph on a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  Select the color square for the metric that you want to hide in the graph's footer\. An ***X*** appears in the color square when you hover over it, and the square turns gray when you choose it\. 

1.  To restore the hidden metric, clear the **X** in the gray square\. 

------