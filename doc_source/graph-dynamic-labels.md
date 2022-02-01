# Using dynamic labels<a name="graph-dynamic-labels"></a>

You can use dynamic labels with your graphs\. Dynamic labels add a dynamically updated value to the label for the selected metric\. You can add a wide range of values to the labels, as shown in the following tables\.

The dynamic value shown in the label is derived from the time range currently shown on the graph\. The dynamic part of the label automatically updates when either the dashboard or the graph is refreshed\. 

If you use a dynamic label with a search expression, the dynamic label applies to every metric returned by the search\. 

You can use the CloudWatch console to add a dynamic value to a label, edit the label, change the position of the dynamic value within the label column, and make other customizations\.

## Dynamic labels<a name="dynamic-label-syntax"></a>

Within a dynamic label, you can use the following values relating to properties of the metric:


| Dynamic label live value | Description | 
| --- | --- | 
|  $\{AVG\} |  The average of the values in the time range currently shown in the graph\.  | 
|  $\{DATAPOINT\_COUNT\} |  The number of data points in the time range that is currently shown in the graph\.  | 
|  $\{FIRST\} |  The oldest of the metric values in the time range that is currently shown in the graph\.  | 
|  $\{FIRST\_LAST\_RANGE\} |  The difference between the metric values of the oldest and newest data points that are currently shown in the graph\.  | 
|  $\{FIRST\_LAST\_TIME\_RANGE\} |  The absolute time range between the oldest and newest data points that are currently shown in the graph\.  | 
|  $\{FIRST\_TIME\} |  The timestamp of the oldest data point in the time range that is currently shown in the graph\.  | 
|  $\{FIRST\_TIME\_RELATIVE\} |  The absolute time difference between now and the timestamp of the oldest data point in the time range that is currently shown in the graph\.  | 
|  $\{LABEL\} |  The representation of the default label for a metric\.  | 
|  $\{LAST\} |  The most recent of the metric values in the time range that is currently shown in the graph\.  | 
|  $\{LAST\_TIME\} |  The timestamp of the newest data point in the time range that is currently shown in the graph\.  | 
|  $\{LAST\_TIME\_RELATIVE\} |  The absolute time difference between now and the timestamp of the newest data point in the time range that is currently shown in the graph\.  | 
|  $\{MAX\} |  The maximum of the values in the time range currently shown in the graph\.  | 
|  $\{MAX\_TIME\} |  The timestamp of the data point that has the highest metric value, of the data points that are currently shown in the graph\.  | 
|  $\{MAX\_TIME\_RELATIVE\} |  The absolute time difference between now and the timestamp of the data point with the highest value, of those data points that are currently shown in the graph\.  | 
|  $\{MIN\} |  The minimum of the values in the time range currently shown in the graph\.  | 
|  $\{MIN\_MAX\_RANGE\} |  The difference in metric values between the data points with the highest and lowest metric values, of those data points that are currently shown in the graph\.  | 
|  $\{MIN\_MAX\_TIME\_RANGE\} |  The absolute time range between the data points with the highest and lowest metric values, of those data points that are currently shown in the graph\.  | 
|  $\{MIN\_TIME\} |  The timestamp of the data point that has the lowest metric value, of the data points that are currently shown in the graph\.  | 
|  $\{MIN\_TIME\_RELATIVE\} |  The absolute time difference between now and the timestamp of the data point with the lowest value, of those data points that are currently shown in the graph\.  | 
|  $\{PROP\('AccountId'\)\} |  The AWS account ID of the metric\.  | 
|  $\{PROP\('Dim\.*dimension\_name*'\)\} |  The value of the specified dimension\.  | 
|  $\{PROP\('MetricName'\)\} |  The name of the metric\.  | 
|  $\{PROP\('Namespace'\)\} |  The namespace of the metric\.  | 
|  $\{PROP\('Period'\)\} |  The period of the metric, in seconds\.  | 
|  $\{PROP\('Region'\)\} |  The AWS Region where the metric is published\.  | 
|  $\{PROP\('Stat'\)\} |  The metric statistic that is being graphed\.  | 
|  $\{SUM\} |  The sum of the values in the time range currently shown in the graph\.  | 

For example, suppose you have a search expression **SEARCH\(' \{AWS/Lambda, FunctionName\} Errors ', 'Sum', 300\)**, which finds the `Errors` for each of your Lambda functions\. If you set the label to be `[max: ${MAX} Errors for Function Name ${LABEL}]`, the label for each metric is **\[max: *number* Errors for Function Name *Name*\]**\.

You can add up to five dynamic values to a label\. You can use the `${LABEL}` placeholder only once within each label\.