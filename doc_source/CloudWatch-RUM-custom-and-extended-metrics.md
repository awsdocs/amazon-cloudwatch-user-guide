# Custom metrics and extended metrics that you can send to CloudWatch and CloudWatch Evidently<a name="CloudWatch-RUM-custom-and-extended-metrics"></a>

By default, RUM app monitors send metrics to CloudWatch\. These default metrics and dimensions are listed in [CloudWatch metrics that you can collect with CloudWatch RUM](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-RUM-metrics.html)\.

You can also set up an app monitor to send extended metrics, custom metrics, or both to CloudWatch or to CloudWatch Evidently\.
+ **Custom metrics**– Custom metrics are metrics that you define\. With custom metrics, you can use any metric name and namespace\. To derive the metrics, you can use any custom events, built\-in events, custom attributes, or default attributes\.

  You can send custom metrics to both CloudWatch and CloudWatch Evidently\.
+ **Extended metrics**– Lets you send the default CloudWatch RUM metrics to CloudWatch Evidently to be used in Evidently experiments\. You can also send any of the default CloudWatch RUM metrics to CloudWatch with additional dimensions\. This way, these metrics can give you a more fine\-grained view\.

**Topics**
+ [Custom metrics](#CloudWatch-RUM-custom-metrics)
+ [Extended metrics](#CloudWatch-RUM-vended-metrics)

## Custom metrics<a name="CloudWatch-RUM-custom-metrics"></a>

To send custom metrics, you must use the AWS APIs or AWS CLI instead of the console\. For more information about using the AWS APIs, see [PutRumMetricsDestination](https://docs.aws.amazon.com/cloudwatchrum/latest/APIReference/API_PutRumMetricsDestination.html) and [BatchCreateRumMetricDefinitions](https://docs.aws.amazon.com/cloudwatchrum/latest/APIReference/API_BatchCreateRumMetricDefinitions.html)\.

The maximum number of extended metric and custom metric definitions that one destination can contain is 2000\. For each custom metric or extended metric that you send to each destination, each combination of dimension name and dimension value counts toward this limit\. This also counts as a CloudWatch custom metric for pricing\.

The following example shows how to create a custom metric derived from a custom event\. Here is the example custom event that is used:

```
cwr('recordEvent', {
    type: 'my_custom_event', 
    data: {
        location: 'IAD', 
        current_url: 'amazonaws.com', 
        user_interaction: {
            interaction_1 : "click",
            interaction_2 : "scroll"
        }, 
        visit_count:10
    }
})
```

Given this custom event, you can create a custom metric that counts the number of visits to the `amazonaws.com` URL from Chrome browsers\. The following definition creates a metric named `AmazonVisitsCount` in your account, in the `RUM/CustomMetrics/PageVisits` namespace\.

```
{
    "AppMonitorName":"customer-appMonitor-name",
    "Destination":"CloudWatch",
    "MetricDefinitions":[
        {
            "Name":"AmazonVisitsCount",
            "Namespace":"PageVisit",
            "ValueKey":"event_details.visit_count",
            "UnitLabel":"Count",
            "DimensionKeys":{
                "event_details.current_url": "URL"
            },
            "EventPattern":"{\"metadata\":{\"browserName\":[\"Chrome\"]},\"event_type\":[\"my_custom_event\"],\"event_details\": {\"current_url\": [\"amazonaws.com\"]}}" 
        }
    ]
}
```

## Extended metrics<a name="CloudWatch-RUM-vended-metrics"></a>

If you set up extended metrics, you can do one or both of the following:
+ Send default CloudWatch RUM metrics to CloudWatch Evidently to be used in Evidently experiments\. Only the **PerformanceNavigationDuration**, **PerformanceResourceDuration**, **WebVitalsCumulativeLayoutShift**, **WebVitalsFirstInputDelay**, and **WebVitalsLargestContentfulPaint** metrics can be sent to Evidently\.
+ Send any of the default CloudWatch RUM metrics to CloudWatch with additional dimensions so that the metrics give you a more fine\-grained view\. For example, you can see metrics specific to a certain browser that's used by your users, or metrics for users in a specific geolocation\.

For more information about the default CloudWatch RUM metrics, see [CloudWatch metrics that you can collect with CloudWatch RUM](CloudWatch-RUM-metrics.md)\.

The maximum number of extended metric and custom metric definitions that one destination can contain is 2000\. For each extended or custom metric that you send to each destination, each combination of dimension name and dimension value counts as an extended metric for this limit\. This also counts as a CloudWatch custom metric for pricing\.

When you send extended metrics to CloudWatch, you can use the CloudWatch RUM console to create CloudWatch alarms on them\.

Extended metrics are charged as CloudWatch custom metrics\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.



The following dimensions are supported for extended metrics for all the metric names that app monitors can send\. These metric names are listed in [CloudWatch metrics that you can collect with CloudWatch RUM](CloudWatch-RUM-metrics.md)\.
+ `BrowserName`

  Example dimension values: `Chrome`, `Firefox`, `Chrome Headless`
+ `CountryCode` This uses the ISO\-3166 format, with two\-letter codes\.

  Example dimension values: `US`, `JP`, `DE`
+ `DeviceType`

  Example dimension values: `desktop`, `mobile`, `tablet`, `embedded`
+ `FileType`

  Example dimension values: `Image`, `Stylesheet`
+ `OSName`

  Example dimension values: `Linux`, `Windows, ``iOS`, `Android`
+ `PageId`

### Set up extended metrics using the console<a name="CloudWatch-RUM-extended-metrics-console"></a>

To use the console to send extended metrics to CloudWatch, use the following steps\.

To send extended metrics to CloudWatch Evidently, you must use the AWS APIs or AWS CLI instead of the console\. For information about using the AWS APIs to send extended metrics to either CloudWatch or Evidently, see [PutRumMetricsDestination](https://docs.aws.amazon.com/cloudwatchrum/latest/APIReference/API_PutRumMetricsDestination.html) and [BatchCreateRumMetricDefinitions](https://docs.aws.amazon.com/cloudwatchrum/latest/APIReference/API_BatchCreateRumMetricDefinitions.html)\.

**To use the console to set up an app monitor and send RUM extended metrics to CloudWatch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **RUM**\.

1. Choose **List view** and then choose the name of the app monitor that is to send the metrics\.

1. Choose the **Configuration** tab and then choose **RUM extended metrics**\.

1. Choose **Send metrics**\.

1. Select one or more metric names to send with additional dimensions\. 

1. Select one or more factors to use as dimensions for these metrics\. As you make your choices, the number of extended metrics that your choices create is displayed in **Number of extended metrics**\.

   This number is calculated by multiplying the number of chosen metric names by the number of different dimensions that you create\. This number represents how many custom metrics you are charged for\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. 

   1. To send a metric with page ID as a dimension, choose **Browse for page ID** and then select the page IDs to use\.

   1. To send a metric with device type as a dimension, choose either **Desktop devices** or **Mobile and tablets**\.

   1. To send a metric with operating system as a dimension, select one or more operating systems under **Operating system**\. 

   1. To send a metric with browser type as a dimension, select one or more browsers under **Browsers**\.

   1. To send a metric with geolocation as a dimension, select one or more locations under **Locations**\.

      Only the locations where this app monitor has reported metrics from will appear in the list to choose from\.

1. When you are finished with your choices, choose **Send metrics**\.

1. \(Optional\) In the **Extended metrics** list, to create an alarm that watches one of the metrics, choose **Create alarm** in that metric's row\.

   For general information about CloudWatch alarms, see [ Using Amazon CloudWatch alarms](AlarmThatSendsEmail.md)\. For a tutorial for setting an alarm on a CloudWatch RUM extended metric, see [Tutorial: create an extended metric and alarm it](#CloudWatch-RUM-extended-metrics-alarmtutorial)\.

**Stop sending extended metrics**

**To use the console to stop sending extended metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **RUM**\.

1. Choose **List view** and then choose the name of the app monitor that is to send the metrics\.

1. Choose the **Configuration** tab and then choose **RUM extended metrics**\.

1. Select one or more metric name and dimension combinations to stop sending\. Then choose **Actions**, **Delete**\.

### Tutorial: create an extended metric and alarm it<a name="CloudWatch-RUM-extended-metrics-alarmtutorial"></a>

This tutorial demonstrates how to set up an extended metric to be sent to CloudWatch, and then how to set an alarm on that metric\. In this tutorial, you create a metric that tracks JavaScript errors on the Chrome browser\.

**To set up this extended metric and set an alarm on it**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **RUM**\.

1. Choose **List view** and then choose the name of the app monitor that is to send the metric\.

1. Choose the **Configuration** tab and then choose **RUM extended metrics**\.

1. Choose **Send metrics**\.

1. Select **JSErrorCount**\.

1. Under **Browsers**, select **Chrome**\.

   This combination of **JSErrorCount** and **Chrome** will send one extended metric to CloudWatch\. The metric counts JavaScript errors only for user sessions that use the Chrome browser\. The metric name will be **JsErrorCount** and the dimension name will be **Browser**\.

1. Choose **Send metrics**\.

1. In the **Extended metrics** list, choose **Create alarm** in the row that displays **JsErrorCount** under **Name** and displays **Chrome** under **BrowserName**\.

1. Under **Specify metric and conditions**, confirm that the **Metric name** and **BrowserName** fields are pre\-filled with the correct values\.

1. For **Statistic**, select the statistic that you want to use for the alarm\. **Average** is a good choice for this type of counting metric\.

1. For **Period**, select **5 minutes**\.

1. Under **Conditions**, do the following:
   + Choose **Static**\.
   + Choose **Greater** to specify that the alarm should go into ALARM state when the number of errors is higher than the threshold you are about to specify\.
   + Under **than\.\.\.**, enter the number for the alarm threshold\. The alarm goes into ALARM state when the number of errors over a 5\-minute period exceeds this number\.

1. \(Optional\) By default, the alarm goes into ALARM state as soon as the number of errors exceeds the threshold number you set during a 5\-minute period\. You can optionally change this so that the alarm goes into ALARM state only if this number is exceeded for more than one 5\-minute period\.

   To do so, choose **Additional configuration** and then for **Datapoints to alarm**, specify how many 5\-minute periods need to have the error number over the threshold to trigger the alarm\. For example, you can select 2 out of 2 to have the alarm trigger only when two consecutive 5\-minute periods are over the threshold, or 2 out of 3 to have the alarm trigger if any two of three consecutive 5\-minute periods are over the threshold\. 

   For more information about this type of alarm evaluation, see [Evaluating an alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

1. Choose **Next**\.

1. For **Configure actions**, specify what should happen when the alarm goes into alarm state\. To receive a notification with Amazon SNS, do the following:
   + Choose **Add notification**\.
   + Choose **In alarm**\.
   + Either select an existing SNS topic or create a new one\. If you create a new one, specify a name for it and add at least one email address to it\.

1. Choose **Next**\.

1. Enter a name and optional description for the alarm, and choose **Next**\.

1. Review the details and choose **Create alarm**\.