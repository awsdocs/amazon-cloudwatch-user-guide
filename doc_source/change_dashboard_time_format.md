# Change the Time Range or Time Zone Format of a CloudWatch Dashboard<a name="change_dashboard_time_format"></a>

You can change the time range to display dashboard data over minutes, hours, days, or weeks\. You can also change the time format to display dashboard data in UTC or local time\. Local time is the time zone specified as your local time zone in the operating system of the computer you are using to view the CloudWatch console\.

**Note**  
If you create a dashboard with graphs that contain close to 100 or more high\-resolution metrics, we recommend that you set the time range to no longer than one hour to ensure good dashboard performance\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

**To change the dashboard time range**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Do one of the following:
   + Select one of the predefined ranges shown, which span from 1 hour to 1 week: 1h, 3h, 12h, 1d, 3d, or 1w\.
   + Choose **custom** and then **Relative**\. Select one of the predefined ranges, which span from 1 minute to 15 months\.
   + Choose **custom** and then **Absolute**\. Use the calendar picker or the text fields to specify the time range\.

**Note**  
When you change the time range of a graph while the aggregation period is set to **Auto**, CloudWatch might change the period\. To manually set the period, choose **Actions** and select a new value for **Period:**\.

**To change the dashboard time format**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Choose **custom**\.

1. From the upper corner, choose **UTC** or **Local timezone**\.