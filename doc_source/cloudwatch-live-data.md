# Use live data<a name="cloudwatch-live-data"></a>

You can choose whether your metric widgets display *live data*\. Live data is data published within the last minute that has not been fully aggregated\.
+ If live data is turned **off**, only data points with an aggregation period of at least one minute in the past are shown\. For example, when using 5\-minute periods, the data point for 12:35 would be aggregated from 12:35 to 12:40, and displayed at 12:41\.
+ If live data is turned **on**, the most recent data point is shown as soon as any data is published in the corresponding aggregation interval\. Each time you refresh the display, the most recent data point may change as new data within that aggregation period is published\. If you use a cumulative statistic such as **Sum** or **Sample Count**, using live data may result in a dip at the end of your graph\.

You can choose to enable live data for a whole dashboard, or for individual widgets on the dashboard\.

**To choose whether to use live data on your entire dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. To permanently turn live data on or off for all widgets on the dashboard, do the following:

   1. Choose **Actions**, **Settings**, **Bulk update live data\.**

   1. Choose **Live Data on** or **Live Data off**, and choose **Set**\.

1. To temporarily override the live data settings of each widget, choose **Actions**\. Then, under **Overrides**, next to **Live data**, do one of the following:
   + Choose **On** to temporarily turn on live data for all widgets\. 
   + Choose **Off** to temporarily turn off live data for all widgets\.
   + Choose **Do not override** to preserve each widget's live data setting\.

**To choose whether to use live data on a single widget**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards** and select a dashboard\.

1. Select a widget, and choose **Actions**, **Edit**\.

1. Choose the **Graph options** tab\.

1. Select or clear the check box under **Live Data**\.