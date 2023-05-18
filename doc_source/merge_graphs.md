# Merge two graphs into one<a name="merge_graphs"></a>

You can merge two different graphs into one, and then the resulting graph shows both metrics\. This can be useful if you already have different metrics displayed in different graphs and want to combine them, or you want to easily create a single graph with metrics from different Regions\.

To merge a graph into another one, you use either the URL or JSON source of the graph that you want to merge in\.

**To merge two graphs into one**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Open the graph that you want to merge into another graph\. To do so, you can choose **Metrics**, **All metrics**, and then choose a metric to graph\. Or you can open a dashboard and then open one of the graphs on the dashboard by selecting the graph and choosing **Open in metrics** from the menu at the upper right of the graph\. 

1. After you have a graph open, do one of the following:\.
   + Copy the URL from the browser bar\.
   + Choose the **Source** tab and then choose **Copy**\.

1. Open the graph that you want to merge the previous graph into\. 

1. When you have the second graph open in the **Metrics** view, choose **Actions**, **Merge graph**\.

1. Enter the URL or JSON that you previously copied, and choose **Merge**\.

1. The merged graphs appear\. The y\-axis on the left is for the original graph, and the y\-axis on the right is for the graph that you merged into it\.
**Note**  
If the graph that you merged into uses the **METRICS\(\)** function, the metrics in the graph that was merged in are not included in the **METRICS\(\)** calculation in the merged graph\.

1. To save the merged graph to a dashboard, chooose **Actions**, **Add to dashboard**\.