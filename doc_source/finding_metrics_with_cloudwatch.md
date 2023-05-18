# Search for available metrics<a name="finding_metrics_with_cloudwatch"></a>

You can search within all of the metrics in your account using targeted search terms\. Metrics are returned that have matching results within their namespace, metric name, or dimensions\.

If this is a monitoring account in CloudWatch cross\-account observability, you also search for metrics from the source accounts linked to this monitoring account\. 

**Note**  
Metrics that have not had any new data points in the past two weeks do not appear in the console\. They also do not appear when you type their metric name or dimension names in the search box in the **All metrics** tab in the console, and they are not returned in the results of a [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html) command\. The best way to retrieve these metrics is with the [get\-metric\-data](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-data.html) or [get\-metric\-statistics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/get-metric-statistics.html) commands in the AWS CLI\.

**To search for available metrics in CloudWatch**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. In the search field on the **All metrics** tab, enter a search term, such as a metric name, namespace, account ID, account label, dimension name or value, or resource name\. This shows you all of the namespaces with metrics with this search term\.

   For example, if you search for **volume**, this shows the namespaces that contain metrics with this term in their name\.

   For more information on search, see [Use search expressions in graphs](using-search-expressions.md)

1. To graph all the search results, choose **Graph search**

   or

   Select a namespace to view the metrics from that namespace\. You can then do the following:

   1. To graph one or more metrics, select the check box next to each metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To refine your search, hover over a metric name and choose **Add to search** or **Search for this only**\.

   1. To view one of the resources on its console, choose the resource ID and then choose **Jump to resource**\.

   1. To view help for a metric, select the metric name and choose **What is this?**\.

   The selected metrics appear on the graph\.  
![\[View the resulting metrics for a search term\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/metrics_search_results.png)

1. \(Optional\) Select one of the buttons in the search bar to edit that part of the search term\.