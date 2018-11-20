# Create a CloudWatch Alarm Based on a Metric Math Expression<a name="Create-alarm-on-metric-math-expression"></a>

To create an alarm based on a metric math expression, choose one or more CloudWatch metrics to use in the expression\. Then, specify the expression, threshold, and evaluation periods\.

**To create an alarm based on a math expression**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select Metric** and do one of the following:

   1. Choose the service namespace that contains a specific metric\. Continue choosing options as they appear to narrow the choices\. When a list of metrics is displayed, select the check box next to the right metric\.

      or

   1. In the search box, type the name of a metric, dimension, or resource ID and press Enter\. Choose one of the results and continue until a list of metrics is displayed\. Select the check box next to the right metric\. 

   \(Optional\) To add another metric to use in the math expression, under **All metrics**, choose **All**, find the specific metric, and then select the check box next to it\. You can add up to 10 metrics\.

1. Choose **Graphed metrics**\. For each metric added, do the following:

   1. Under **Statistic**, choose one of the statistics or predefined percentiles, or specify a custom percentile \(for example, p95\.45\)\.

   1. Under **Period**, choose the evaluation period for the alarm\. When evaluating the alarm, each period is aggregated into one data point\.

      You can also choose whether the Y axis legend appears on the left or right while you are creating the alarm\. This preference is used only while you are creating the alarm\.

1. Choose **Add a math expression**\. A new row appears for the expression\.

1. On the new row, under the **Details** column, type the math expression and press Enter\. For information about the functions and syntax that you can use, see [Metric Math Syntax and Functions](using-metric-math.md#metric-math-syntax)\.

   To use a metric or the result of another expression as part of the formula for this expression, use the value shown in the **Id** column\. For example, **m1\+m2** or **e1\-MIN\(e1\)**\.

   You can change the value of **Id**\. It can include numbers, letters, and underscores, and must start with a lowercase letter\. Changing the value of **Id** to a more meaningful name can also make the alarm graph easier to understand\. 

1. \(Optional\), add more math expressions, using both metrics and the results of other math expressions in the formulas of the new math expressions\.

1. When you have the expression to use for the alarm, clear the check boxes to the left of every other expression and every metric on the page\. Only the check box next to the expression to use for the alarm should be selected\. The expression you choose for the alarm must produce a single time series, and show only one line on the graph\. Then choose **Select metric**\.

1. Type a name and description for the alarm\. The name must contain only ASCII characters\. 

1. For **Whenever**, specify the alarm condition\.

   1. For **is:**, specify whether the expression result must be greater than, less than, or equal to the threshold, and specify the threshold value\.

   1. For **for:**, specify how many evaluation periods \(data points\) must be in the ALARM state to trigger the alarm\. Initially, you can change only the second value, and the first value changes to match your entry\. This creates an alarm that goes to the ALARM state if that many consecutive periods are breaching\.

      To create an M out of N alarm, choose the pencil icon\. You can then change the M number to be different than the N number\. For more information, see [Evaluating an Alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.

1. Under **Additional settings**, for **Treat missing data as**, choose how to have the alarm behave when some data points are missing\. For more information, see [Configuring How CloudWatch Alarms Treat Missing Data](AlarmThatSendsEmail.md#alarms-and-missing-data)\.

1. Under **Actions**, select the type of action to have the alarm to perform when the alarm is triggered\. Choose **\+Notification** or **\+AutoScaling Action** to have the alarm perform multiple actions\. Specify at least one action\.

1. Choose **Create Alarm**\.

You can also add alarms to a dashboard\. For more information, see [Add or Remove an Alarm from a CloudWatch Dashboard](add_remove_alarm_dashboard.md)\. 