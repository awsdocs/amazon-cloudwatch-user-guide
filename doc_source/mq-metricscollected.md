# Amazon MQ Metrics<a name="mq-metricscollected"></a>

Amazon MQ and Amazon CloudWatch are integrated so you can use CloudWatch to view and analyze metrics for your ActiveMQ broker and the broker's destinations \(queues and topics\)\. You can view and analyze your Amazon MQ metrics from the CloudWatch console, the AWS CLI, or the CloudWatch CLI\. CloudWatch metrics for Amazon MQ are automatically polled from the broker and then pushed to CloudWatch every minute\. For more information, see [Monitoring Amazon MQ with CloudWatch](Amazon MQ Developer Guideamazon-mq-monitoring-cloudwatch.html) in the *Amazon MQ Developer Guide*\.

**Note**  
The following statistics are valid for all of the metrics:  
`Average`
`Minimum`
`Maximum`
`Sum`

The `AWS/AmazonMQ` namespace includes the following metrics\.

## Broker Metrics<a name="broker-metrics"></a>


| Metric | Unit | Description | 
| --- | --- | --- | 
| CpuCreditBalance | Credits \(vCPU\-minutes\) |   This metric is available only for the `mq.t2.micro` broker instance type\. CPU credit metrics are available only at five\-minute intervals\.  The number of earned CPU credits that an instance has accrued since it was launched or started \(including the number of launch credits\)\. The credit balance is available for the broker instance to spend on bursts beyond the baseline CPU utilization\. Credits are accrued in the credit balance after they're earned and removed from the credit balance after they're spent\. The credit balance has a maximum limit\. Once the limit is reached, any newly earned credits are discarded\.  | 
| CpuUtilization | Percent | The percentage of allocated EC2 compute units that the broker currently uses\. | 
| CurrentConnectionsCount | Count | The current number of active connections on the current broker\. | 
| HeapUsage | Percent | The percentage of the ActiveMQ JVM memory limit that the broker currently uses\. | 
| NetworkIn | Bytes | The volume of incoming traffic for the broker\. | 
| NetworkOut | Bytes | The volume of outgoing traffic for the broker\. | 
| TotalConsumerCount | Count | The number of message consumers subscribed to destinations on the current broker\. | 
| TotalMessageCount | Count | The number of messages stored on the broker\. | 
| TotalProducerCount | Count | The number of message producers active on destinations on the current broker\. | 

### Dimension for Broker Metrics<a name="broker-metrics-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
| Broker |  The name of the broker\. A single\-instance broker has the suffix `-1`\. An active\-standby broker for high availability has the suffixes `-1` and `-2` for its redundant pair\.  | 

## Destination \(Queue and Topic\) Metrics<a name="destination-queue-topic-metrics"></a>

**Important**  
The following metrics record only values since CloudWatch polled the metrics last:  
`EnqueueCount`
`ExpiredCount`
`DequeueCount`
`DispatchCount`


| Metric | Unit | Description | 
| --- | --- | --- | 
| ConsumerCount | Count | The number of consumers subscribed to the destination\. | 
| EnqueueCount | Count | The number of messages sent to the destination\. | 
| EnqueueTime | Time \(milliseconds\) | The amount of time it takes the broker to accept a message from the producer and send it to the destination\. | 
| ExpiredCount | Count | The number of messages that couldn't be delivered because they expired\. | 
| DispatchCount | Count | The number of messages sent to consumers\. | 
| DequeueCount | Count | The number of messages acknowledged by consumers\. | 
| MemoryUsage | Percent | The percentage of the memory limit that the destination currently uses\. | 
| ProducerCount | Count | The number of producers for the destination\. | 
| QueueSize | Count | The number of messages in the queue\. This metric applies only to queues\.  | 

### Dimensions for Destination \(Queue and Topic\) Metrics<a name="destination-queue-topic-dimensions"></a>


| Dimension | Description | 
| --- | --- | 
| Broker |  The name of the broker\. A single\-instance broker has the suffix `-1`\. An active\-standby broker for high availability has the suffixes `-1` and `-2` for its redundant pair\.  | 
| Topic or Queue | The name of the topic or queue\. | 