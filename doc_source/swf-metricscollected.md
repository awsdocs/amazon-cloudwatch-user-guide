# Amazon SWF Metrics and Dimensions<a name="swf-metricscollected"></a>

The `AWS/SWF` namespace includes the metrics in the following table\. For more information, see [Amazon SWF Metrics for CloudWatch](https://docs.aws.amazon.com/amazonswf/latest/developerguide/cw-metrics.html) in the *AWS Storage Gateway User Guide*\.


|  Metric  |  Description  | 
| --- | --- | 
|  `DecisionTaskScheduleToStartTime`  |  The time interval, in milliseconds, between the time that the decision task was scheduled and when it was picked up by a worker and started\. CloudWatch Units: `Time` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Average, Minimum, Maximum`  | 
|  `DecisionTaskStartToCloseTime`  |  The time interval, in milliseconds, between the time that the decision task was started and when it closed\. CloudWatch Units: `Time` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Average, Minimum, Maximum`  | 
|  `DecisionTasksCompleted`  |  The count of decision tasks that have been completed\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `StartedDecisionTasksTimedOutOnClose`  |  The count of decision tasks that started but timed out on closing\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `WorkflowStartToCloseTime`  |  The time, in milliseconds, between the time the workflow started and when it closed\. CloudWatch Units: `Time` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Average, Minimum, Maximum`  | 
|  `WorkflowsCanceled`  |  The count of workflows that were canceled\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `WorkflowsCompleted`  |  The count of workflows that completed\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `WorkflowsContinuedAsNew`  |  The count of workflows that continued as new\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `WorkflowsFailed`  |  The count of workflows that failed\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `WorkflowsTerminated`  |  The count of workflows that were terminated\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `WorkflowsTimedOut`  |  The count of workflows that timed out, for any reason\. CloudWatch Units: `Count` Dimensions: `Domain, WorkflowTypeName, WorkflowTypeVersion` Valid statistics: `Sum`  | 
|  `ActivityTaskScheduleToCloseTime`  |  The time interval, in milliseconds, between the time when the activity was scheduled and when it closed\. CloudWatch Units: `Time` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Average, Minimum, Maximum`  | 
|  `ActivityTaskScheduleToStartTime`  |  The time interval, in milliseconds, between the time when the activity task was scheduled and when it started\. CloudWatch Units: `Time` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Average, Minimum, Maximum`  | 
|  `ActivityTaskStartToCloseTime`  |  The time interval, in milliseconds, between the time when the activity task started and when it closed\. CloudWatch Units: `Time` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Average, Minimum, Maximum`  | 
|  `ActivityTasksCanceled`  |  The count of activity tasks that were canceled\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `ActivityTasksCompleted`  |  The count of activity tasks that completed\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `ActivityTasksFailed`  |  The count of activity tasks that failed\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `ScheduledActivityTasksTimedOutOnClose`  |  The count of activity tasks that were scheduled but timed out on close\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `ScheduledActivityTasksTimedOutOnStart`  |  The count of activity tasks that were scheduled but timed out on start\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `StartedActivityTasksTimedOutOnClose`  |  The count of activity tasks that were started but timed out on close\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `StartedActivityTasksTimedOutOnHeartbeat`  |  The count of activity tasks that were started but timed out due to a heartbeat timeout\. CloudWatch Units: `Count` Dimensions: `Domain, ActivityTypeName, ActivityTypeVersion` Valid statistics: `Sum`  | 
|  `ThrottledEvents`  |  The count of requests that have been throttled\. CloudWatch Units: `Count` Dimensions: `APIName, DecisionName` Valid statistics: `Sum`  | 
|  `ProvisionedBucketSize`  |  The count of available requests per second\. Dimensions: `APIName, DecisionName` Valid statistics: `Minimum`  | 
|  `ConsumedCapacity`  | The count of requests per second\. CloudWatch Units: `Count` Dimensions: `APIName, DecisionName` Valid statistics: `Sum`  | 
|  `ProvisionedRefillRate`  |  The count of requests per second that are allowed into the bucket\. Dimensions: `APIName, DecisionName` Valid statistics: `Minimum`  | 

The Amazon SWF metrics support the following dimensions\.


|  Dimension  |  Description  | 
| --- | --- | 
|  `Domain`  |  Filters data to the Amazon SWF domain that the workflow or activity is running in\.  | 
|  `ActivityTypeName`  |  Filters data to the name of the activity type\.  | 
|  `ActivityTypeVersion`  |  Filters data to the version of the activity type\.  | 
|  `WorkflowTypeName`  |  Filters data to the name of the workflow type for this workflow execution\.  | 
|  `WorkflowTypeVersion`  |  Filters data to the version of the workflow type for this workflow execution\.  | 
|  `APIName`  |  Filters data to an API of the specified API name\.  | 
|  `DecisionName`  |  Filters data to the specified Decision name\.  | 
| TaskListName |  Filters data to the specified Task List name\.  | 