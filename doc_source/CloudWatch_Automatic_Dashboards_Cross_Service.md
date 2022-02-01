# See key metrics from all AWS services<a name="CloudWatch_Automatic_Dashboards_Cross_Service"></a>

If you use six or more AWS services, the cross\-service dashboard is not displayed on the overview page\. You can switch to this dashboard to see key metrics from all the AWS services that you are using\.

**To open the cross\-service dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

   The overview appears\.

1. Near the bottom of the page, choose **View cross service dashboard**\.

   The cross\-service dashboard appears, showing each AWS service you are using, displayed in alphabetical order\. For each service, one or two key metrics are displayed\.

1. You can focus on a particular service in two ways:

   1. To see more key metrics for a service, choose its name from the list at the top of the screen, where **Cross service dashboard** is currently shown\. Or, you can choose **View *Service* dashboard** next to the service name\.

      An automatic dashboard for that service is displayed, showing more metrics for that service\. Additionally, for some services, the bottom of the service dashboard displays resources related to that service\. You can choose one of those resources to that service console and focus further on that resource\.

   1. To see all the alarms related to a service, choose the button on the right of the screen next to that service name\. The text on this button indicates how many alarms you have created in this service, and whether any are in the ALARM state\.

      When the alarms are displayed, multiple alarms that have similar settings \(such as dimensions, threshold, or period\) may be shown in a single graph\.

      You can then view details about an alarm and see the alarm history\. To do so, hover on the alarm graph, and choose the actions icon, **View in alarms**\.

      The alarms view appears in a new browser tab, displaying a list of your alarms, along with details about the chosen alarm\. To see the history for this alarm, choose the **History** tab\.

1. You can focus on resources in a particular resource group\. To do so, choose the resource group from the list at the top of the page where **All resources** is displayed\.

   For more information, see [Focus on metrics and alarms in a resource group](CloudWatch_Automatic_Dashboards_Resource_Group.md)\.

1. To change the time range shown in all graphs and alarms currently displayed, select the range you want next to **Time range** at the top of the screen\. Choose **custom** to select from more time range options than those displayed by default\.

1. Alarms are always refreshed once a minute\. To refresh the view, choose the refresh icon \(two curved arrows\) at the top right of the screen\. To change the automatic refresh rate for items on the screen other than alarms, choose the down arrow next to the refresh icon and choose the refresh rate you want\. You can also choose to turn off automatic refresh\.

## Remove a service from appearing in the cross\-service dashboard<a name="Remove_service_from_Cross_Service_Dashboard"></a>

You can prevent a service's metrics from appearing in the cross\-service dashboard\. This helps you focus your cross\-service dashboard on the services you most want to monitor\.

If you remove a service from the cross\-service dashboard, the alarms for that service still appear in the views of your alarms\.

**To remove a service's metrics from the cross\-service dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

   The home page appears\.

1. At the top of the page, under **Overview**, choose the service you want to remove\.

   The view changes to show metrics from only that service\.

1. Choose **Actions**, then clear the check box next to **Show on cross service dashboard**\.