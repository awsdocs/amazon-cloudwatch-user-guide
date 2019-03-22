# Focus on Metrics and Alarms in a Single AWS Service<a name="CloudWatch_Automatic_Dashboards_Focus_Service"></a>

On the CloudWatch home page, you can focus the view to a single AWS service\. You can drill down further by focusing on both a single AWS service and a resource group at the same time\. The following procedure shows only how to focus on an AWS service\.

**To focus on a single service**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

   The home page appears\.

1. Choose the service name from the list at the top of the screen, where **Overview** is currently shown\.

   The view changes to display graphs of key metrics from the selected service\.

1. To switch to viewing the alarms for this service, choose ** Alarms dashboard** at the top of the screen where **Service dashboard** is currently displayed\.

1. When viewing metrics, you can focus on a particular metric in several ways:

   1. To see more details about the metrics in any graph, hover on the graph, and choose the actions icon, **View in metrics**\.

      The graph appears in a new tab, with the relevant metrics listed below the graph\. You can customize your view of this graph, changing the metrics and resources shown, the statistic, the period, and other factors to get a better understanding of the current situation\.

   1. You can view log events from the time range shown in the graph\. This may help you discover events that happened in your infrastructure that are causing an unexpected change in your metrics\.

      To see the log events, hover on the graph, and choose the actions icon, **View in logs**\.

      The CloudWatch Logs view appears in a new tab, displaying a list of your log groups\. To see the log events in one of these log groups that occurred during the time range shown in the original graph, choose that log group\.

1. When viewing alarms, you can focus on a particular alarm in several ways:

   1. To see more details about an alarm, hover on the alarm, and choose the actions icon, **View in alarms**\.

     The alarms view appears in a new tab, displaying a list of your alarms, along with details about the chosen alarm\. To see the history for this alarm, choose the **History** tab\.

1. Alarms are always refreshed one time per minute\. To refresh the view, choose the refresh icon \(two curved arrows\) at the top right of the screen\. To change the automatic refresh rate for items on the screen other than alarms, choose the down arrow next to the refresh icon and choose a refresh ratet\. You can also choose to turn off automatic refresh\.

1. To change the time range shown in all graphs and alarms currently displayed, next to **Time range** at the top of the screen, choose the range \. To select from more time range options than those displayed by default, choose **custom**\.

1. To return to the cross\-service dashboard, choose **Overview** in the list at the top of the screen that currently shows the service you are focusing on\.

   Alternatively, from any view, you can choose **CloudWatch** at the top of the screen to clear all filters and return to the overview page\.