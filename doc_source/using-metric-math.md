# Use metric math<a name="using-metric-math"></a>

Metric math enables you to query multiple CloudWatch metrics and use math expressions to create new time series based on these metrics\. You can visualize the resulting time series on the CloudWatch console and add them to dashboards\. Using AWS Lambda metrics as an example, you could divide the `Errors` metric by the `Invocations` metric to get an error rate\. Then add the resulting time series to a graph on your CloudWatch dashboard\.

You can also perform metric math programmatically, using the `GetMetricData` API operation\. For more information, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)\.

## Add a math expression to a CloudWatch graph<a name="adding-metrics-expression-console"></a>

You can add a math expression to a graph on your CloudWatch dashboard\. Each graph is limited to using a maximum of 500 metrics and expressions, so you can add a math expression only if the graph has 499 or fewer metrics\. This applies even if not all the metrics are displayed on the graph\.

**To add a math expression to a graph**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Create or edit a graph\. There needs to be at least one metric in the graph\.

1. Choose **Graphed metrics**\.

1. Choose **Math expression**, **Start with empty expression**\. A new line appears for the expression\.

1. In the new line, under the **Details** column, enter the math expression\. The tables in the Metric Math Syntax and Functions section list the functions that you can use in the expression\. 

   To use a metric or the result of another expression as part of the formula for this expression, use the value shown in the **Id** column: for example, **m1\+m2** or **e1\-MIN\(e1\)**\.

   You can change the value of **Id**\. It can include numbers, letters, an underscore, and must start with a lowercase letter\. Changing the value of **Id** to a more meaningful name can also make a graph easier to understand;for example, changing **m1** and **m2** to **errors** and **requests**\.
**Tip**  
Choose the down arrow next to **Math Expression** to see a list of supported functions, which you can use when creating your expression\.

1. For the **Label** column of the expression, enter a name that describes what the expression is calculating\.

   If the result of an expression is an array of time series, each of those time series is displayed on the graph with a separate line, with different colors\. Immediately under the graph is a legend for each line in the graph\. For a single expression that produces multiple time series, the legend captions for those time series are in the format ***Expression\-Label Metric\-Label***\. For example, if the graph includes a metric with a label of **Errors** and an expression **FILL\(METRICS\(\), 0\)** that has a label of **Filled With 0:**, one line in the legend would be **Filled With 0: Errors**\. To have the legend show only the original metric labels, set *Expression\-Label* to be empty\.

   When one expression produces an array of time series on the graph, you can't change the colors used for each of those time series\.

1. After you have added the desired expressions, you can simplify the graph by hiding some of the original metrics\. To hide a metric or expression, clear the check box to the left of the **Id** field\.

## Metric math syntax and functions<a name="metric-math-syntax"></a>

The following sections explain the functions available for metric math\. All functions must be written in uppercase letters \(such as **AVG**\), and the **Id** field for all metrics and math expressions must start with a lowercase letter\. 

The final result of any math expression must be a single time series or an array of time series\. Some functions produce a scalar number\. You can use these functions within a larger function that ultimately produces a time series\. For example, taking the **AVG** of a single time series produces a scalar number, so it can't be the final expression result\. But you could use it in the function **m1\-AVG\(m1\)** to display a time series of the difference between each individual data point and the average value in the time series\.

### Data type abbreviations<a name="metric-math-syntax-datatypes"></a>

Some functions are valid for only certain types of data\. The abbreviations in the following list are used in the tables of functions to represent the types of data supported for each function:
+ **S** represents a scalar number, such as 2, \-5, or 50\.25
+ **TS** is a time series \(a series of values for a single CloudWatch metric over time\): for example, the `CPUUtilization` metric for instance `i-1234567890abcdef0` over the last 3 days
+ **TS\[\]** is an array of time series, such as the time series for multiple metrics

### The METRICS\(\) function<a name="metric-math-syntax-metrics-function"></a>

