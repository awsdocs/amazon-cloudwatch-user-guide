# Use Metric Math<a name="using-metric-math"></a>

Metric math enables you to query multiple CloudWatch metrics and use math expressions to create new time series based on these metrics\. You can visualize the resulting time series in the CloudWatch console and add them to dashboards\. For an example using AWS Lambda metrics, you could divide the **Errors** metric by the **Invocations** metric to get an error rate, and add the resulting time series to a graph on your CloudWatch dashboard\.

You can also perform metric math programmatically, using the `GetMetricData` API operation\.

## Adding a Math Expression to a CloudWatch Graph<a name="adding-metrics-expression-console"></a>

You can add a math expression to a graph on your CloudWatch dashboard\. Each graph is limited to a maximum of 100 metrics and expressions, so you can add a math expression only if the graph has 99 or fewer metrics\.

**To add a math expression to a graph**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Create or edit a graph or line widget\.

1. Choose **Graphed metrics**\.

1. Choose **Add a math expression**\. A new line appears for the expression\.

1. For the **Details** column, type the math expression\. The tables in the following section list the functions you can use in the expression\.

   To use a metric or the result of another expression as part of the formula for this expression, use the value shown in the **Id** column\. For example, **m1\+m2** or **e1\-MIN\(e1\)**\.

   You can change the value of **Id**\. It can include numbers, letters, and underscore, and must start with a lowercase letter\. Changing the value of **Id** to a more meaningful name can also make a graph easier to understand\. For example, changing from **m1** and **m2** to **errors** and **requests**\.

1. For the **Label** column of the expression, type a name that describes what the expression is calculating\.

   If the result of an expression is an array of time series, each of those time series is displayed on the graph with a separate line, with different colors\. Immediately under the graph is a legend for each line in the graph\. For a single expression that produces multiple time series, the legend captions for those time series are in the format ***Expression\-Label Metric\-Label***\. For example, if the graph includes a metric with a label of **Errors** and an expression **FILL\(METRICS\(\), 0\)** that has a label of **Filled With 0:**, one line in the legend would be **Filled With 0: Errors**\. You can set *Expression\-Label* to be empty to have the legend show only the original metric labels\.

   When one expression produces an array of time series on the graph, you cannot change the colors used for each of those time series\.

1. After you have added the desired expressions, you can optionally simplify the graph by hiding some of the original metrics\. To hide a metric or expression, clear the check box to the left of the **Id** field\.

## Metric Math Syntax and Functions<a name="metric-math-syntax"></a>

The following sections explain the functions available for metric math\. All functions must be written in uppercase letters \(such as **AVG**\), while the **Id** field for all metrics and math expressions must start with a lowercase letter\. 

The final result of any math expression must be a single time series or an array of time series\. Some functions produce a scalar number\. You can use these functions within a larger function that ultimately produces a time series\. For example, taking the **AVG** of a single time series produces a scalar number, so it cannot be the final expression result\. But you could use it in the function **m1\-AVG\(m1\)** to display a time series of the difference between each individual data point and the average value of that data point\.

### Data Type Abbreviations<a name="metric-math-syntax-datatypes"></a>

Some functions are valid for only certain types of data\. The abbreviations in the following list are used in the tables of functions to represent the types of data supported for each function\. 
+ **S** represents a scalar number, such as 2, \-5, or 50\.25\.
+ **TS** is a time series \(a series of values for a single CloudWatch metric over time\)\. For example, the **CPUUtilization** metric for instance `i-1234567890abcdef0` over the last three days\.
+ **TS\[\]** is an array of time series, such as the time series for multiple metrics\.

### The METRICS\(\) Function<a name="metric-math-syntax-metrics-function"></a>

The **METRICS\(\)** function returns all the metrics in the request\. Math expressions are not included\.

You can use **METRICS\(\)** within a larger expression that produces a single time series or an array of time series\. For example, the expression **SUM\(METRICS\(\)\)** returns a time series \(TS\) that is the sum of the values of all the graphed metrics\. **METRICS\(\)/100** returns an array of time series, each of which is a time series showing each data point of one of the metrics divided by 100\.

You can use the **METRICS\(\)** function with a string to return only the graphed metrics that contain that string in their **Id** field\. For example, the expression **SUM\(METRICS\("errors"\)\)** returns a time series that is the sum of the values of all the graphed metrics that have ‘errors’ in their **Id** field\. You can also use **SUM\(\[METRICS\(“4xx”\), METRICS\(“5xx”\)\]\)** to match multiple strings\.

### Basic Arithmetic Functions<a name="metric-math-syntax-arithmetic"></a>

The following table lists the basic arithmetic functions that are supported\. Missing values in time series are treated as 0\. If the value of a data point causes a function to attempt to divide by zero, the data point is dropped\.


| Operation | Arguments | Examples | 
| --- | --- | --- | 
|  Arithmetic operators: \+ \- \* / ^ |  S, S S, TS TS, TS S, TS\[\] TS, TS\[\]  |  PERIOD\(m1\)/60 5 \* m1 m1 \- m2 SUM\(100/\[m1, m2\]\) AVG\(\[m1,m2\]/m3\) METRICS\(\)\*100  | 
|  Unary subtraction \-  |  S TS TS\[\]  |  \-5\*m1 \-m1 SUM\(\-\[m1, m2\]\)  | 

