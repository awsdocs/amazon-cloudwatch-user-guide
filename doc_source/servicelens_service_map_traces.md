# Using the traces view in ServiceLens<a name="servicelens_service_map_traces"></a>

The traces view enables you to view recent X\-Ray traces in your application\.

**To view traces and dive down for more information**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **ServiceLens**, **Traces**\.

1. Under **Filter type**, you can choose different criteria to sort the traces by\. As you do, CloudWatch displays the success rate and response time of the traces according to your choice\. The list of traces displayed at the bottom of the page is narrowed to traces that match your filter\. The first 1000 traces that match your filter are retrieved\.

   The values in the filter table are populated by the traces that the filter returns, and update as you refine your choices in the filter\.

   Use the **Custom query** option under **Filter type** to add a custom expression as a filter\. Custom expressions can contain keywords, unary or binary operators, and comparison values for the keywords\. Keywords can correspond to parts of a trace, such as `responsetime`, `duration`, and status codes\. For more information, see [Filter Expression Syntax](https://docs.aws.amazon.com/xray/latest/devguide/xray-console-filters.html#console-filters-syntax)\.

   You can focus your filter further by selecting the check box next to a row under **Traces by** and choosing **Add to filter**\. The filter value from the selected row is added to the filter\. 

1. To see a histogram representing the response time distribution of the traces returned by the current filter, and a table displaying each individual trace, scroll to the bottom of the screen\.

   Choose one of the traces in the table to view detailed information about the trace\.

   The trace details page displays a timeline that includes each of the segments that comprise the trace\. Choose a segment to view additional associated metadata, including errors where applicable\. For traces where the logs can be associated, the log lines associated with the trace are displayed under the timeline\.

   You can then optionally view those log entries in CloudWatch Logs Insights\. To do that, choose **View in CloudWatch Logs Insights** and then choose **Run query**\.

   Some traces do not have associated log entries\.
   + If you don't see log entries for Amazon EC2 or Amazon EKS, you need to update to the latest version of the X\-Ray SDK\. For more information, see [Deploying AWS X\-Ray](deploy_servicelens_xray.md)\.
   + ServiceLens shows log entries for Amazon ECS only if there are 20 or fewer log groups with names that start with `/ecs/`\.
   + Currently, log entries for Fargate traces are not available\.