# Amazon SQS Metrics and Dimensions<a name="sqs-metricscollected"></a>

Amazon SQS sends data points to CloudWatch for several metrics\. All active queues automatically send five\-minute metrics to CloudWatch\. A queue stays active for six hours from the last activity \(for example, any API call\) on the queue\. For more information, see [Monitoring Amazon SQS with Amazon CloudWatch](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/MonitorSQSwithCloudWatch.html) in the *Amazon Simple Queue Service Developer Guide*\.

**Note**  
 Detailed monitoring, or one\-minute metrics, is currently unavailable for Amazon SQS\. Making requests to CloudWatch at this resolution might return no data\.

## Amazon SQS Metrics<a name="sqs-metrics"></a>

The `AWS/SQS` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| ApproximateAgeOfOldestMessage |  The approximate age of the oldest non\-deleted message in the queue\.  For dead\-letter queues, the value of `ApproximateAgeOfOldestMessage` is the longest time that a message has been in the queue\.  Units: *Seconds* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| ApproximateNumberOfMessagesDelayed |  The number of messages in the queue that are delayed and not available for reading immediately\. This can happen when the queue is configured as a delay queue or when a message has been sent with a delay parameter\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| ApproximateNumberOfMessagesNotVisible |  The number of messages that are "in flight\." Messages are considered in flight if they have been sent to a client but have not yet been deleted or have not yet reached the end of their visibility window\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| ApproximateNumberOfMessagesVisible |  The number of messages available for retrieval from the queue\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| NumberOfEmptyReceives |  The number of `ReceiveMessage` API calls that did not return a message\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| NumberOfMessagesDeleted |  The number of messages deleted from the queue\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\) Amazon SQS emits the `NumberOfMessagesDeleted` metric for every successful deletion operation that uses a valid [ receipt handle](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-queue-message-identifiers.html#receipt-handle), including duplicate deletions\. The following scenarios might cause the value of the `NumberOfMessagesDeleted` metric to be higher than expected: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/sqs-metricscollected.html)  | 
| NumberOfMessagesReceived |  The number of messages returned by calls to the `ReceiveMessage` action\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| `NumberOfMessagesSent` |  The number of messages added to a queue\. Units: *Count* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\)  | 
| `SentMessageSize` |  The size of messages added to a queue\.  Units: *Bytes* Valid Statistics: Average, Minimum, Maximum, Sum, Data Samples \(displays as Sample Count in the Amazon SQS console\) Note that `SentMessageSize` does not display as an available metric in the CloudWatch console until at least one message is sent to the corresponding queue\.  | 

## Dimensions for Amazon SQS Metrics<a name="sqs-metric-dimensions"></a>

The only dimension that Amazon SQS sends to CloudWatch is `QueueName`\. This means that all available statistics are filtered by `QueueName`\.