The **METRICS\(\)** function returns all the metrics in the request\. Math expressions aren't included\.

You can use **METRICS\(\)** within a larger expression that produces a single time series or an array of time series\. For example, the expression **SUM\(METRICS\(\)\)** returns a time series \(TS\) that is the sum of the values of all the graphed metrics\. **METRICS\(\)/100** returns an array of time series, each of which is a time series showing each data point of one of the metrics divided by 100\.

You can use the **METRICS\(\)** function with a string to return only the graphed metrics that contain that string in their **Id** field\. For example, the expression **SUM\(METRICS\("errors"\)\)** returns a time series that is the sum of the values of all the graphed metrics that have ‘errors’ in their **Id** field\. You can also use **SUM\(\[METRICS\(“4xx”\), METRICS\(“5xx”\)\]\)** to match multiple strings\.

### Basic arithmetic functions<a name="metric-math-syntax-arithmetic"></a>

The following table lists the basic arithmetic functions that are supported\. Missing values in a time series are treated as 0\. If the value of a data point causes a function to attempt to divide by zero, the data point is dropped\.


| Operation | Arguments | Examples | 
| --- | --- | --- | 
|  Arithmetic operators: \+ \- \* / ^ |  S, S S, TS TS, TS S, TS\[\] TS, TS\[\]  |  PERIOD\(m1\)/60 **5 \* m1** **m1 \- m2** **SUM\(100/\[m1, m2\]\)** **AVG\(METRICS\(\)\)** **METRICS\(\)\*100**  | 
|  Unary subtraction \-  |  S TS TS\[\]  |  **\-5\*m1** **\-m1** **SUM\(\-\[m1, m2\]\)**  | 

### Comparison and logical operators<a name="metric-math-syntax-operators"></a>

You can use comparison and logical operators with either a pair of time series or a pair of single scalar values\. When you use a comparison operator with a pair of time series, the operators return a time series where each data point is either 0 \(false\) or 1 \(true\)\. If you use a comparison operator on a pair of scalar values, a single scalar value is returned, either 0 or 1\.

When comparison operators are used between two time series, and only one of the time series has a value for a particular time stamp, the function treats the missing value in the other time series as **0**\.

You can use logical operators in conjunction with comparison operators, to create more complex functions\.

The following table lists the operators that are supported\.


| Type of operator | Supported operators | 
| --- | --- | 
|  Comparison operators |  == \!= <= >= < >  | 
|  Logical operators |  AND or && OR or \|\|  | 

To see how these operators are used, suppose we have two time series: **metric1** has values of `[30, 20, 0, 0]` and **metric2** has values of `[20, -, 20, -]` where `-` indicates that there is no value for that timestamp\.


| Expression | Output | 
| --- | --- | 
|  **\(metric1 < metric2\)** |  **0, 0, 1, 0**  | 
|  **\(metric1 >= 30\)** |  **1, 0, 0, 0**  | 
|  **\(metric1 > 15 AND metric2 > 15\)** |  **1, 0, 0, 0**  | 

### Functions supported for metric math<a name="metric-math-syntax-functions-list"></a>

The following table describes the functions that you can use in math expressions\. Enter all functions in uppercase letters\.

The final result of any math expression must be a single time series or an array of time series\. Some functions in tables in the following sections produce a scalar number\. You can use these functions within a larger function that ultimately produces a time series\. For example, taking the **AVG** of a single time series produces a scalar number, so it can't be the final expression result\. But you could use it in the function **m1\-AVG\(m1\)** to display a time series of the difference between each individual data point and the average value of that data point\.

In the following table, every example in the **Examples** column is an expression that results in a single time series or an array of time series\. These examples show how functions that return scalar numbers can be used as part of a valid expression that produces a single time series\.


