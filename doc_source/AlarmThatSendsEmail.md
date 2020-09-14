# Using Amazon CloudWatch Alarms<a name="AlarmThatSendsEmail"></a>

You can create both *metric alarms* and *composite alarms* in CloudWatch\.
+ A *metric alarm* watches a single CloudWatch metric or the result of a math expression based on CloudWatch metrics\. The alarm performs one or more actions based on the value of the metric or expression relative to a threshold over a number of time periods\. The action can be an Amazon EC2 action, an Auto Scaling action, or a notification sent to an Amazon SNS topic\.
+ A *composite alarm* includes a rule expression that takes into account the alarm states of other alarms that you have created\. The composite alarm goes into ALARM state only if all conditions of the rule are met\. The alarms specified in a composite alarm's rule expression can include metric alarms and other composite alarms\.

  Using composite alarms can reduce alarm noise\. You can create multiple metric alarms, and also create a composite alarm and set up alerts only for the composite alarm\. For example, a composite might go into ALARM state only when all of the underlying metric alarms are in ALARM state\.

  Composite alarms can send Amazon SNS notifications when they change state, but can't perform EC2 actions or Auto Scaling actions\.

You can add alarms to CloudWatch dashboards and monitor them visually\. When an alarm is on a dashboard, it turns red when it is in the `ALARM` state, making it easier for you to monitor its status proactively\.

An alarm invokes actions only when the alarm changes state\. The exception is for alarms with Auto Scaling actions\. For Auto Scaling actions, the alarm continues to invoke the action once per minute that the alarm remains in the new state\. 

**Note**  
CloudWatch doesn't test or validate the actions that you specify, nor does it detect any Amazon EC2 Auto Scaling or Amazon SNS errors resulting from an attempt to invoke nonexistent actions\. Make sure that your alarm actions exist\.

## Metric Alarm States<a name="alarm-states"></a>

A metric alarm has the following possible states:
+ `OK` – The metric or expression is within the defined threshold\.
+ `ALARM` – The metric or expression is outside of the defined threshold\.
+ `INSUFFICIENT_DATA` – The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state\.

## Evaluating an Alarm<a name="alarm-evaluation"></a>

When you create an alarm, you specify three settings to enable CloudWatch to evaluate when to change the alarm state:
+ **Period** is the length of time to evaluate the metric or expression to create each individual data point for an alarm\. It is expressed in seconds\. If you choose one minute as the period, the alarm evaluates the metric once per minute\.
+ **Evaluation Periods** is the number of the most recent periods, or data points, to evaluate when determining alarm state\.
+ **Datapoints to Alarm** is the number of data points within the Evaluation Periods that must be breaching to cause the alarm to go to the `ALARM` state\. The breaching data points don't have to be consecutive, they just must all be within the last number of data points equal to **Evaluation Period**\.

In the following figure, the alarm threshold for a metric alarm is set to three units\. Both **Evaluation Period** and **Datapoints to Alarm** are 3\. That is, when all existing data points in the most recent three consecutive periods are above the threshold, the alarm goes to `ALARM` state\. In the figure, this happens in the third through fifth time periods\. At period six, the value dips below the threshold, so one of the periods being evaluated is not breaching, and the alarm state changes back to `OK`\. During the ninth time period, the threshold is breached again, but for only one period\. Consequently, the alarm state remains `OK`\.

