# View experiment results in the dashboard<a name="CloudWatch-Evidently-experiment-dashboard"></a>

You can see the statistical results of an experiment while it is ongoing and after it is completed\. Experiment results are available up to 63 days after the start of the experiment\. They are not available after that because of CloudWatch data retention policies\.

No statistical results are displayed until each variation has at least 100 events\.

Evidently performs an additional offline p\-value analysis at the end of the experiment\. Offline p\-value analysis can detect statistical significance in some cases where the anytime p\-values used during the experiment do not find statistical significance\.

For more information about how CloudWatch Evidently calculates experiment results, see [How Evidently calculates results](CloudWatch-Evidently-calculate-results.md)\. 

**To see the results of an experiment**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose the name of the project that contains the experiment\.

1. Choose the **Experiments** tab\.

1. Choose the name of the experiment, and then choose the **Results** tab\.

1. By **Variation performance**, there is a control where you can select which experiment statistics to display\. If you select more than one statistic, Evidently displays a graph and table for each statistic\.

   Each graph and table displays the results of the experiment so far\. 

   Each graph can display the following results\. You can use the control at the right of the graph to determine which of the following items is displayed:
   + The number of user session events recorded for each variation\.
   + The average value of the metric that is selected at the top of the graph, for each variation\.
   + The statistical significance of the experiments\. This compares the difference for the metric selected at the top of the graph with the default variation and each of the other variations\.
   + The 95% upper and lower confidence bounds on the difference of the selected metric, between each of the variations and the default variation\.

   The table displays a row for each variation\. For each variation that is not the default, Evidently displays whether it has received enough data to declare the results statistically significant\. It also shows whether the variation's improvement in the statistical value has reached a 95% confidence level\. 

   Finally, in the **Result** column, Evidently provides a recommendation about which variation performs best based on this statistic, or whether the results are inconclusive\.