| Function | Arguments | Return type**\*** | Description | Examples | Supported for cross\-account? | 
| --- | --- | --- | --- | --- | --- | 
|  **ABS** |  TS TS\[\]  |  TS TS\[\]  | Returns the absolute value of each data point\. |  **ABS\(m1\-m2\)** **MIN\(ABS\(\[m1, m2\]\)\)** **ABS\(METRICS\(\)\)**  | ✓ | 
|  **ANOMALY\_DETECTION\_BAND** |  TS TS, S  |  TS\[\]  | Returns an anomaly detection band for the specified metric\. The band consists of two time series, one representing the upper limit of the "normal" expected value of the metric, and the other representing the lower limit\. The function can take two arguments\. The first is the ID of the metric to create the band for\. The second argument is the number of standard deviations to use for the band\. If you don't specify this argument, the default of 2 is used\. For more information, see [Using CloudWatch anomaly detection](CloudWatch_Anomaly_Detection.md)\. |  **ANOMALY\_DETECTION\_BAND\(m1\)** **ANOMALY\_DETECTION\_BAND\(m1,4\)**  |  | 
|  **AVG** |  TS TS\[\]  |  S TS  | The **AVG** of a single time series returns a scalar representing the average of all the data points in the metric\. The **AVG** of an array of time series returns a single time series\. Missing values are treated as 0\.  We recommend that you do not use this function in CloudWatch alarms if you want the function to return a scalar\. For example, `AVG(m2)`\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\.  |  **SUM\(\[m1,m2\]\)/AVG\(m2\)** **AVG\(METRICS\(\)\)**  | ✓ | 
|  **CEIL** |  TSTS\[\]  |  TS TS\[\]  | Returns the ceiling of each metric\. The ceiling is the smallest integer greater than or equal to each value\. |  **CEIL\(m1\)** **CEIL\(METRICS\(\)\)** **SUM\(CEIL\(METRICS\(\)\)\)**  | ✓ | 
|  **DATAPOINT\_COUNT** |  TS TS\[\]  |  S TS  | Returns a count of the data points that reported values\. This is useful for calculating averages of sparse metrics\. We recommend that you do not use this function in CloudWatch alarms\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **SUM\(m1\) / DATAPOINT\_COUNT\(m1\)** **DATAPOINT\_COUNT\(METRICS\(\)\)**  | ✓ | 
|  **DIFF** |  TSTS\[\]  |  TS TS\[\]  | Returns the difference between each value in the time series and the preceding value from that time series\. |  **DIFF\(m1\)**  | ✓ | 
|  **DIFF\_TIME** |  TSTS\[\]  |  TS TS\[\]  | Returns the difference in seconds between the timestamp of each value in the time series and the timestamp of the preceding value from that time series\. |  **DIFF\_TIME\(METRICS\(\)\)**  | ✓ | 
|  **FILL** |  TS, \[S \| REPEAT \| LINEAR\] TS\[\], \[TS \| S \| REPEAT \| LINEAR\]  |  TS TS\[\]  | Fills the missing values of a time series\. There are several options for the values to use as the filler for missing values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html) When you use this function in an alarm, you can encounter an issue if your metrics are being published with a slight delay, and the most recent minute never has data\. In this case, **FILL** replaces that missing data point with the requested value\. That causes the latest data point for the metric to always be the FILL value, which can result in the alarm being stuck in either OK state or ALARM state\. You can work around this by using a M out of N alarm\. For more information, see [Evaluating an alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.  |  **FILL\(m1,10\)** **FILL\(METRICS\(\), 0\)** **FILL\(METRICS\(\), m1\)** **FILL\(m1, MIN\(m1\)\)** **FILL\(m1, REPEAT\)** **FILL\(METRICS\(\), LINEAR\)**  | ✓ | 
|  **FIRST** **LAST** |  TS\[\]  |  TS  | Returns the first or last time series from an array of time series\. This is useful when used with the **SORT** function\. It can also be used to get the high and low thresholds from the **ANOMALY\_DETECTION\_BAND** function\.  |  **IF\(FIRST\(SORT\(METRICS\(\), AVG, DESC\)\)>100, 1, 0\)** Looks at the top metric from an array, which is sorted by AVG\. It then returns a 1 or 0 for each data point, depending on whether that data point value is more than 100\. **LAST\(ANOMALY\_DETECTION\_BAND\(m1\)\) returns the lower bound of the anomaly prediction band\.**  | ✓ | 
|  **FLOOR** |  TSTS\[\]  |  TS TS\[\]  | Returns the floor of each metric\. The floor is the largest integer less than or equal to each value\. |  **FLOOR\(m1\)** **FLOOR\(METRICS\(\)\)**  | ✓ | 
|  **IF** |  **IF** expression  |  TS  | Use **IF** along with a comparison operator to filter out data points from a time series, or create a mixed time\-series composed of multiple collated time series\. For more information, see [Using IF expressions](#using-IF-expressions)\. | For examples, see [Using IF expressions](#using-IF-expressions)\.  | ✓ | 
|  **INSIGHT\_RULE\_METRIC** |  **INSIGHT\_RULE\_METRIC\(ruleName, metricName\)**  |  TS  | Use **INSIGHT\_RULE\_METRIC** to extract statistics from a rule in Contributor Insights\. For more information, see [Graphing metrics generated by rulesSetting an alarm on Contributor Insights metric data](ContributorInsights-GraphReportData.md)\. |   |  | 
|  **LOG** |  TS TS\[\]  |  TS TS\[\]  | The **LOG** of a time series returns the natural logarithm value of each value in the time series\. |  **LOG\(METRICS\(\)\)**  | ✓ | 
|  **LOG10** |  TS TS\[\]  |  TS TS\[\]  | The **LOG10** of a time series returns the base\-10 logarithm value of each value in the time series\. |  **LOG10\(m1\)**  | ✓ | 
|  **MAX** |  TS TS\[\]  |  S TS  | The **MAX** of a single time series returns a scalar representing the maximum value of all data points in the metric\. The **MAX** value of an array of time series returns a single time series\.  We recommend that you do not use this function in CloudWatch alarms\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **MAX\(m1\)/m1** **MAX\(METRICS\(\)\)**  | ✓ | 
|  **METRIC\_COUNT** |  TS\[\]  |  S  | Returns the number of metrics in the time series array\.  |  **m1/METRIC\_COUNT\(METRICS\(\)\)**  | ✓ | 
|  **METRICS** |  null string  |  TS\[\]  | The **METRICS\(\)** function returns all CloudWatch metrics in the request\. Math expressions aren't included\. You can use **METRICS\(\)** within a larger expression that produces a single time series or an array of time series\. You can use the **METRICS\(\)** function with a string to return only the graphed metrics that contain that string in their **Id** field\. For example, the expression **SUM\(METRICS\("errors"\)\)** returns a time series that is the sum of the values of all the graphed metrics that have ‘errors’ in their **Id** field\. You can also use **SUM\(\[METRICS\(“4xx”\), METRICS\(“5xx”\)\]\)** to match multiple strings\.  |  **AVG\(METRICS\(\)\)** **SUM\(METRICS\("errors"\)\)**  | ✓ | 
|  **MIN** |  TS TS\[\]  |  S TS  | The **MIN** of a single time series returns a scalar representing the minimum value of all data points in the metric\. The **MIN** of an array of time series returns a single time series\.  We recommend that you do not use this function in CloudWatch alarms\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **m1\-MIN\(m1\)** **MIN\(METRICS\(\)\)**  | ✓ | 
|  **MINUTE** **HOUR** **DAY** **DATE** **MONTH** **YEAR** **EPOCH** |  TS  |  TS  | These functions take the period and range of the time series and return a new non\-sparse time series where each value is based on its timestamp\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html) |  **MINUTE\(m1\)** **IF\(DAY\(m1\)<6,m1\)** returns metrics only from weekdays, Monday to Friday\. **IF\(MONTH\(m1\) == 4,m1\)** returns only metrics published in April\.  | ✓ | 
|  **PERIOD** |  TS  |  S  | Returns the period of the metric in seconds\. Valid input is metrics, not the results of other expressions\.  |  **m1/PERIOD\(m1\)**  | ✓ | 
|  **RATE** |  TS TS\[\]  |  TS TS\[\] | Returns the rate of change of the metric per second\. This is calculated as the difference between the latest data point value and the previous data point value, divided by the time difference in seconds between the two values\. |  **RATE\(m1\)** **RATE\(METRICS\(\)\)**  | ✓ | 
|  **REMOVE\_EMPTY** |  TS\[\]  |  TS\[\] | Removes any time series that have no data points from an array of time series\. The result is an array of time series where each time series contains at least one data point\. We recommend that you do not use this function in CloudWatch alarms\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **REMOVE\_EMPTY\(METRICS\(\)\)**   | ✓ | 
|  **RUNNING\_SUM** |  TS TS\[\]  |  TS TS\[\]  | Returns a time series with the running sum of the values in the original time series\. We recommend that you do not use this function in CloudWatch alarms\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **RUNNING\_SUM\(\[m1,m2\]\)**  | ✓ | 
|  **SEARCH** |  Search expression  |  One or more TS  | Returns one or more time series that match a search criteria that you specify\. The **SEARCH** function enables you to add multiple related time series to a graph with one expression\. The graph is dynamically updated to include new metrics that are added later and match the search criteria\. For more information, see [Use search expressions in graphs](using-search-expressions.md)\. You can't create an alarm based on a **SEARCH** expression\. This is because search expressions return multiple time series, and an alarm based on a math expression can watch only one time series\. If you are signed in to a monitoring account in CloudWatch cross\-account observability, the **SEARCH** function finds metrics in the source accounts and the monitoring accout\. |   | ✓ | 
|  **SERVICE\_QUOTA** |  TS that is a usage metric  |  TS  | Returns the service quota for the given usage metric\. You can use this to visualize how your current usage compares to the quota, and to set alarms that warn you when you approach the quota\. For more information, see [AWS usage metrics](CloudWatch-Service-Quota-Integration.md)\. |   |  | 
|  **SLICE** |  \(TS\[\], S, S\) or \(TS\[\], S\)  |  TS\[\] TS  | Retrieves part of an array of time series\. This is especially useful when combined with **SORT**\. For example, you can exclude the top result from an array of time series\. You can use two scalar arguments to define the set of time series that you want returned\. The two scalars define the start \(inclusive\) and end \(exclusive\) of the array to return\. The array is zero\-indexed, so the first time series in the array is time series 0\. Alternatively, you can specify just one value, and CloudWatch returns all time series starting with that value\.  | **SLICE\(SORT\(METRICS\(\), SUM, DESC\), 0, 10\)** returns the 10 metrics from the array of metrics in the request that have the highest SUM value\. **SLICE\(SORT\(METRICS\(\), AVG, ASC\), 5\)** sorts the array of metrics by the AVG statistic, then returns all the time series except for the 5 with the lowest AVG\.  | ✓ | 
|  **SORT** |  \(TS\[\], FUNCTION, SORT\_ORDER\) \(TS\[\], FUNCTION, SORT\_ORDER, S\)  |  TS\[\]  | Sorts an array of time series according to the function you specify\. The function you use can be **AVG**, **MIN**, **MAX**, or **SUM**\. The sort order can be either **ASC** for ascending \(lowest values first\) or **DESC** to sort the higher values first\. You can optionally specify a number after the sort order which acts as a limit\. For example, specifying a limit of 5 returns only the top 5 time series from the sort\. When this math function is displayed on a graph, the labels for each metric in the graph are also sorted and numbered\.  | **SORT\(METRICS\(\), AVG, DESC, 10\)** calculates the average value of each time series, sorts the time series with the highest values at the beginning of the sort, and returns only the 10 time series with the highest averages\. **SORT\(METRICS\(\), MAX, ASC\)** sorts the array of metrics by the MAX statistic, then returns all of them in ascending order\.  | ✓ | 
|  **STDDEV** |  TS TS\[\]  |  S TS  | The **STDDEV** of a single time series returns a scalar representing the standard deviation of all data points in the metric\. The **STDDEV** of an array of time series returns a single time series\.  We recommend that you do not use this function in CloudWatch alarms\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **m1/STDDEV\(m1\)** **STDDEV\(METRICS\(\)\)**  | ✓ | 
|  **SUM** |  TS TS\[\]  |  S TS  | The **SUM** of a single time series returns a scalar representing the sum of the values of all data points in the metric\. The **SUM** of an array of time series returns a single time series\.  We recommend that you do not use this function in CloudWatch alarms if you want the function to return a scalar\. For example, `SUM(m1)`\. Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as Evaluation Periods\. This function acts differently when extra data is requested\. |  **SUM\(METRICS\(\)\)/SUM\(m1\)** **SUM\(\[m1,m2\]\)** **SUM\(METRICS\("errors"\)\)/SUM\(METRICS\("requests"\)\)\*100**  | ✓ | 
|  **TIME\_SERIES** |  S  |  TS  | Returns a non\-sparse time series where every value is set to a scalar argument\. |  **TIME\_SERIES\(MAX\(m1\)\)** **TIME\_SERIES\(5\*AVG\(m1\)\)** **TIME\_SERIES\(10\)**  | ✓ | 

**\***Using a function that returns only a scalar number is not valid, as all final results of expressions must be a single time series or an array of time series\. Instead, use these functions as part of a larger expression that returns a time series\.

## Using IF expressions<a name="using-IF-expressions"></a>

Use **IF** along with a comparison operator to filter out data points from a time series, or create a mixed time\-series composed of multiple collated time series\.

**IF** uses the following arguments:

```
IF(condition, trueValue, falseValue)
```

The condition evaluates to FALSE if the value of the condition data point is 0, and to TRUE if the value of the condition is any other value, whether that value is positive or negative\. If the condition is a time series, it is evaluated separately for every timestamp\.

The following lists the valid syntaxes\. For each of these syntaxes, the output is a single time series\.
+ **IF\(TS *Comparison Operator* S, S \| TS, *S \| TS*\)**
+ **IF\(TS, TS, *TS*\)**
+ **IF\(TS, S, *TS*\)**
+ **IF\(TS, TS, S\)**
+ **IF\(TS, S, S\)**
+ **IF\(S, TS, *TS*\)**

The following sections provide more details and examples for these syntaxes\.

**IF\(TS *Comparison Operator* S, scalar2 \| metric2, scalar3 \| metric3\)**

The corresponding output time series value:
+ has the value of **scalar2** or **metric2**, if **TS *Comparison Operator* S** is TRUE
+ has the value of **scalar3** or **metric3**, if **TS *Comparison Operator* S** is FALSE
+ is an empty time series, if the corresponding data point of does not exist in **metric3**, or if **scalar3**/**metric3** is omitted from the expression

**IF\(metric1, metric2, *metric3*\)**

For each data point of **metric1**, the corresponding output time series value:
+ has the value of **metric2**, if the corresponding data point of **metric1** is TRUE\.
+ has the value of **metric3**, if the corresponding data point of **metric1** is FALSE\.
+ has the value of **0**, if the corresponding data point of **metric1** is TRUE and the corresponding data point does not exist in **metric2**\.
+ is dropped, if the corresponding data point of **metric1** is FALSE and the corresponding data point does not exist in **metric3** or if **metric3** is omitted from the expression\.

The following table shows an example for this syntax\.


| Metric or function | Values | 
| --- | --- | 
|  **\(metric1\)** |  **\[1, 1, 0, 0, \-\]**  | 
|  **\(metric2\)** |  **\[30, \-, 0, 0, 30\]**  | 
|  **\(metric3\)** |  **\[0, 0, 20, \-, 20\]**  | 
|  **IF\(metric1, metric2, metric3\)** |  **\[30, 0, 20, \-, \-\]**  | 

**IF\(metric1, scalar2, metric3\)**

For each data point of **metric1**, the corresponding output time series value:
+ has the value of **scalar2**, if the corresponding data point of **metric1** is TRUE\.
+ has the value of **metric3**, if the corresponding data point of **metric1** is FALSE\.
+ is dropped, if the corresponding data point of **metric1** is FALSE and the corresponding data point does not exist on **metric3**, or if **metric3** is omitted from the expression\.


| Metric or function | Values | 
| --- | --- | 
|  **\(metric1\)** |  **\[1, 1, 0, 0, \-\]**  | 
|  **scalar2** |  **5**  | 
|  **\(metric3\)** |  **\[0, 0, 20, \-, 20\]**  | 
|  **IF\(metric1, scalar2, metric3\)** |  **\[5, 5, 20, \-, \-\]**  | 

**IF\(metric1, metric2, scalar3\)**

For each data point of **metric1**, the corresponding output time series value:
+ has the value of **metric2**, if the corresponding data point of **metric1** is TRUE\.
+ has the value of **scalar3**, if the corresponding data point of **metric1** is FALSE\.
+ has the value of **0**, if the corresponding data point of **metric1** is TRUE and the corresponding data point does not exist in **metric2**\. 
+ is dropped if the corresponding data point in **metric1** does not exist\. 


| Metric or function | Values | 
| --- | --- | 
|  **\(metric1\)** |  **\[1, 1, 0, 0, \-\]**  | 
|  **\(metric2\)** |  **\[30, \-, 0, 0, 30\]**  | 
|  **scalar3** |  **5**  | 
|  **IF\(metric1, metric2, scalar3\)** |  **\[30, 0, 5, 5, \-\]**  | 

**IF\(scalar1, metric2, *metric3*\)**

The corresponding output time series value:
+ has the value of **metric2**, if **scalar1** is TRUE\.
+ has the value of **metric3**, if **scalar1** is FALSE\.
+ is an empty time series, if **metric3** is omitted from the expression\.

### Use case examples for IF expressions<a name="using-IF-expressions-use-cases"></a>

The following examples illustrate the possible uses of the **IF** function\.
+ To display only the low values of a metric:

  **IF\(metric1<400, metric1\)**
+ To change each data point in a metric to one of two values, to show relative highs and lows of the original metric:

  **IF\(metric1<400, 10, 2\)**
+ To display a 1 for each timestamp where latency is over the threshold, and display a 0 for all other data points:

  **IF\(latency>threshold, 1, 0\)**

### Use metric math with the GetMetricData API operation<a name="using-metrics-expression-api"></a>

You can use `GetMetricData` to perform calculations using math expressions, and also retrieve large batches of metric data in one API call\. For more information, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)\.

## Anomaly detection on metric math<a name="using-anomaly-detection-on-metric-math"></a>

Anomaly detection on metric math is a feature that you can use to create anomaly detection alarms on single metrics and the outputs of metric math expressions\. You can use these expressions to create graphs that visualize anomaly detection bands\. The feature supports basic arithmetic functions, comparison and logical operators, and most other functions\.

**Anomaly detection on metric math doesn't support the following functions:**
+ Expressions that contain more than one **ANOMALY\_DETECTION\_BAND** in the same line\.
+ Expressions that contain more than 10 metrics or math expressions\.
+ Expressions that contain the **METRICS** expression\.
+ Expressions that contain the **SEARCH** function\.
+ Expressions that use metrics with different periods\.
+ Metric math anonmaly detectors that use high\-resolution metrics as input\.

For more information about this feature, see [Using CloudWatch anomaly detection](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html#anomaly_detection_on_metric_math) in the *Amazon CloudWatch User Guide*\.