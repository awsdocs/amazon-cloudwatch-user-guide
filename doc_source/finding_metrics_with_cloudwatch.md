# Search for Available Metrics<a name="finding_metrics_with_cloudwatch"></a>

You can search within all the metrics in your account using targeted search terms\. Metrics are returned that have matching results within their namespace, metric name, or dimensions\.

**To search for available metrics in CloudWatch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In the search field on the **All metrics** tab, type a search term, such as a metric name, service name, or resource name, and press Enter\. This shows you all the namespaces with metrics with this search term\.

   For example, if you search for **volume**, this shows the namespaces that contain metrics with this term in their name\.

1. Select a namespace with results for your search to view the metrics\. You can do the following:

   1. To graph one or more metrics, select the check box next to each metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To view one of the resources in its console, choose the resource ID and then choose **Jump to resource**\.

   1. To view help for a metric, select the metric name and choose **What is this?**\.  
![\[View the resulting metrics for a search term\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metrics_search_results.png)