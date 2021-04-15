# Viewing your Lambda Insights metrics<a name="Lambda-Insights-view-metrics"></a>

After you have installed the Lambda Insights extension on a Lambda function that has been invoked, you can use the CloudWatch console to see your metrics\. You can see a multi\-function overview, or focus on a single function\.

For a list of Lambda Insights metrics, see [Metrics collected by Lambda Insights](Lambda-Insights-metrics.md)\.

**To view the multi\-function overview for your Lambda Insights metrics**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, under **Lambda Insights**, choose **Multi\-function**\.

   The top part of the page displays graphs with aggregated metrics of all your Lambda functions in the Region that have Lambda Insights enabled\. Lower on the page is a table that lists the functions\.

1. To filter by function name to reduce the number of functions displayed, type part of the function name in the box near the top of the page\.

1. To add this view to a dashboard as a widget, choose **Add to dashboard**\.

**To view metrics for a single function**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, under **Lambda Insights**, choose **Single\-function**\.

   The top part of the page displays graphs with metrics for the selected function\.

1. If you have X\-Ray enabled, you can choose a single trace ID\. This opens CloudWatch ServiceLens for that invocation, and from there you can zoom out to see the distributed trace and the other services involved in handling that specific transaction\. For more information about ServiceLens, see [Using ServiceLens to Monitor the Health of Your Applications](ServiceLens.md)\.

1. To open CloudWatch Logs Insights and zoom in on a specific error, choose **View logs** by the table at the bottom of the page\.

1. To add this view to a dashboard as a widget, choose **Add to dashboard**\.