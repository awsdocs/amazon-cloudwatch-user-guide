# See key metrics from all AWS services<a name="CloudWatch_Automatic_Dashboards_Cross_Service"></a>

 You can switch to the Cross\-service dashboard screen and interact with dashboards for all of the AWS services that you're using\. The CloudWatch Console displays your dashboards in alphabetical order and shows one or two key metrics on each dashboard\. 

**Note**  
 If you're using five or more AWS services, the CloudWatch Console won't display the Cross\-service dashboard on the Overview screen\. 

**To open the Cross\-service dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

    You're directed to the Overview screen\. 

1.  From the Overview screen, select the dropdown that reads **Overview**, and then choose **Cross service dashboard**\.

    You're directed to the Cross service dashboard screen\. 

1.  \(Optional\) If you're using the original interface, scroll to the section **Cross\-service dashboard**, and then choose **View Cross\-service dashboard**\. 

    You're directed to the Cross\-service dashboard screen\. 

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