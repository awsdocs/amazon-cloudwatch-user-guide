# Add or remove a number widget from a CloudWatch dashboard<a name="add_remove_number_dashboard"></a>

 With the number widget, you can look at the latest metric values and trends as soon as they appear\. Because the number widget includes the sparkline feature, you can visualize the top and bottom halves of metric trends in a single graph\. The procedures in this section describe how to add and remove a number widget from a CloudWatch dashboard\. 

![\[A screenshot of the number widget that focuses on the sparkline feature\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/widget_number.png)

**Note**  
 Only the new interface supports the sparkline feature\. When you create a number widget, the sparkline feature is automatically included\. 

**To add a number widget to a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose a dashboard\. 

1.  Choose the **\+** symbol, and select **Number**\. 

1.  In the **Browse** tab, search or browse for the metric that you want to display\. 

1.  \(Optional\) To change the color of the sparkline feature, choose **Graphed metrics**, and select the color box next to the metric label\. A menu appears where you can choose a different color or enter a six\-digit hex color code to specify a color\. 

1.  \(Optional\) To turn off the sparkline feature, choose **Options**\. Under ***Sparkline***, the check box\. 

1.  \(Optional\) To change your number widget's time range, select one of the predefined time ranges in the upper\-right corner of your graph\. The time ranges span from 1 hour to 1 week \(***1h***, ***3h***, ***12h***, ***1d***, ***3d***, or ***1w***\)\. 

    To set your own time range, choose **Custom**\. 

   1. \(Optional\) To have this widget keep using this time range that you select, even if the time range for the rest of the dashboard is later changed, choose **Persist time range**\.

1.  Choose **Create widget**, and choose **Save dashboard**\. 

**Tip**  
 You can turn off the sparkline feature from the number widget on the dashboard screen\. In the upper\-right corner of the number widget that you want to modify, choose **Widget actions**\. Select **Sparkline**, and then choose **Hide sparkline**\. 

**To remove a number widget from a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  In the navigation pane, choose **Dashboards**, and then choose the dashboard that contains the number widget that you want to delete\. 

1.  In the upper\-right corner of the number widget that you want to remove, choose **Widget actions**, and then choose **Delete**\. 

1.  Choose **Save dashboard**\. 