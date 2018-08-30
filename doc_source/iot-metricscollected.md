# AWS IoT Metrics and Dimensions<a name="iot-metricscollected"></a>

When you interact with AWS IoT, it sends the following metrics to CloudWatch every minute, in the `AWS/IoT` namespace\.

## AWS IoT Metrics<a name="aws-iot-metrics"></a>

The `AWS/IoT` namespace includes the following metrics\. AWS IoT sends the following metrics to CloudWatch once per received request\.


**IoT Metrics**  

| Metric | Description | 
| --- | --- | 
|  RulesExecuted  |  The number of AWS IoT rules executed\.  | 


**Rule Metrics**  

| Metric | Description | 
| --- | --- | 
|  TopicMatch  |  The number of incoming messages published on a topic on which a rule is listening\. The `RuleName` dimension contains the name of the rule\. | 
|  ParseError  |  The number of JSON parse errors that occurred in messages published on a topic on which a rule is listening\. The `RuleName` dimension contains the name of the rule\.  | 


**Rule Action Metrics**  

| Metric | Description | 
| --- | --- | 
|  Success  |  The number of successful rule action invocations\. The `RuleName` dimension contains the name of the rule that specifies the action\. The `ActionType` dimension contains the type of action that was invoked\.  | 
|  Failure  |  The number of failed rule action invocations\. The `RuleName` dimension contains the name of the rule that specifies the action\. The `RuleName` dimension contains the name of the rule that specifies the action\. The `ActionType` dimension contains the type of action that was invoked\.  | 


**Message Broker Metrics**  