![\[Alarm threshold trigger alarm\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/alarm_graph.png)

When you configure **Evaluation Periods** and **Datapoints to Alarm** as different values, you're setting an "M out of N" alarm\. **Datapoints to Alarm** is \("M"\) and **Evaluation Periods** is \("N"\)\. The evaluation interval is the number of data points multiplied by the period\. For example, if you configure 4 out of 5 data points with a period of 1 minute, the evaluation interval is 5 minutes\. If you configure 3 out of 3 data points with a period of 10 minutes, the evaluation interval is 30 minutes\.

**Note**  
If data points are missing soon after you create an alarm, and the metric was being reported to CloudWatch before you created the alarm, CloudWatch retrieves the most recent data points from before the alarm was created when evaluating the alarm\.

## Configuring How CloudWatch Alarms Treat Missing Data<a name="alarms-and-missing-data"></a>

Sometimes, not every expected data point for a metric gets reported to CloudWatch\. For example, this can happen when a connection is lost, a server goes down, or when a metric reports data only intermittently by design\.

CloudWatch enables you to specify how to treat missing data points when evaluating an alarm\. This helps you to configure your alarm so that it goes to `ALARM` state only when appropriate for the type of data being monitored\. You can avoid false positives when missing data doesn't indicate a problem\.

Similar to how each alarm is always in one of three states, each specific data point reported to CloudWatch falls under one of three categories:
+ Not breaching \(within the threshold\)
+ Breaching \(violating the threshold\)
+ Missing

For each alarm, you can specify CloudWatch to treat missing data points as any of the following:
+ `notBreaching` – Missing data points are treated as "good" and within the threshold,
+ `breaching` – Missing data points are treated as "bad" and breaching the threshold
+ `ignore` – The current alarm state is maintained
+ `missing` – If all data points in the alarm evaluation range are missing, the alarm transitions to INSUFFICIENT\_DATA\.

The best choice depends on the type of metric\. For a metric that continually reports data, such as `CPUUtilization` of an instance, you might want to treat missing data points as `breaching`, because they might indicate that something is wrong\. But for a metric that generates data points only when an error occurs, such as `ThrottledRequests` in Amazon DynamoDB, you would want to treat missing data as `notBreaching`\. The default behavior is `missing`\.

Choosing the best option for your alarm prevents unnecessary and misleading alarm condition changes, and also more accurately indicates the health of your system\.

### How Alarm State Is Evaluated When Data Is Missing<a name="alarms-evaluating-missing-data"></a>

Whenever an alarm evaluates whether to change state, CloudWatch attempts to retrieve a higher number of data points than the number specified as **Evaluation Periods**\. The exact number of data points it attempts to retrieve depends on the length of the alarm period and whether it is based on a metric with standard resolution or high resolution\. The time frame of the data points that it attempts to retrieve is the *evaluation range*\.

Once CloudWatch retrieves these data points, the following happens:
+ If no data points in the evaluation range are missing, CloudWatch evaluates the alarm based on the most recent data points collected\. The number of data points evaluated is equal to the **Evaluation Periods** for the alarm\. The extra data points from farther back in the evaluation range are not needed and are ignored\.
+ If some data points in the evaluation range are missing, but the total number of existing data points that were successfully retrieved from the evaluation range is equal to or more than the alarm's **Evaluation Periods**, CloudWatch evaluates the alarm state based on the most recent real data points that were successfully retrieved, including the necessary extra data points from farther back in the evaluation range\. In this case, the value you set for how to treat missing data is not needed and is ignored\.
+ If some data points in the evaluation range are missing, and the number of actual data points that were retrieved is lower than the alarm's number of **Evaluation Periods**, CloudWatch fills in the missing data points with the result you specified for how to treat missing data, and then evaluates the alarm\. However, all real data points in the evaluation range are included in the evaluation\. CloudWatch uses missing data points only as few times as possible\. 

**Note**  
A particular case of this behavior is that CloudWatch alarms might repeatedly re\-evaluate the last set of data points for a period of time after the metric has stopped flowing\. This re\-evaluation might cause the alarm to change state and re\-execute actions, if it had changed state immediately prior to the metric stream stopping\. To mitigate this behavior, use shorter periods\.

The following tables illustrate examples of the alarm evaluation behavior\. In the first table, **Datapoints to Alarm** and **Evaluation Periods** are both 3\. CloudWatch retrieves the 5 most recent data points when evaluating the alarm, in case some of the most recent 3 data points are missing\. 5 is the evaluation range for the alarm\.

Column 1 shows the 5 most recent data points, because the evaluation range is 5\. These data points are shown with the most recent data point on the right\. 0 is a non\-breaching data point, X is a breaching data point, and \- is a missing data point\.

Column 2 shows how many of the 3 necessary data points are missing\. Even though the most recent 5 data points are evaluated, only 3 \(the setting for **Evaluation Periods**\) are necessary to evaluate the alarm state\. The number of data points in Column 2 is the number of data points that must be "filled in", using the setting for how missing data is being treated\. 

In columns 3\-6, the column headers are the possible values for how to treat missing data\. The rows in these columns show the alarm state that is set for each of these possible ways to treat missing data\.


| Data points | \# of data points that must be filled | MISSING | IGNORE | BREACHING | NOT BREACHING | 
| --- | --- | --- | --- | --- | --- | 
|  0 \- X \- X  |  0  |  `OK`  |  `OK`  |  `OK`  |  `OK`  | 
|  0 \- \- \- \-  |  2  |  `OK`  |  `OK`  |  `OK`  |  `OK`  | 
|  \- \- \- \- \-  |  3  |  `INSUFFICIENT_DATA`  |  Retain current state  |  `ALARM`  |  `OK`  | 
|  0 X X \- X  |  0  |  `ALARM`  |  `ALARM`  |  `ALARM`  |  `ALARM`  | 
|  \- \- X \- \-   |  2  |  `ALARM`  |  `Retain current state`  |  `ALARM`  |  `OK`  | 

In the second row of the preceding table, the alarm stays `OK` even if missing data is treated as breaching, because the one existing data point is not breaching, and this is evaluated along with two missing data points which are treated as breaching\. The next time this alarm is evaluated, if the data is still missing it will go to `ALARM`, as that non\-breaching data point will no longer be in the evaluation range\.

The third row, where all five of the most recent data points are missing, illustrates how the various settings for how to treat missing data affect the alarm state\. If missing data points are considered breaching, the alarm goes into ALARM state, while if they are considered not breaching, then the alarm goes into OK state\. If missing data points are ignored, the alarm retains the current state it had before the missing data points\. And if missing data points are just considered as missing, then the alarm does not have enough recent real data to make an evaluation, and goes into INSUFFICIENT\_DATA\.

In the fourth row, the alarm goes to `ALARM` state in all cases because the three most recent data points are breaching, and the alarm's **Evaluation Periods** and **Datapoints to Alarm** are both set to 3\. In this case, the missing data point is ignored and the setting for how to evaluate missing data is not needed, because there are 3 real data points to evaluate\.\.

Row 5 represents a special case of alarm evaluation called *premature alarm state*\. For more information, see [Avoiding Premature Transitions to Alarm State](#CloudWatch-alarms-avoiding-premature-transition)\.

In the next table, the **Period** is again set to 5 minutes, and **Datapoints to Alarm** is only 2 while **Evaluation Periods** is 3\. This is a 2 out of 3, M out of N alarm\.

The evaluation range is 5\. This is the maximum number of recent data points that are retrieved and can be used in case some data points are missing\.


| Data points | \# of missing data points | MISSING | IGNORE | BREACHING | NOT BREACHING | 
| --- | --- | --- | --- | --- | --- | 
|  0 \- X \- X  |  0  |  `ALARM`  |  `ALARM`  |  `ALARM`  |  `ALARM`  | 
|  0 0 X 0 X  |  0  |  `ALARM`  |  `ALARM`  |  `ALARM`  |  `ALARM`  | 
|  0 \- X \- \-  |  1  |  `OK`  |  `OK`  |  `ALARM`  |  `OK`  | 
|  \- \- \- \- 0  |  2  |  `OK`  |  `OK`  |  `ALARM`  |  `OK`  | 
|  \- \- \- X \-  |  2  |  `ALARM`  |  Retain current state  |  `ALARM`  |  `OK`  | 

In rows 1 and 2, the alarm always goes to ALARM state because 2 of the 3 most recent data points are breaching\. In row 2, the two oldest data points in the evaluation range are not needed because none of the 3 most recent data points are missing, so these two older data points are ignored\.

In rows 3 and 4, the alarm goes to ALARM state only if missing data is treated as breaching, in which case the two most recent missing data points are both treated as breaching\. In row 4, these two missing data points that are treated as breaching provide the two necessary breaching data points to trigger the ALARM state\.

Row 5 represents a special case of alarm evaluation called *premature alarm state*\. For more information, see the following section\.

#### Avoiding Premature Transitions to Alarm State<a name="CloudWatch-alarms-avoiding-premature-transition"></a>

CloudWatch alarm evaluation includes logic to try to avoid false alarms, where the alarm goes into ALARM state prematurely when data is intermittent\. The example shown in row 5 in the tables in the previous section illustrate this logic\. In those rows, and in the following examples, the **Evaluation Periods** is 3 and the evaluation range is 5 data points\. **Datapoints to Alarm** is 3, except for the M out of N example, where **Datapoints to Alarm** is 2\.

Suppose an alarm's most recent data is `- - - - X`, with four missing data points and then a breaching data point as the most recent data point\. Because the next data point may be non\-breaching, the alarm does not go immediately into ALARM state when the data is either `- - - - X` or `- - - X -` and **Datapoints to Alarm** is 3\. This way, false positives are avoided when the next data point is non\-breaching and causes the data to be `- - - X O` or `- - X - O`\.

However, if the last few data points are `- - X - -`, the alarm goes into ALARM state even if missing data points are treated as missing\. This is because alarms are designed to always go into ALARM state when the oldest available breaching datapoint during the Evaluation Periods number of data points is at least as old as the value of **Datapoints to Alarm**, and all other more recent data points are breaching or missing\. In this casethe alarm goes into ALARM state no matter if the total number of datapoints available is lower than M \(**Datapoints to Alarm**\)\.

This alarm logic applies to M out of N alarms as well\. If the oldest breaching data point during the **Evaluation Periods** number of data points is at least as old as the value of **Evaluation Periods**, and all of the more recent data points are either breaching or missing, the alarm goes into ALARM state no matter the value of M \(**Datapoints to Alarm**\)\.

## High\-Resolution Alarms<a name="high-resolution-alarms"></a>

 If you set an alarm on a high\-resolution metric, you can specify a high\-resolution alarm with a period of 10 seconds or 30 seconds, or you can set a regular alarm with a period of any multiple of 60 seconds\. There is a higher charge for high\-resolution alarms\. For more information about high\-resolution metrics, see [Publishing Custom Metrics](publishingMetrics.md)\.

## Alarms on Math Expressions<a name="alarms-on-metric-math-expressions"></a>

 You can set an alarm on the result of a math expression that is based on one or more CloudWatch metrics\. A math expression used for an alarm can include as many as 10 metrics\. Each metric must be using the same period\.

For an alarm based on a math expression, you can specify how you want CloudWatch to treat missing data points for the underlying metrics when evaluating the alarm\. 

Alarms based on math expressions can't perform Amazon EC2 actions\.

For more information about metric math expressions and syntax, see [Using Metric Math](using-metric-math.md)\.

## Percentile\-Based CloudWatch Alarms and Low Data Samples<a name="percentiles-with-low-samples"></a>

When you set a percentile as the statistic for an alarm, you can specify what to do when there is not enough data for a good statistical assessment\. You can choose to have the alarm evaluate the statistic anyway and possibly change the alarm state\. Or, you can have the alarm ignore the metric while the sample size is low, and wait to evaluate it until there is enough data to be statistically significant\.

For percentiles between 0\.5 \(inclusive\) and 1\.00 \(exclusive\), this setting is used when there are fewer than 10/\(1\-percentile\) data points during the evaluation period\. For example, this setting would be used if there were fewer than 1000 samples for an alarm on a p99 percentile\. For percentiles between 0 and 0\.5 \(exclusive\), the setting is used when there are fewer than 10/percentile data points\.

## Common Features of CloudWatch Alarms<a name="common-features-of-alarms"></a>

The following features apply to all CloudWatch alarms:
+ You can create up to 5000 alarms per Region per AWS account\. To create or update an alarm, you use the CloudWatch console, the [PutMetricAlarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html) API action, or the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command in the AWS CLI\.
+ Alarm names must contain only ASCII characters\.
+ You can list any or all of the currently configured alarms, and list any alarms in a particular state by using the CloudWatch console, the [DescribeAlarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarms.html) API action, or the [describe\-alarms](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/describe-alarms.html) command in the AWS CLI\.
+ You can disable and enable alarms by using the CloudWatch console, the [DisableAlarmActions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DisableAlarmActions.html) and [EnableAlarmActions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_EnableAlarmActions.html) API actions, or the [disable\-alarm\-actions](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/disable-alarm-actions.html) and [enable\-alarm\-actions](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/enable-alarm-actions.html) commands in the AWS CLI\. 
+ You can test an alarm by setting it to any state using the [SetAlarmState](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_SetAlarmState.html) API action or the [set\-alarm\-state](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/set-alarm-state.html) command in the AWS CLI\. This temporary state change lasts only until the next alarm comparison occurs\.
+ You can create an alarm for a custom metric before you've created that custom metric\. For the alarm to be valid, you must include all of the dimensions for the custom metric in addition to the metric namespace and metric name in the alarm definition\. To do this, you can use the [PutMetricAlarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html) API action, or the [put\-metric\-alarm](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command in the AWS CLI\.
+ You can view an alarm's history using the CloudWatch console, the [DescribeAlarmHistory](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_DescribeAlarmHistory.html) API action, or the [describe\-alarm\-history](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/describe-alarm-history.html) command in the AWS CLI\. CloudWatch preserves alarm history for two weeks\. Each state transition is marked with a unique timestamp\. In rare cases, your history might show more than one notification for a state change\. The timestamp enables you to confirm unique state changes\.
+ The number of evaluation periods for an alarm multiplied by the length of each evaluation period can't exceed one day\.

**Note**  
Some AWS resources don't send metric data to CloudWatch under certain conditions\.  
For example, Amazon EBS might not send metric data for an available volume that is not attached to an Amazon EC2 instance, because there is no metric activity to be monitored for that volume\. If you have an alarm set for such a metric, you might notice its state change to `INSUFFICIENT_DATA`\. This might indicate that your resource is inactive, and might not necessarily mean that there is a problem\. You can specify how each alarm treats missing data\. For more information, see [Configuring How CloudWatch Alarms Treat Missing Data](#alarms-and-missing-data)\.