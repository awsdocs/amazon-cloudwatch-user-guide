# Build your queries<a name="cloudwatch-metrics-insights-buildquery"></a>

****  
CloudWatch Metrics Insights is in open preview\. The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\.

You can run a CloudWatch Metrics Insights query using the CloudWatch console, the AWS CLI, or the AWS SDKs\. Queries run in the console are free of charge\. For more information about CloudWatch pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

For more information about using the AWS SDKs to perform a Metrics Insights query, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)\.

To run a query using the CloudWatch console, follow these steps:

**To query your metrics using Metrics Insights**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**, **All metrics**\.

1. Choose the **Query** tab\.

1. \(Optional\) To run a pre\-built sample query, choose **Add query** and select the query to run\. If you are satisfied with this query, you can skip the rest of this procedure\. Or, you can choose **Editor** to edit the sample query and then choose **Run** to run the modified query\. 

1. To create your own query, you can use the **Builder** view, the **Editor** view, and also use a combination of both\. You can switch between the two views anytime and see your work in progress in both views\. 

   In the **Builder** view, you can browse and select the metric namespace, metric name, filter, group, and order options\. For each of these options, the query builder offers you a list of possible selections from your environment to choose from\.

   In the **Editor** view, you can start writing your query\. As you type, the editor offers suggestions based on the characters that you have typed so far\.

1. When you are satisfied with your query, choose **Run**\.

1. \(Optional\) Another way to edit a query that you have graphed is to choose the **Graphed metrics** tab and choose the edit icon next to the query formula in the **Details** column\.

1. \(Optional\) To remove a query from the graph, choose **Graphed metrics** and choose the **X** icon at the right side of the row that displays your query\.