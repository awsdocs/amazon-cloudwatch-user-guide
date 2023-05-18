# Change the period override setting or refresh interval for the CloudWatch dashboard<a name="change_dashboard_refresh_interval"></a>

You can specify how the period setting of graphs added to this dashboard are retained or modified\.

**To change the period override options**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Actions**\.

1. Under **Period override**, choose one of the following:
   + Choose **Auto** to have the period of the metrics on each graph automatically adapt to the dashboard's time range\.
   + Choose **Do not override** to ensure that the period setting of each graph is always obeyed\.
   + Choose one of the other options to cause graphs added to the dashboard to always adapt that chosen time as their period setting\.

   The **Period override** always reverts to **Auto** when the dashboard is closed or the browser is refreshed\. Different settings for **Period override** can't be saved\.

You can change how often the data on your CloudWatch dashboard is refreshed or set it to automatically refresh\.

**To change the dashboard refresh interval**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**, and then choose a dashboard\.

1. On the **Refresh options** menu \(upper\-right corner\), choose **10 Seconds**, **1 Minute**, **2 Minutes**, **5 Minutes**, or **15 Minutes**\.

**To automatically refresh the dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**, and then choose a dashboard\.

1. Choose **Refresh options** and then **Auto refresh**\.