### Functions Supported for Metric Math<a name="metric-math-syntax-functions-list"></a>

The following table describes the functions you can use in math expressions\. Write all functions in uppercase letters\.

The final result of any math expression must be a single time series or an array of time series\. Some functions in tables in the following sections produce a scalar number\. You can use these functions within a larger function that ultimately produces a time series\. For example, taking the **AVG** of a single time series produces a scalar number, so it cannot be the final expression result\. But you could use it in the function **m1\-AVG\(m1\)** to display a time series of the difference between each individual data point and the average value of that data point\.

In the following table, every example in the **Examples** column is an expression that results in a single time series or an array of time series\. This shows how functions that return scalar numbers can be used as part of a valid expression that produces a single time series\.


| Function | Arguments | Return Type**\*** | Description | Examples | 
| --- | --- | --- | --- | --- | 
|  ABS |  TS TS\[\]  |  TS TS\[\]  | Returns the absolute value of each data point\. |  ABS\(m1\-m2\) MIN\(ABS\(\[m1, m2\]\)\) ABS\(METRICS\(\)\)  | 
|  AVG |  TS TS\[\]  |  S TS  | The **AVG** of a single time series returns a scalar representing the average of all the data points in the metric\. The **AVG** of an array of time series returns a single time series\. Missing values are treated as 0\.  |  SUM\(\[m1,m2\]\)/AVG\(m2\) AVG\(METRICS\(\)\)  | 
|  CEIL |  TSTS\[\]  |  TS TS\[\]  | Returns the ceiling of each metric \(the smallest integer greater than or equal to each value\)\. |  CEIL\(m1\) CEIL\(METRICS\(\)\) SUM\(CEIL\(METRICS\(\)\)\)  | 
|  FILL |  TS, TS/S TS\[\], TS/S  |  TS TS\[\]  | Fills the missing values of a metric with the specified filler value, when the metric values are sparse\. |  FILL\(m1,10\) FILL\(METRICS\(\), 0\) FILL\(m1, MIN\(m1\)\)  | 
|  FLOOR |  TSTS\[\]  |  TS TS\[\]  | Returns the floor of each metric \(the largest integer less than or equal to each value\)\. |  FLOOR\(m1\) FLOOR\(METRICS\(\)\)  | 
|  MAX |  TS TS\[\]  |  S TS  | The **MAX** of a single time series returns a scalar representing the maximum value of all data points in the metric\. The **MAX** value of an array of time series returns a single time series\.  |  MAX\(m1\)/m1 MAX\(METRICS\(\)\)  | 
|  METRIC\_COUNT |  TS\[\]  |  S  | Returns the number of metrics in the time series array\.  |  m1/METRIC\_COUNT\(METRICS\(\)\)  | 
|  METRICS\(\) |  null string  |  TS\[\]  | The **METRICS\(\)** function returns all the CloudWatch metrics in the request\. Math expressions are not included You can use **METRICS\(\)** within a larger expression that produces a single time series or an array of time series\. You can use the **METRICS\(\)** function with a string to return only the graphed metrics that contain that string in their **Id** field\. For example, the expression SUM\(METRICS\("errors"\)\) returns a time series that is the sum of the values of all the graphed metrics that have ‘errors’ in their **Id** field\. You can also use SUM\(\[METRICS\(“4xx”\), METRICS\(“5xx”\)\]\) to match multiple strings\.  |  AVG\(METRICS\(\)\) SUM\(METRICS\("errors"\)\)  | 
|  MIN |  TS TS\[\]  |  S TS  | The **MIN** of a single time series returns a scalar representing the minimum value of all data points in the metric\. The **MIN** of an array of time series returns a single time series\.  |  m1\-MIN\(m1\) MIN\(METRICS\(\)\)  | 
|  PERIOD |  TS  |  S  | Returns the period of the metric in seconds\. Valid input is metrics, not the results of other expressions\.  |  m1/PERIOD\(m1\)  | 
|  RATE |  TS TS\[\]  |  TS TS\[\] | Returns the rate of change of the metric, per second\. This is calculated as the difference between the latest data point value and the previous data point value, divided by the time difference in seconds between the two values\. |  RATE\(m1\) RATE\(METRICS\(\)\)  | 
|  STDDEV |  TS TS\[\]  |  S TS  | The **STDDEV** of a single time series returns a scalar representing the standard deviation of all data points in the metric\. The **STDDEV** of an array of time series returns a single time series\.  |  m1/STDDEV\(m1\) STDDEV\(METRICS\(\)\)  | 
|  SUM |  TS TS\[\]  |  S TS  | The **SUM** of a single time series returns a scalar representing the sum of the values of all data points in the metric\. The **SUM** of an array of time series returns a single time series\.  |  SUM\(METRICS\(\)\)/SUM\(m1\) SUM\(\[m1,m2\]\) SUM\(METRICS\("errors"\)\)/SUM\(METRICS\("requests"\)\)\*100  | 

**\***Using only a function that returns a scalar number is not valid, as all final results of expressions must be a single time series or an array of time series\. Instead, use these functions as part of a larger expression that returns a time series\.

## Using Metric Math with the GetMetricData API Operation<a name="using-metrics-expression-api"></a>

You can use **GetMetricData** to perform calculations using math expressions, as well as to retrieve large batches of metric data in one API call\. For more information, see [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html)\.