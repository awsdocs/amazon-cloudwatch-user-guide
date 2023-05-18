# Add or remove an alarm widget from a CloudWatch dashboard<a name="add_remove_alarm_dashboard"></a>

To add an alarm widget to a dashboard, choose one of the following options:
+ Add a single alarm in a widget, which displays the graph of the alarm's metric and also displays the alarm status\.
+ Add an *alarm status widget*, which displays the status of multiple alarms in a grid\. Only the alarm names and current status are displayed, Graphs are not displayed\. You can include up to 100 alarms in one alarm status widget\.

**To add a single alarm, including its graph, to a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, select the alarm to add, and then choose **Add to Dashboard**\.

1. Select a dashboard, choose a widget type \(**Line**, **Stacked area**, or **Number**\), and then choose **Add to dashboard**\.

1. To see your alarm on the dashboard, choose **Dashboards** in the navigation pane and select the dashboard\.

1. \(Optional\) To temporarily make an alarm graph larger, select the graph\.

1. \(Optional\) To change the widget type, hover over the title of the graph, choose **Widget actions**, and then choose **Widget type**\.

**To add an alarm status widget to a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**, and then choose a dashboard\.

1. Choose the **\+** symbol\.

1. Choose **Alarm status**\.

1. Select the check boxes next to the alarms that you want to add to the widget, and then choose **Create widget**\.

1. Choose **Add to dashboard**\.

**To remove an alarm widget from a dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**, and then choose a dashboard\.

1. Hover over the widget, choose **Widget actions**, and then choose **Delete**\.

1. Choose **Save dashboard**\. If you attempt to navigate away from the dashboard before you save your changes, you're prompted to either save or discard your changes\.