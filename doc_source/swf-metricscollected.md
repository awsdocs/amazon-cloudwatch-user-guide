# Amazon SWF Metrics and Dimensions<a name="swf-metricscollected"></a>

Amazon SWF sends data points to CloudWatch for several metrics\. Some of the Amazon SWF metrics for CloudWatch are time intervals, always measured in milliseconds\. These metrics generally correspond to stages of your workflow execution for which you can set workflow and activity timeouts, and have similar names\. For example, the **DecisionTaskStartToCloseTime** metric measures the time it took for the decision task to complete after it began executing, which is the same time period for which you can set a **DecisionTaskStartToCloseTimeout** value\.

Other Amazon SWF metrics report results as a count\. For example, **WorkflowsCanceled**, records a result as either one or zero, indicating whether or not the workflow was canceled\. A value of zero does not indicate that the metric was not reported, only that the condition described by the metric did not occur\. For count metrics, minimum and maximum will always be either zero or one, but average will be a value ranging from zero to one\. For more information, see [Viewing Amazon SWF Metrics for CloudWatch using the AWS Management Console;](http://docs.aws.amazon.com/amazonswf/latest/developerguide/cw-metrics-console.html) in the *Amazon Simple Workflow Service Developer Guide*\.

## Workflow Metrics<a name="cloudwatch-workflow-metrics"></a>

The `AWS/SWF` namespace includes the following metrics for Amazon SWF workflows:


| Metric | Description | 
| --- | --- | 
|  DecisionTaskScheduleToStartTime  |  The time interval, in milliseconds, between the time that the decision task was scheduled and the time it was picked up by a worker and started\.  | 
|  DecisionTaskStartToCloseTime  |  The time interval, in milliseconds, between the time that the decision task was started and the time it was closed\.  | 
|  DecisionTasksCompleted  |  The count of decision tasks that have been completed\.  | 
|  StartedDecisionTasksTimedOutOnClose  |  The count of decision tasks that started but timed out on closing\.  | 
|  WorkflowStartToCloseTime  |  The time, in milliseconds, between the time the workflow started and the time it closed\.  | 
|  WorkflowsCanceled  |  The count of workflows that were canceled\.  | 
|  WorkflowsCompleted  |  The count of workflows that completed\.  | 
|  WorkflowsContinuedAsNew  |  The count of workflows that continued as new\.  | 
|  WorkflowsFailed  |  the count of workflows that failed\.  | 
|  WorkflowsTerminated  |  the count of workflows that were terminated\.  | 
|  WorkflowsTimedOut  |  The count of workflows that timed out, for any reason\.  | 

### Dimensions for Amazon SWF Workflow Metrics<a name="cloudwatch-swf-workflow-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  Domain  |  The Amazon SWF domain that the workflow is running in\.  | 
|  WorkflowTypeName  |  The name of the workflow type for this workflow execution\.  | 
|  WorkflowTypeVersion  |  The version of the workflow type for this workflow execution\.  | 

## Activity Metrics<a name="cloudwatch-activity-metrics"></a>

The `AWS/SWF` namespace includes the following metrics for Amazon SWF activities:


| Metric | Description | 
| --- | --- | 
|  ActivityTaskScheduleToCloseTime  |  The time interval, in milliseconds, between the time when the activity was scheduled to when it closed\.  | 
|  ActivityTaskScheduleToStartTime  |  The time interval, in milliseconds, between the time when the activity task was scheduled and when it started\.  | 
|  ActivityTaskStartToCloseTime  |  The time interval, in milliseconds, between the time when the activity task started and when it was closed\.  | 
|  ActivityTasksCanceled  |  The count of activity tasks that were canceled\.  | 
|  ActivityTasksCompleted  |  The count of activity tasks that completed\.  | 
|  ActivityTasksFailed  |  The count of activity tasks that failed\.  | 
|  ScheduledActivityTasksTimedOutOnClose  |  The count of activity tasks that were scheduled but timed out on close\.  | 
|  ScheduledActivityTasksTimedOutOnStart  |  The count of activity tasks that were scheduled but timed out on start\.  | 
|  StartedActivityTasksTimedOutOnClose  |  The count of activity tasks that were started but timed out on close\.  | 
|  StartedActivityTasksTimedOutOnHeartbeat  |  The count of activity tasks that were started but timed out due to a heartbeat timeout\.  | 

### Dimensions for Amazon SWF Activity Metrics<a name="cloudwatch-swf-activity-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  Domain  |  The Amazon SWF domain that the activity is running in\.  | 
|  ActivityTypeName  |  The name of the activity type\.  | 
|  ActivityTypeVersion  |  The version of the activity type  | 