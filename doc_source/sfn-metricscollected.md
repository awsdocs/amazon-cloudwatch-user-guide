# AWS Step Functions Metrics and Dimensions<a name="sfn-metricscollected"></a>

The following metrics are available for AWS Step Functions\. For more information, see [Monitoring Step Functions Using CloudWatch](http://docs.aws.amazon.com/step-functions/latest/dg/procedure-cw-metrics.html) in the *AWS Step Functions Developer Guide*\.

## Execution Metrics<a name="cloudwatch-step-functions-execution-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions executions:


| Metric | Description | 
| --- | --- | 
| ExecutionTime | The interval, in milliseconds, between the time the execution starts and the time it closes\. | 
| ExecutionThrottled | The number of StateEntered events and retries that have been throttled\. This is related to StateTransition throttling\. For more information, see [Limits Related to State Throttling](http://docs.aws.amazon.com/step-functions/latest/dg/limits.html#service-limits-api-state-throttling) in the AWS Step Functions Developer Guide\. | 
| ExecutionsAborted | The number of aborted or terminated executions\. | 
| ExecutionsFailed | The number of failed executions\. | 
| ExecutionsStarted | The number of started executions\. | 
| ExecutionsSucceeded | The number of successfully completed executions\. | 
| ExecutionsTimedOut | The number of executions that time out for any reason\. | 

### Dimension for Step Functions Execution Metrics<a name="cloudwatch-step-functions-execution-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  StateMachineArn  |  The ARN of the state machine for the execution in question\.  | 

## Activity Metrics<a name="cloudwatch-step-functions-activity-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions activities:


| Metric | Description | 
| --- | --- | 
| ActivityRunTime  | The interval, in milliseconds, between the time the activity starts and the time it closes\. | 
| ActivityScheduleTime | The interval, in milliseconds, for which the activity stays in the schedule state\. | 
| ActivityTime | The interval, in milliseconds, between the time the activity is scheduled and the time it closes\. | 
| ActivitiesFailed | The number of failed activities\. | 
| ActivitiesHeartbeatTimedOut | The number of activities that time out due to a heartbeat timeout\. | 
| ActivitiesScheduled | The number of scheduled activities\. | 
| ActivitiesStarted | The number of started activities\. | 
| ActivitiesSucceeded | The number of successfully completed activities\. | 
| ActivitiesTimedOut | The number of activities that time out on close\. | 

### Dimension for Step Functions Activity Metrics<a name="cloudwatch-step-functions-activity-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
|  `ActivityArn`  |  The ARN of the activity\.  | 

## Lambda Function Metrics<a name="cloudwatch-step-functions-lambda-function-metrics"></a>

The `AWS/States` namespace includes the following metrics for Step Functions Lambda functions:


|  Metric  |  Description  | 
| --- | --- | 
| LambdaFunctionRunTime | The interval, in milliseconds, between the time the Lambda function starts and the time it closes\. | 
| LambdaFunctionScheduleTime | The interval, in milliseconds, for which the Lambda function stays in the schedule state\. | 
| LambdaFunctionTime | The interval, in milliseconds, between the time the Lambda function is scheduled and the time it closes\. | 
| LambdaFunctionsFailed | The number of failed Lambda functions\. | 
| LambdaFunctionsHeartbeatTimedOut | The number of Lambda functions that time out due to a heartbeat timeout\. | 
| LambdaFunctionsScheduled | The number of scheduled Lambda functions\. | 
| LambdaFunctionsStarted | The number of started Lambda functions\. | 
| LambdaFunctionsSucceeded | The number of successfully completed Lambda functions\. | 
| LambdaFunctionsTimedOut | The number of Lambda functions that time out on close\. | 

### Dimension for Step Functions Lambda Function Metrics<a name="cloudwatch-step-functions-lambda-function-metrics-dimensions"></a>


|  Dimension  |  Description  | 
| --- | --- | 
|  `LambdaFunctionArn`  |  The ARN of the Lambda function\.  | 