# Using Dynamic Labels<a name="graph-dynamic-labels"></a>

You can use dynamic labels with your graphs\. Dynamic labels add a dynamically updated value to the label for the selected metric\. The values displayed can include the average, maximum, minimum, sum, or the most recent data point\. The dynamic value shown in the label is derived from the time range currently shown on the graph\. This part of the label automatically updates when either the dashboard or the graph is refreshed\. 

If you use a dynamic label with a search expression, the dynamic label applies to every metric returned by the search\. 

The CloudWatch console makes it easy for you to add a dynamic value to a label\. You can then further edit the label, to change the position of the dynamic value within the label or make other customizations\.

## Dynamic Label Syntax<a name="dynamic-label-syntax"></a>

Within a dynamic label, the dynamic values can include the following:


| Variable | Description | 
| --- | --- | 
|  $\{AVG\} |  The average of the values in the time range currently shown in the graph\.  | 
|  $\{DATAPOINT_COUNT\} | The number of data points in the time range that is currently shown in the graph\.  | 
|  $\{FIRST\} |  The oldest of the metric values in the time range that is currently shown in the graph\.  |
|  $\{FIRST_LAST_RANGE\} |  The difference between the metric values of the oldest and newest data points that are currently shown in the graph\.  | 
|  $\{FIRST_LAST_TIME_RANGE\} |  The absolute time range between the oldest and newest data points that are currently shown in the graph\.  | 
|  $\{FIRST_TIME\} |  The timestamp of the oldest data point in the time range that is currently shown in the graph\.  | 
|  $\{FIRST_TIME_RELATIVE\} |  The absolute time difference between now and the timestamp of the oldest data point in the time range that is currently shown in the graph\.  | 
|  $\{LABEL\} |  Represents the default label for a metric\.  | 
|  $\{LAST\} |  The most recent of the values in the time range currently shown in the graph\.  | 
|  $\{LAST_TIME\} |  The timestamp of the newest data point in the time range that is currently shown in the graph\.  | 
|  $\{LAST_TIME_RELATIVE\} |  The absolute time difference between now and the timestamp of the newest data point in the time range that is currently shown in the graph\.  | 
|  $\{MAX\} |  The maximum of the values in the time range currently shown in the graph\.  | 
|  $\{MIN\} |  The minimum of the values in the time range currently shown in the graph\.  | 
|  $\{MIN_MAX_RANGE\} |  The difference in metric values between the data points with the highest and lowest metric values, of those data points that are currently shown in the graph\.  |
|  $\{MIN_MAX_TIME_RANGE\} |  The absolute time range between the data points with the highest and lowest metric values, of those data points that are currently shown in the graph\.  |
|  $\{MIN_TIME\} |  The timestamp of the data point that has the lowest metric value, of the data points that are currently shown in the graph\.  |
|  $\{MIN_TIME_RELATIVE\} |  The absolute time difference between now and the timestamp of the data point with the lowest value, of those data points that are currently shown in the graph\.  |
|  $\{PROP('AccountId')\} |  The AWS account ID of the metric\.  |
|  $\{PROP('Dim.*dimension_name*')\}   |  The value of the specified dimension\.  |
|  $\{PROP('MetricName')\} |  The name of the metric\.  |
|  $\{PROP('Namespace')\} |  The namespace of the metric\.  |
|  $\{PROP('Period')\} |  The period of the metric, in seconds\.  |
|  $\{PROP('Region')\} |  The AWS Region where the metric is published\.  |
|  $\{PROP('Stat')\} |  The metric statistic that is being graphed\.  |
|  $\{SUM\} |  The sum of the values in the time range currently shown in the graph\.  | 


For example, suppose you have a search expression **SEARCH\(' \{AWS/Lambda, FunctionName\} Errors ', 'Sum', 300\)**, which finds the `Errors` for each of your Lambda functions\. If you set the label to be `[max: ${MAX} Errors for Function Name ${LABEL}]`, the label for each metric is **\[max: *number* Errors for Function Name *Name*\]**\.

You can add up to five dynamic statistical values to a label\. You can use the `${LABEL}` placeholder only once within each label\.