| Metric | Description | 
| --- | --- | 
|  Connect\.AuthError  |  The number of connection requests that could not be authorized by the message broker\. The `Protocol` dimension contains the protocol used to send the `CONNECT`\. message\.  | 
|  Connect\.ClientError  |  The number of connection requests rejected because the MQTT message did not meet the requirements defined in [AWS IoT Limits](http://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `CONNECT`\. message\.  | 
|  Connect\.ServerError  |  The number of connection requests that failed because an internal error occurred\. The Protocol dimension contains the protocol used to send the `CONNECT` message\.  | 
|  Connect\.Success  |  The number of successful connections to the message broker\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\.  | 
|  Connect\.Throttle  |  The number of connection requests that were throttled because the client exceeded the allowed connect request rate\. The `Protocol` dimension contains the protocol used to send the `CONNECT` message\.  | 
|  Ping\.Success  |  The number of ping messages received by the message broker\. The `Protocol` dimension contains the protocol used to send the ping message\.  | 
|  PublishIn\.AuthError  |  The number of publish requests the message broker was unable to authorize\. The `Protocol` dimension contains the protocol used to publish the message\.  | 
|  PublishIn\.ClientError  |  The number of publish requests rejected by the message broker because the message did not meet the requirements defined in [AWS IoT Limits](http://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to publish the message\.  | 
|  PublishIn\.ServerError  |  The number of publish requests the message broker failed to process because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  PublishIn\.Success  |  The number of publish requests successfully processed by the message broker\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  PublishIn\.Throttle  |  The number of publish request that were throttled because the client exceeded the allowed inbound message rate\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  PublishOut\.AuthError  |  The number of publish requests made by the message broker that could not be authorized by AWS IoT\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  PublishOut\.ClientError  |  The number of publish requests made by the message broker that were rejected because the message did not meet the requirements defined in [AWS IoT Limits](http://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  PublishOut\.Success  |  The number of publish requests successfully made by the message broker\. The `Protocol` dimension contains the protocol used to send the `PUBLISH` message\.  | 
|  Subscribe\.AuthError  |  The number of subscription requests made by a client that could not be authorized\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  Subscribe\.ClientError  |  The number of subscribe requests that were rejected because the `SUBSCRIBE` message did not meet the requirements defined in [AWS IoT Limits](http://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  Subscribe\.ServerError  |  The number of subscribe requests that were rejected because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  Subscribe\.Success  |  The number of subscribe requests that were successfully processed by the message broker\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  Subscribe\.Throttle  |  The number of subscribe requests that were throttled because the client exceeded the allowed subscribe request rate\. The `Protocol` dimension contains the protocol used to send the `SUBSCRIBE` message\.  | 
|  Unsubscribe\.ClientError  |  The number of unsubscribe requests that were rejected because the `UNSUBSCRIBE` message did not meet the requirements defined in [AWS IoT Limits](http://docs.aws.amazon.com/iot/latest/developerguide/iot-limits.html)\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.  | 
|  Unsubscribe\.ServerError  |  The number of unsubscribe requests that were rejected because an internal error occurred\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.  | 
|  Unsubscribe\.Success  |  The number of unsubscribe requests that were successfully processed by the message broker\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.  | 
|  Unsubscribe\.Throttle  |  The number of unsubscribe requests that were rejected because the client exceeded the allowed unsubscribe request rate\. The `Protocol` dimension contains the protocol used to send the `UNSUBSCRIBE` message\.   | 

**Note**  
The message broker metrics are displayed in the AWS IoT console under **Protocol Metrics**\.


**Device Shadow Metrics**  

| Metric | Description | 
| --- | --- | 
|  DeleteThingShadow\.Accepted  |  The number of DeleteThingShadow requests processed successfully\. The `Protocol` dimension contains the protocol used to make the request\.  | 
|  GetThingShadow\.Accepted  |  The number of GetThingShadow requests processed successfully\. The `Protocol` dimension contains the protocol used to make the request\.  | 
|  UpdateThingShadow\.Accepted  |  The number of UpdateThingShadow requests processed successfully\. The `Protocol` dimension contains the protocol used to make the request\.  | 

**Note**  
The device shadow metrics are displayed in the AWS IoT console under **Protocol Metrics**\.


**Jobs Metrics**  

| Metric | Description | 
| --- | --- | 
|  ServerError  |  The number of server errors generated while executing the job\. The `JobId` dimension contains the ID of the job\.  | 
|  ClientError  |  The number of client errors generated while executing the job\. The `JobId` dimension contains the ID of the job\.  | 
|  QueuedJobExecutionTotalCount  |  The total number of job executions whose status is `QUEUED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  InProgressJobExecutionTotalCount  |  The total number of job executions whose status is `IN_PROGRESS` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  FailedJobExecutionTotalCount  |  The total number of job executions whose status is `FAILED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  SuccededJobExecutionTotalCount  |  The total number of job executions whose status is `SUCCESS` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  CanceledJobExecutionTotalCount  |  The total number of job executions whose status is `CANCELED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  RejectedJobExecutionTotalCount  |  The total number of job executions whose status is `REJECTED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  RemovedJobExecutionTotalCount  |  The total number of job executions whose status is `REMOVED` for the given job\. The `JobId` dimension contains the ID of the job\.  | 
|  QueuedJobExecutionCount  |  The number of job executions whose status has changed to `QUEUED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  InProgressJobExecutionCount  |  The number of job executions whose status has changed to `IN_PROGRESS` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  FailedJobExecutionCount  |  The number of job executions whose status has changed to `FAILED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  SuccededJobExecutionCount  |  The number of job executions whose status has changed to `SUCCESS` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  CanceledJobExecutionCount  |  The number of job executions whose status has changed to `CANCELED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  RejectedJobExecutionCount  |  The number of job executions whose status has changed to `REJECTED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 
|  RemovedJobExecutionCount  |  The number of job executions whose status has changed to `REMOVED` within a time period that is determined by CloudWatch\. \(For more information about CloudWatch metrics, see [Amazon CloudWatch Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)\.\) The `JobId` dimension contains the ID of the job\.  | 


**Device Defender Audit Metrics**  

| Metric | Description | 
| --- | --- | 
|  NonCompliantResources  |  The number of resources that were found to be non\-compliant with a check\. The system will report the number of resources which were out of compliance for each check of each audit performed\.   | 
|  ResourcesEvaluated  |  The number of resources that were evaluated for compliance\. The system will report the number of resources which were evaluated for each check of each audit performed\.   | 


**Device Defender Detect Metrics**  

| Metric | Description | 
| --- | --- | 
|  Violations   |  The number of new violations of security profile behaviors that have been found since the last time an evaluation was performed\. The system will report the number of new violations for the account, for a specific security profile, and for a specific behavior of a specific security profile\.   | 
|  ViolationsCleared   |  The number of violations of security profile behaviors that have resolved since the last time an evaluation was performed\. The system will report the number of resolved violations for the account, for a specific security profile, and for a specific behavior of a specific security profile\.   | 
|  ViolationsInvalidated   |  The number of violations of security profile behaviors for which information is no longer available since the last time an evaluation was performed \(because the reporting device stopped reporting, or is no longer being monitored for some reason\)\. The system will report the number of invalidated violations for the entire account, for a specific security profile, and for a specific behavior of a specific security profile\.   | 

## Dimensions for Metrics<a name="aws-iot-metricdimensions"></a>

Metrics use the namespace and provide metrics for the following dimension\(s\):


| Dimension | Description | 
| --- | --- | 
| ActionType |  The [action type](http://docs.aws.amazon.com/iot/latest/developerguide/iot-rule-actions.html) specified by the rule that triggered by the request\.  | 
|  Protocol  |  The protocol used to make the request\. Valid values are: MQTT or HTTP  | 
|  RuleName  |  The name of the rule triggered by the request\.  | 
|  JobId  |  The ID of the job whose progress or message connection success/failure is being monitored\.  | 
|  TaskType  |  The type of Device Defender Audit whose check results are being monitored\. One of "SCHEDULED\_AUDIT\_TASK" or "ON\_DEMAND\_AUDIT\_TASK"\.  | 
|  CheckName  |  The name of the Device Defender audit check whose results are being monitored\.  | 
|  ScheduledAuditName  |  The name of the Device Defender scheduled audit whose check results are being monitored\. This will have the value "OnDemand" if the results reported are for an audit that was performed on\-demand\.  | 
|  SecurityProfileName  |  The name of the Device Defender Detect security profile whose behaviors are being monitored\.  | 
|  BehaviorName  |  The name of the Device Defender Detect security profile behavior which is being monitored\.  | 