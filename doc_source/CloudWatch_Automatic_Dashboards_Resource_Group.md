# Focus on metrics and alarms in a resource group<a name="CloudWatch_Automatic_Dashboards_Resource_Group"></a>

You can focus your view to display metrics and alarms from a single resource group\. Using resource groups enables you to use tags to organize projects, focus on a subset of your architecture, or distinguish between your production and development environments\. They also enable you to focus on each of these resource groups on the CloudWatch overview\. For more information, see [What Is AWS Resource Groups?](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)\.

When you focus on a resource group, the display changes to show only the services where you have tagged resources as part of this resource group\. The recent alarms area shows only the alarms that are associated with resources that are part of the resource group\. Additionally, if you have created a dashboard with the name **CloudWatch\-Default\-*ResourceGroupName***, it is displayed in the **Default dashboard** area\.

You can drill down further by focusing on both a single AWS service and a resource group at the same time\. The following procedure shows just how to focus on a resource group\.

**To focus on a single resource group**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. At the top of the page, where **All resources** is displayed, choose a resource group\.

1. To see more metrics related to this resource group, near the bottom of the screen, choose **View cross service dashboard**\.

   The cross\-service dashboard appears, showing only the services related to this resource group\. For each service, one or two key metrics are displayed\.

1. To change the time range shown in all graphs and alarms currently displayed, for **Time range** at the top of the screen, select a range\. To select from more time range options than those displayed by default, choose **custom**\.

1. Alarms are always refreshed one time per minute\. To refresh the view, choose the refresh icon \(two curved arrows\) at the top right of the screen\. To change the automatic refresh rate for items on the screen other than alarms, choose the down arrow next to the refresh icon and choose a refresh rate\. You can also choose to turn off automatic refresh\.

1. To return to showing information about all the resources in your account, near the top of the screen where the name of the resource group is currently displayed, choose **All resources**\. 