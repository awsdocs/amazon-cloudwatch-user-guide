# Viewing Contributor Insights reports<a name="ContributorInsights-ViewReports"></a>

To view graphs of report data and a ranked list of contributors found by your rules, follow these steps\.

**To view your rule reports**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Contributor Insights**\.

   

1. In the list of rules, choose the name of a rule\.

   The graph displays the results of the rule over the last three hours\. The table under the graph shows the top 10 contributors\.

1. To change the number of contributors shown in the table, choose **Top 10 contributors** at the top of the graph\.

1. To filter the graph to show only the results from a single contributor, choose that contributor in the table legend\. To again show all contributors, choose that same contributor again in the legend\.

1. To change the time range shown in the report, choose **15m**, **30m**, **1h**, **2h**, **3h**, or **custom** at the top of the graph\.

   The maximum time range for the report is 24 hours, but you can choose a 24\-hour window that occurred up to 15 days ago\. To choose a time window in the past, choose **custom**, **absolute**, and then specify your time window\.

1. To change the length of the time period used for the aggregation and ranking of contributors, choose **period** at the top of the graph\. Viewing a longer time period generally shows a smoother report with few spikes\. Choosing a shorter time period is more likely to display spikes\.

1. To add this graph to a CloudWatch dashboard, choose **Add to dashboard**\.

1. To open the CloudWatch Logs Insights query window, with the log groups in this report already loaded in the query box, choose **View logs**\.

1. To export the report data to your clipboard or a CSV file, choose **Export**\.