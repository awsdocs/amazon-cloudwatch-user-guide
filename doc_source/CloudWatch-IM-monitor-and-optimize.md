# Monitor and optimize with the Internet Monitor dashboard<a name="CloudWatch-IM-monitor-and-optimize"></a>

This section describes how to filter and view information on the Amazon CloudWatch Internet Monitor dashboard to visualize and get insights about your AWS application's internet traffic and setup\.

After you create a monitor to monitor your application's internet performance and availability, Amazon CloudWatch Internet Monitor publishes CloudWatch logs containing internet measurements for client location\-ASN pairs, and publishes aggregated CloudWatch metrics for traffic to your application, and to each AWS Region and edge location\. You can filter, explore, and get action\-oriented suggestions from this information from Internet Monitor in several different ways\.

To get started, on the CloudWatch console, under **Application monitoring**, choose **Internet Monitor**\.

**Note**  
This section describes how to filter and view Internet Monitor metrics using the AWS Management Console but alternatively, you can use Internet Monitor API actions with the AWS CLI or an SDK to work with information in the log files\. For more information, see [Amazon CloudWatch Internet Monitor API Reference](https://docs.aws.amazon.com/internet-monitor/latest/api/Welcome.html)\.

There are three tabs in the Internet Monitor dashboard\.
+ On the **Overview** tab, you can see current and historical performance and availability information about your application, and health events impacting your client locations\.
+ On the next tab, **Historical explorer**, you can filter by location, ASN, date, and so on, and visualize metrics for your internet traffic over time, using the graphs\.
+ On the **Traffic insights** tab, in addition to viewing information about top monitored traffic summarized in several customizable ways, you can get suggestions for optimized setups to improve performance for different location and ASN pairs\. Internet Monitor predicts your application's performance improvement, based on your traffic patterns and past performance, when you change how you route your traffic or the AWS resources you use\. 

In addition, because Internet Monitor generates and publishes log files with the measurements about your traffic, you can use other CloudWatch tools in the console to further visualize the data published by Internet Monitor, including CloudWatch Contributor Insights, CloudWatch Metrics, and CloudWatch Logs Insights\. For more information, see [Viewing log files with CloudWatch tools](CloudWatch-IM-view-cw-tools.md)\.

Learn about using Internet Monitor to explore your performance and availability measurements in the following sections\.

**Topics**
+ [Tracking real\-time performance and availability in Amazon CloudWatch Internet Monitor \(Overview tab\)](CloudWatch-IM-overview.md)
+ [Filtering and viewing historical data in Amazon CloudWatch Internet Monitor \(Historical explorer tab\)](CloudWatch-IM-historical-explorer.md)
+ [Getting insights to improve application performance in Amazon CloudWatch Internet Monitor \(Traffic insights tab\)](CloudWatch-IM-insights.md)