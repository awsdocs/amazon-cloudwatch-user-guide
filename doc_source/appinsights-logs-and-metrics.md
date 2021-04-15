# Logs and metrics supported by Amazon CloudWatch Application Insights<a name="appinsights-logs-and-metrics"></a>

The following lists show the supported logs and metrics for Amazon CloudWatch Application Insights\. 

**CloudWatch Application Insights supports the following logs:**
+ Microsoft Internet Information Services \(IIS\) logs
+ Error log for SQL Server on EC2
+ Custom \.NET application logs, such as Log4Net
+ Windows Event logs, including Windows logs \(System, Application, and Security\) and Applications and Services log
+ Amazon CloudWatch Logs for AWS Lambda
+ Error log and slow log for RDS MySQL, Aurora MySQL, and MySQL on EC2
+ Postgresql log for PostgreSQL RDS and PostgreSQL on EC2
+ Amazon CloudWatch Logs for AWS Step Functions
+ Execution logs and access logs \(JSON, CSV, and XML, but not CLF\) for API Gateway REST API stages
+ Prometheus JMX exporter logs \(EMF\)
+ Alert logs and listener logs for Oracle on Amazon RDS and Oracle on Amazon EC2

**Contents**
+ [Amazon Elastic Compute Cloud \(EC2\)](#appinsights-metrics-ec2)
  + [CloudWatch built\-in metrics](#appinsights-metrics-ec2-built-in)
  + [CloudWatch Agent metrics \(Windows server\)](#appinsights-metrics-ec2-windows)
  + [CloudWatch Agent metrics \(Linux server\)](#appinsights-metrics-ec2-linux)
+ [Elastic Block Store \(EBS\)](#appinsights-metrics-ebs)
+ [Elastic Load Balancer \(ELB\)](#appinsights-metrics-elb)
+ [Application ELB](#appinsights-metrics-app-elb)
+ [Amazon EC2 Auto Scaling Groups](#appinsights-metrics-as)
+ [Amazon Simple Queue Server \(SQS\)](#appinsights-metrics-sqs)
+ [Amazon Relational Database Service \(RDS\)](#appinsights-metrics-rds)
  + [RDS Database instances](#appinsights-metrics-rds-instances)
  + [RDS Database clusters](#appinsights-metrics-rds-clusters)
+ [AWS Lambda Function](#appinsights-metrics-lambda)
+ [Amazon DynamoDB table](#appinsights-metrics-dyanamodb)
+ [Amazon S3 bucket](#appinsights-metrics-s3)
+ [AWS Step Functions](#appinsights-metrics-step-functions)
  + [Execution\-level](#appinsights-metrics-step-functions-execution)
  + [Activity](#appinsights-metrics-step-functions-activity)
  + [Lambda function](#appinsights-metrics-step-functions-lambda)
  + [Service integration](#appinsights-metrics-step-functions-service-integration)
  + [Step Functions API](#appinsights-metrics-step-functions-api)
+ [API Gateway REST API stages](#appinsights-metrics-api-gateway)
+ [Java](#appinsights-metrics-java)
+ [Metrics With datapoints requirements](#appinsights-metrics-datapoint-requirements)
  + [AWS/ApplicationELB](#appinsights-metrics-datapoint-requirements-app-elb)
  + [AWS/AutoScaling](#appinsights-metrics-datapoint-requirements-autoscaling)
  + [AWS/EC2](#appinsights-metrics-datapoint-requirements-ec2)
  + [Elastic Block Store \(EBS\)](#appinsights-metrics-datapoint-requirements-ebs)
  + [AWS/ELB](#appinsights-metrics-datapoint-requirements-elb)
  + [AWS/RDS](#appinsights-metrics-datapoint-requirements-rds)
  + [AWS/Lambda](#appinsights-metrics-datapoint-requirements-lambda)
  + [AWS/SQS](#appinsights-metrics-datapoint-requirements-sqs)
  + [AWS/CWAgent](#appinsights-metrics-datapoint-requirements-cwagent)
  + [AWS/DynamoDB](#appinsights-metrics-datapoint-requirements-dynamo)
  + [AWS/S3](#appinsights-metrics-datapoint-requirements-s3)
  + [AWS/States](#appinsights-metrics-datapoint-requirements-states)
  + [AWS/ApiGateway](#appinsights-metrics-datapoint-requirements-api-gateway)
+ [Recommended metrics](#application-insights-recommended-metrics)
+ [Performance Counter metrics](#application-insights-performance-counter)

## Amazon Elastic Compute Cloud \(EC2\)<a name="appinsights-metrics-ec2"></a>

**Topics**
+ [CloudWatch built\-in metrics](#appinsights-metrics-ec2-built-in)
+ [CloudWatch Agent metrics \(Windows server\)](#appinsights-metrics-ec2-windows)
+ [CloudWatch Agent metrics \(Linux server\)](#appinsights-metrics-ec2-linux)

### CloudWatch built\-in metrics<a name="appinsights-metrics-ec2-built-in"></a>

CPUCreditBalance

CPUCreditUsage

CPUSurplusCreditBalance

CPUSurplusCreditsCharged

CPUUtilization

DiskReadBytes

DiskReadOps

DiskWriteBytes

DiskWriteOps

EBSByteBalance%

EBSIOBalance%

EBSReadBytes

EBSReadOps

EBSWriteBytes

EBSWriteOps

NetworkIn

NetworkOut

NetworkPacketsIn

NetworkPacketsOut

StatusCheckFailed

StatusCheckFailed\_Instance

StatusCheckFailed\_System

### CloudWatch Agent metrics \(Windows server\)<a name="appinsights-metrics-ec2-windows"></a>

\.NET CLR Exceptions \# of Exceps Thrown

\.NET CLR Exceptions \# of Exceps Thrown/Sec

\.NET CLR Exceptions \# of Filters/sec

\.NET CLR Exceptions \# of Finallys/sec

\.NET CLR Exceptions Throw to Catch Depth/sec

\.NET CLR Interop \# of CCWs

\.NET CLR Interop \# of Stubs

\.NET CLR Interop \# of TLB exports/sec

\.NET CLR Interop \# of TLB imports/sec

\.NET CLR Interop \# of marshaling

\.NET CLR Jit % Time in Jit

\.NET CLR Jit Standard Jit Failures

\.NET CLR Loading % Time Loading

\.NET CLR Loading Rate of Load Failures

\.NET CLR LocksAndThreads Contention Rate/sec

\.NET CLR LocksAndThreads Queue Length/sec

\.NET CLR Memory \# Total Committed Bytes

\.NET CLR Memory % Time in GC

\.NET CLR Networking 4\.0\.0\.0 HttpWebRequest Average Queue Time

\.NET CLR Networking 4\.0\.0\.0 HttpWebRequests Aborted/sec

\.NET CLR Networking 4\.0\.0\.0 HttpWebRequests Failed/sec

\.NET CLR Networking 4\.0\.0\.0 HttpWebRequests Queued/sec

ASP\.NET Application Restarts

ASP\.NET Applications Errors Total/Sec

ASP\.NET Applications Errors Unhandled During Execution/sec

ASP\.NET Applications Requests in Application Queue

ASP\.NET Applications Requests/Sec

ASP\.NET Request Wait Time

ASP\.NET Requests Queued

HTTP Service Request Queues CurrentQueueSize

LogicalDisk % Free Space

Memory % Committed Bytes In Use

Memory Available Mbytes

Memory Pages/sec

Network Interface Bytes Total/sec

Paging File % Usage

PhysicalDisk % Disk Time

PhysicalDisk Avg\. Disk Queue Length

PhysicalDisk Avg\. Disk sec/Read

PhysicalDisk Avg\. Disk sec/Write

PhysicalDisk Disk Read Bytes/sec

PhysicalDisk Disk Reads/sec

PhysicalDisk Disk Write Bytes/sec

PhysicalDisk Disk Writes/sec

Processor % Idle Time

Processor % Interrupt Time

Processor % Processor Time

Processor % User Time

SQLServer:Access Methods Forwarded Records/sec

SQLServer:Access Methods Full Scans/sec

SQLServer:Access Methods Page Splits/sec

SQLServer:Buffer Manager Buffer cache hit ratio

SQLServer:Buffer Manager Page life expectancy

SQLServer:General Statistics Processes blocked

SQLServer:General Statistics User Connections

SQLServer:Latches Average Latch Wait Time \(ms\)

SQLServer:Locks Average Wait Time \(ms\)

SQLServer:Locks Lock Timeouts/sec

SQLServer:Locks Lock Waits/sec

SQLServer:Locks Number of Deadlocks/sec

SQLServer:Memory Manager Memory Grants Pending

SQLServer:SQL Statistics Batch Requests/sec

SQLServer:SQL Statistics SQL Compilations/sec

SQLServer:SQL Statistics SQL Re\-Compilations/sec

System Processor Queue Length

TCPv4 Connections Established

TCPv6 Connections Established

W3SVC\_W3WP File Cache Flushes

W3SVC\_W3WP File Cache Misses

W3SVC\_W3WP Requests/Sec

W3SVC\_W3WP URI Cache Flushes

W3SVC\_W3WP URI Cache Misses

Web Service Bytes Received/Sec

Web Service Bytes Sent/Sec

Web Service Connection attempts/sec

Web Service Current Connections

Web Service Get Requests/sec

Web Service Post Requests/sec

### CloudWatch Agent metrics \(Linux server\)<a name="appinsights-metrics-ec2-linux"></a>

cpu\_time\_active

cpu\_time\_guest

cpu\_time\_guest\_nice

cpu\_time\_idle

cpu\_time\_iowait

cpu\_time\_irq

cpu\_time\_nice

cpu\_time\_softirq

cpu\_time\_steal

cpu\_time\_system

cpu\_time\_user

cpu\_usage\_active

cpu\_usage\_guest

cpu\_usage\_guest\_nice

cpu\_usage\_idle

cpu\_usage\_iowait

cpu\_usage\_irq

cpu\_usage\_nice

cpu\_usage\_softirq

cpu\_usage\_steal

cpu\_usage\_system

cpu\_usage\_user

disk\_free

disk\_inodes\_free

disk\_inodes\_used

disk\_used

disk\_used\_percent

diskio\_io\_time

diskio\_iops\_in\_progress

diskio\_read\_bytes

diskio\_read\_time

diskio\_reads

diskio\_write\_bytes

diskio\_write\_time

diskio\_writes

mem\_active

mem\_available

mem\_available\_percent

mem\_buffered

mem\_cached

mem\_free

mem\_inactive

mem\_used

mem\_used\_percent

net\_bytes\_recv

net\_bytes\_sent

net\_drop\_in

net\_drop\_out

net\_err\_in

net\_err\_out

net\_packets\_recv

net\_packets\_sent

netstat\_tcp\_close

netstat\_tcp\_close\_wait

netstat\_tcp\_closing

netstat\_tcp\_established

netstat\_tcp\_fin\_wait1

netstat\_tcp\_fin\_wait2

netstat\_tcp\_last\_ack

netstat\_tcp\_listen

netstat\_tcp\_none

netstat\_tcp\_syn\_recv

netstat\_tcp\_syn\_sent

netstat\_tcp\_time\_wait

netstat\_udp\_socket

processes\_blocked

processes\_dead

processes\_idle

processes\_paging

processes\_running

processes\_sleeping

processes\_stopped

processes\_total

processes\_total\_threads

processes\_wait

processes\_zombies

swap\_free

swap\_used

swap\_used\_percent

## Elastic Block Store \(EBS\)<a name="appinsights-metrics-ebs"></a>

VolumeReadBytes

VolumeWriteBytes

VolumeReadOps

VolumeWriteOps

VolumeTotalReadTime 

VolumeTotalWriteTime 

VolumeIdleTime

VolumeQueueLength

VolumeThroughputPercentage

VolumeConsumedReadWriteOps

BurstBalance

## Elastic Load Balancer \(ELB\)<a name="appinsights-metrics-elb"></a>

EstimatedALBActiveConnectionCount

EstimatedALBConsumedLCUs

EstimatedALBNewConnectionCount

EstimatedProcessedBytes

HTTPCode\_Backend\_4XX

HTTPCode\_Backend\_5XX

HealthyHostCount

RequestCount

UnHealthyHostCount

## Application ELB<a name="appinsights-metrics-app-elb"></a>

EstimatedALBActiveConnectionCount

EstimatedALBConsumedLCUs

EstimatedALBNewConnectionCount

EstimatedProcessedBytes

HTTPCode\_Backend\_4XX

HTTPCode\_Backend\_5XX

HealthyHostCount

Latency

RequestCount

SurgeQueueLength

UnHealthyHostCount

## Amazon EC2 Auto Scaling Groups<a name="appinsights-metrics-as"></a>

CPUCreditBalance

CPUCreditUsage

CPUSurplusCreditBalance

CPUSurplusCreditsCharged

CPUUtilization

DiskReadBytes

DiskReadOps

DiskWriteBytes

DiskWriteOps

EBSByteBalance%

EBSIOBalance%

EBSReadBytes

EBSReadOps

EBSWriteBytes

EBSWriteOps

NetworkIn

NetworkOut

NetworkPacketsIn

NetworkPacketsOut

StatusCheckFailed

StatusCheckFailed\_Instance

StatusCheckFailed\_System

## Amazon Simple Queue Server \(SQS\)<a name="appinsights-metrics-sqs"></a>

ApproximateAgeOfOldestMessage

ApproximateNumberOfMessagesDelayed

ApproximateNumberOfMessagesNotVisible

ApproximateNumberOfMessagesVisible

NumberOfEmptyReceives

NumberOfMessagesDeleted

NumberOfMessagesReceived

NumberOfMessagesSent

## Amazon Relational Database Service \(RDS\)<a name="appinsights-metrics-rds"></a>

### RDS Database instances<a name="appinsights-metrics-rds-instances"></a>

BurstBalance

CPUCreditBalance

CPUUtilization

DatabaseConnections

DiskQueueDepth

FailedSQLServerAgentJobsCount

FreeStorageSpace

FreeableMemory

NetworkReceiveThroughput

NetworkTransmitThroughput

ReadIOPS

ReadLatency

ReadThroughput

WriteIOPS

WriteLatency

WriteThroughput

### RDS Database clusters<a name="appinsights-metrics-rds-clusters"></a>

ActiveTransactions

AuroraBinlogReplicaLag

AuroraReplicaLag

BackupRetentionPeriodStorageUsed

BinLogDiskUsage

BlockedTransactions

BufferCacheHitRatio

CPUUtilization

CommitLatency

CommitThroughput

DDLLatency

DDLThroughput

DMLLatency

DMLThroughput

DatabaseConnections

Deadlocks

DeleteLatency

DeleteThroughput

EngineUptime

FreeLocalStorage

FreeableMemory

InsertLatency

InsertThroughput

LoginFailures

NetworkReceiveThroughput

NetworkThroughput

NetworkTransmitThroughput

Queries

ResultSetCacheHitRatio

SelectLatency

SelectThroughput

SnapshotStorageUsed

TotalBackupStorageBilled

UpdateLatency

UpdateThroughput

VolumeBytesUsed

VolumeReadIOPs

VolumeWriteIOPs

## AWS Lambda Function<a name="appinsights-metrics-lambda"></a>

Errors

DeadLetterErrors

Duration

Throttles

IteratorAge

ProvisionedConcurrencySpilloverInvocations

## Amazon DynamoDB table<a name="appinsights-metrics-dyanamodb"></a>

SystemErrors

UserErrors

ConsumedReadCapacityUnits

ConsumedWriteCapacityUnits

ReadThrottleEvents

WriteThrottleEvents

TimeToLiveDeletedItemCount

ConditionalCheckFailedRequests

TransactionConflict

ReturnedRecordsCount

PendingReplicationCount

ReplicationLatency

## Amazon S3 bucket<a name="appinsights-metrics-s3"></a>

ReplicationLatency

BytesPendingReplication

OperationsPendingReplication

4xxErrors

5xxErrors

AllRequests

GetRequests

PutRequests

DeleteRequests

HeadRequests

PostRequests

SelectRequests

ListRequests

SelectScannedBytes

SelectReturnedBytes

FirstByteLatency

TotalRequestLatency

BytesDownloaded

BytesUploaded

## AWS Step Functions<a name="appinsights-metrics-step-functions"></a>

### Execution\-level<a name="appinsights-metrics-step-functions-execution"></a>

ExecutionTime

ExecutionThrottled

ExecutionsFailed

ExecutionsTimedOut

ExecutionsAborted

ExecutionsSucceeded

ExecutionsStarted

### Activity<a name="appinsights-metrics-step-functions-activity"></a>

ActivityRunTime

ActivityScheduleTime

ActivityTime

ActivitiesFailed

ActivitiesHeartbeatTimedOut

ActivitiesTimedOut

ActivitiesScheduled

ActivitiesSucceeded

ActivitiesStarted

### Lambda function<a name="appinsights-metrics-step-functions-lambda"></a>

LambdaFunctionRunTime

LambdaFunctionScheduleTime

LambdaFunctionTime

LambdaFunctionsFailed

LambdaFunctionsTimedOut

LambdaFunctionsScheduled

LambdaFunctionsSucceeded

LambdaFunctionsStarted

### Service integration<a name="appinsights-metrics-step-functions-service-integration"></a>

ServiceIntegrationRunTime

ServiceIntegrationScheduleTime

ServiceIntegrationTime

ServiceIntegrationsFailed

ServiceIntegrationsTimedOut

ServiceIntegrationsScheduled

ServiceIntegrationsSucceeded

ServiceIntegrationsStarted

### Step Functions API<a name="appinsights-metrics-step-functions-api"></a>

ThrottledEvents

ProvisionedBucketSize

ProvisionedRefillRate

ConsumedCapacity

## API Gateway REST API stages<a name="appinsights-metrics-api-gateway"></a>

4XXError

5XXError

IntegrationLatency

Latency

DataProcessed

CacheHitCount

CacheMissCount

## Java<a name="appinsights-metrics-java"></a>

java\_lang\_memory\_heapmemoryusage\_used

java\_lang\_memory\_heapmemoryusage\_committed

java\_lang\_operatingsystem\_openfiledescriptorcount

java\_lang\_operatingsystem\_maxfiledescriptorcount

java\_lang\_operatingsystem\_freephysicalmemorysize

java\_lang\_operatingsystem\_freeswapspacesize

java\_lang\_threading\_threadcount

java\_lang\_threading\_daemonthreadcount

java\_lang\_classloading\_loadedclasscount

java\_lang\_garbagecollector\_collectiontime\_copy

java\_lang\_garbagecollector\_collectiontime\_ps\_scavenge

java\_lang\_garbagecollector\_collectiontime\_parnew

java\_lang\_garbagecollector\_collectiontime\_marksweepcompact

java\_lang\_garbagecollector\_collectiontime\_ps\_marksweep

java\_lang\_garbagecollector\_collectiontime\_concurrentmarksweep

java\_lang\_garbagecollector\_collectiontime\_g1\_young\_generation

java\_lang\_garbagecollector\_collectiontime\_g1\_old\_generation

java\_lang\_garbagecollector\_collectiontime\_g1\_mixed\_generation

java\_lang\_operatingsystem\_committedvirtualmemorysize

## Metrics With datapoints requirements<a name="appinsights-metrics-datapoint-requirements"></a>

For metrics without an obvious default threshold to alarm on, Application Insights waits until the metric has enough data points to predict a reasonable threshold to alarm on\. The metric datapoints requirement that CloudWatch Application Insights checks before an alarm is created are: 
+ The metric has at least 100 datapoints from the past 15 to the past 2 days\.
+ The metric has at least 100 datapoints from the last day\.

The following metrics follow these datapoints requirements\. Note that CloudWatch agent metrics require up to one hour to create alarms\. 

**Topics**
+ [AWS/ApplicationELB](#appinsights-metrics-datapoint-requirements-app-elb)
+ [AWS/AutoScaling](#appinsights-metrics-datapoint-requirements-autoscaling)
+ [AWS/EC2](#appinsights-metrics-datapoint-requirements-ec2)
+ [Elastic Block Store \(EBS\)](#appinsights-metrics-datapoint-requirements-ebs)
+ [AWS/ELB](#appinsights-metrics-datapoint-requirements-elb)
+ [AWS/RDS](#appinsights-metrics-datapoint-requirements-rds)
+ [AWS/Lambda](#appinsights-metrics-datapoint-requirements-lambda)
+ [AWS/SQS](#appinsights-metrics-datapoint-requirements-sqs)
+ [AWS/CWAgent](#appinsights-metrics-datapoint-requirements-cwagent)
+ [AWS/DynamoDB](#appinsights-metrics-datapoint-requirements-dynamo)
+ [AWS/S3](#appinsights-metrics-datapoint-requirements-s3)
+ [AWS/States](#appinsights-metrics-datapoint-requirements-states)
+ [AWS/ApiGateway](#appinsights-metrics-datapoint-requirements-api-gateway)

### AWS/ApplicationELB<a name="appinsights-metrics-datapoint-requirements-app-elb"></a>

ActiveConnectionCount

ConsumedLCUs

HTTPCode\_ELB\_4XX\_Count

HTTPCode\_Target\_2XX\_Count

HTTPCode\_Target\_3XX\_Count

HTTPCode\_Target\_4XX\_Count

HTTPCode\_Target\_5XX\_Count

NewConnectionCount

ProcessedBytes

TargetResponseTime

UnHealthyHostCount

### AWS/AutoScaling<a name="appinsights-metrics-datapoint-requirements-autoscaling"></a>

GroupDesiredCapacity

GroupInServiceInstances

GroupMaxSize

GroupMinSize

GroupPendingInstances

GroupStandbyInstances

GroupTerminatingInstances

GroupTotalInstances

### AWS/EC2<a name="appinsights-metrics-datapoint-requirements-ec2"></a>

CPUCreditBalance

CPUCreditUsage

CPUSurplusCreditBalance

CPUSurplusCreditsCharged

CPUUtilization

DiskReadBytes

DiskReadOps

DiskWriteBytes

DiskWriteOps

EBSByteBalance%

EBSIOBalance%

EBSReadBytes

EBSReadOps

EBSWriteBytes

EBSWriteOps

NetworkIn

NetworkOut

NetworkPacketsIn

NetworkPacketsOut

### Elastic Block Store \(EBS\)<a name="appinsights-metrics-datapoint-requirements-ebs"></a>

VolumeReadBytes 

VolumeWriteBytes 

VolumeReadOps

VolumeWriteOps

VolumeTotalReadTime 

VolumeTotalWriteTime 

VolumeIdleTime

VolumeQueueLength

VolumeThroughputPercentage

VolumeConsumedReadWriteOps

BurstBalance

### AWS/ELB<a name="appinsights-metrics-datapoint-requirements-elb"></a>

EstimatedALBActiveConnectionCount

EstimatedALBConsumedLCUs

EstimatedALBNewConnectionCount

EstimatedProcessedBytes

HTTPCode\_Backend\_4XX

HTTPCode\_Backend\_5XX

HealthyHostCount

Latency

RequestCount

SurgeQueueLength

UnHealthyHostCount

### AWS/RDS<a name="appinsights-metrics-datapoint-requirements-rds"></a>

ActiveTransactions

AuroraBinlogReplicaLag

AuroraReplicaLag

BackupRetentionPeriodStorageUsed

BinLogDiskUsage

BlockedTransactions

CPUCreditBalance

CommitLatency

CommitThroughput

DDLLatency

DDLThroughput

DMLLatency

DMLThroughput

DatabaseConnections

Deadlocks

DeleteLatency

DeleteThroughput

DiskQueueDepth

EngineUptime

FreeLocalStorage

FreeStorageSpace

FreeableMemory

InsertLatency

InsertThroughput

LoginFailures

NetworkReceiveThroughput

NetworkThroughput

NetworkTransmitThroughput

Queries

ReadIOPS

ReadThroughput

SelectLatency

SelectThroughput

SnapshotStorageUsed

TotalBackupStorageBilled

UpdateLatency

UpdateThroughput

VolumeBytesUsed

VolumeReadIOPs

VolumeWriteIOPs

WriteIOPS

WriteThroughput

### AWS/Lambda<a name="appinsights-metrics-datapoint-requirements-lambda"></a>

Errors

DeadLetterErrors

Duration

Throttles

IteratorAge

ProvisionedConcurrencySpilloverInvocations

### AWS/SQS<a name="appinsights-metrics-datapoint-requirements-sqs"></a>

ApproximateAgeOfOldestMessage

ApproximateNumberOfMessagesDelayed

ApproximateNumberOfMessagesNotVisible

ApproximateNumberOfMessagesVisible

NumberOfEmptyReceives

NumberOfMessagesDeleted

NumberOfMessagesReceived

NumberOfMessagesSent

### AWS/CWAgent<a name="appinsights-metrics-datapoint-requirements-cwagent"></a>

LogicalDisk % Free Space

Memory % Committed Bytes In Use

Memory Available Mbytes

Network Interface Bytes Total/sec

Paging File % Usage

PhysicalDisk % Disk Time

PhysicalDisk Avg\. Disk sec/Read

PhysicalDisk Avg\. Disk sec/Write

PhysicalDisk Disk Read Bytes/sec

PhysicalDisk Disk Reads/sec

PhysicalDisk Disk Write Bytes/sec

PhysicalDisk Disk Writes/sec

Processor % Idle Time

Processor % Interrupt Time

Processor % Processor Time

Processor % User Time

SQLServer:Access Methods Forwarded Records/sec

SQLServer:Access Methods Page Splits/sec

SQLServer:Buffer Manager Buffer cache hit ratio

SQLServer:Buffer Manager Page life expectancy

SQLServer:Database Replica File Bytes Received/sec

SQLServer:Database Replica Log Bytes Received/sec

SQLServer:Database Replica Log remaining for undo

SQLServer:Database Replica Log Send Queue

SQLServer:Database Replica Mirrored Write Transaction/sec

SQLServer:Database Replica Recovery Queue

SQLServer:Database Replica Redo Bytes Remaining

SQLServer:Database Replica Redone Bytes/sec

SQLServer:Database Replica Total Log requiring undo

SQLServer:Database Replica Transaction Delay

SQLServer:General Statistics Processes blocked

SQLServer:SQL Statistics Batch Requests/sec

SQLServer:SQL Statistics SQL Compilations/sec

SQLServer:SQL Statistics SQL Re\-Compilations/sec

System Processor Queue Length

TCPv4 Connections Established

TCPv6 Connections Established

### AWS/DynamoDB<a name="appinsights-metrics-datapoint-requirements-dynamo"></a>

ConsumedReadCapacityUnits

ConsumedWriteCapacityUnits

ReadThrottleEvents

WriteThrottleEvents

TimeToLiveDeletedItemCount

ConditionalCheckFailedRequests

TransactionConflict

ReturnedRecordsCount

PendingReplicationCount

ReplicationLatency

### AWS/S3<a name="appinsights-metrics-datapoint-requirements-s3"></a>

ReplicationLatency

BytesPendingReplication

OperationsPendingReplication

4xxErrors

5xxErrors

AllRequests

GetRequests

PutRequests

DeleteRequests

HeadRequests

PostRequests

SelectRequests

ListRequests

SelectScannedBytes

SelectReturnedBytes

FirstByteLatency

TotalRequestLatency

BytesDownloaded

BytesUploaded

### AWS/States<a name="appinsights-metrics-datapoint-requirements-states"></a>

ActivitiesScheduled

ActivitiesStarted

ActivitiesSucceeded

ActivityScheduleTime

ActivityRuntime

ActivityTime

LambdaFunctionsScheduled

LambdaFunctionsStarted

LambdaFunctionsSucceeded

LambdaFunctionScheduleTime

LambdaFunctionRuntime

LambdaFunctionTime

ServiceIntegrationsScheduled

ServiceIntegrationsStarted

ServiceIntegrationsSucceeded

ServiceIntegrationScheduleTime

ServiceIntegrationRuntime

ServiceIntegrationTime

ProvisionedRefillRate

ProvisionedBucketSize

ConsumedCapacity

ThrottledEvents

### AWS/ApiGateway<a name="appinsights-metrics-datapoint-requirements-api-gateway"></a>

4XXError 

IntegrationLatency

Latency

DataProcessed

CacheHitCount

CacheMissCount

## Recommended metrics<a name="application-insights-recommended-metrics"></a>

The following table lists the recommended metrics for each component type\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-logs-and-metrics.html)

## Performance Counter metrics<a name="application-insights-performance-counter"></a>

Performance Counter metrics are recommended for instances only when the corresponding Performance Counter sets are installed on the Windows instances\.


| Performance Counter metric name | Performance Counter set name | 
| --- | --- | 
| \.NET CLR Exceptions \# of Exceps Thrown | \.NET CLR Exceptions | 
| \.NET CLR Exceptions \# of Exceps Thrown/Sec  | \.NET CLR Exceptions | 
| \.NET CLR Exceptions \# of Filters/Sec  | \.NET CLR Exceptions | 
| \.NET CLR Exceptions \# of Finallys/Sec  | \.NET CLR Exceptions | 
| \.NET CLR Exceptions Throw to Catch Depth/Sec  | \.NET CLR Exceptions | 
| \.NET CLR Interop \# of CCWs  | \.NET CLR Interop  | 
| \.NET CLR Interop \# of Stubs  | \.NET CLR Interop  | 
| \.NET CLR Interop \# of TLB exports/Sec  | \.NET CLR Interop  | 
| \.NET CLR Interop \# of TLB imports/Sec  | \.NET CLR Interop  | 
| \.NET CLR Interop \# of Marshaling  | \.NET CLR Interop  | 
| \.NET CLR Jit % Time in Jit  | \.NET CLR Jit  | 
| \.NET CLR Jit Standard Jit Failures  | \.NET CLR Jit  | 
| \.NET CLR Loading % Time Loading  | \.NET CLR Loading  | 
| \.NET CLR Loading Rate of Load Failures  | \.NET CLR Loading  | 
| \.NET CLR LocksAndThreads Contention Rate/Sec  | \.NET CLR LocksAndThreads  | 
| \.NET CLR LocksAndThreads Queue Length/Sec  | \.NET CLR LocksAndThreads  | 
| \.NET CLR Memory \# Total Committed Bytes  | \.NET CLR Memory | 
| \.NET CLR Memory % Time in GC | \.NET CLR Memory | 
| \.NET CLR Networking 4\.0\.0\.0 HttpWebRequest Average Queue Time  | \.NET CLR Networking 4\.0\.0\.0  | 
| \.NET CLR Networking 4\.0\.0\.0 HttpWebRequests Aborted/Sec  | \.NET CLR Networking 4\.0\.0\.0  | 
| \.NET CLR Networking 4\.0\.0\.0 HttpWebRequests Failed/Sec  | \.NET CLR Networking 4\.0\.0\.0  | 
| \.NET CLR Networking 4\.0\.0\.0 HttpWebRequests Queued/Sec  | \.NET CLR Networking 4\.0\.0\.0  | 
| ASP\.NET Application Restarts  | ASP\.NET  | 
| ASP\.NET Applications Errors Total/Sec | ASP\.NET Applications | 
|  ASP\.NET Applications Errors Unhandled During Execution/Sec  |  ASP\.NET Applications  | 
|  ASP\.NET Applications Requests in Application Queue  |  ASP\.NET Applications  | 
|  ASP\.NET Applications Requests/Sec  |  ASP\.NET Applications  | 
| ASP\.NET Request Wait Time  | ASP\.NET  | 
| ASP\.NET Requests Queued  | ASP\.NET  | 
| HTTP Service Request Queues CurrentQueueSize  | HTTP Service Request Queues  | 
|  LogicalDisk % Free Space  |  LogicalDisk  | 
|  Memory % Committed Bytes In Use  | Memory | 
|  Memory Available Mbytes  |  Memory  | 
| Memory Pages/Sec  |  Memory  | 
|  Network Interface Bytes Total/Sec  | Network Interface | 
|  Paging File % Usage  |  Paging File  | 
| PhysicalDisk % Disk Time | PhysicalDisk | 
| PhysicalDisk Avg\. Disk Queue Length | PhysicalDisk | 
| PhysicalDisk Avg\. Disk Sec/Read | PhysicalDisk | 
| PhysicalDisk Avg\. Disk Sec/Write | PhysicalDisk | 
| PhysicalDisk Disk Read Bytes/Sec | PhysicalDisk | 
| PhysicalDisk Disk Reads/Sec | PhysicalDisk | 
| PhysicalDisk Disk Write Bytes/Sec | PhysicalDisk | 
| PhysicalDisk Disk Writes/Sec | PhysicalDisk | 
|  Processor % Idle Time  | Processor | 
| Processor % Interrupt Time |  Processor  | 
| Processor % Processor Time |  Processor  | 
| Processor % User Time |  Processor  | 
| SQLServer:Access Methods Forwarded Records/Sec |  SQLServer:Access Methods  | 
| SQLServer:Access Methods Full Scans/Sec |  SQLServer:Access Methods  | 
| SQLServer:Access Methods Page Splits/Sec |  SQLServer:Access Methods  | 
| SQLServer:Buffer Manager Buffer cache hit Ratio |  SQLServer:Buffer Manager  | 
| SQLServer:Buffer Manager Page life Expectancy |  SQLServer:Buffer Manager  | 
| SQLServer:Database Replica File Bytes Received/sec | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Log Bytes Received/sec | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Log remaining for undo | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Log Send Queue | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Mirrored Write Transaction/sec | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Recovery Queue | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Redo Bytes Remaining | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Redone Bytes/sec | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Total Log requiring undo | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:Database Replica Transaction Delay | SQL\_SERVER\_ALWAYSON\_AG | 
| SQLServer:General Statistics Processes Blocked |  SQLServer:General Statistics  | 
| SQLServer:General Statistics User Connections | SQLServer:General Statistics | 
| SQLServer:Latches Average Latch Wait Time \(ms\)  | SQLServer:Latches  | 
| SQLServer:Locks Average Wait Time \(ms\)  | SQLServer:Locks  | 
| SQLServer:Locks Lock Timeouts/Sec  | SQLServer:Locks  | 
| SQLServer:Locks Lock Waits/Sec  | SQLServer:Locks  | 
| SQLServer:Locks Number of Deadlocks/Sec  | SQLServer:Locks  | 
| SQLServer:Memory Manager Memory Grants Pending  | SQLServer:Memory Manager  | 
| SQLServer:SQL Statistics Batch Requests/Sec | SQLServer:SQL Statistics | 
| SQLServer:SQL Statistics SQL Compilations/Sec | SQLServer:SQL Statistics | 
| SQLServer:SQL Statistics SQL Re\-Compilations/Sec |  SQLServer:SQL Statistics  | 
| System Processor Queue Length |  System  | 
| TCPv4 Connections Established | TCPv4 | 
| TCPv6 Connections Established | TCPv6 | 
| W3SVC\_W3WP File Cache Flushes  | W3SVC\_W3WP  | 
| W3SVC\_W3WP File Cache Misses  | W3SVC\_W3WP  | 
| W3SVC\_W3WP Requests/Sec  | W3SVC\_W3WP  | 
| W3SVC\_W3WP URI Cache Flushes  | W3SVC\_W3WP  | 
| W3SVC\_W3WP URI Cache Misses  | W3SVC\_W3WP  | 
| Web Service Bytes Received/Sec |  Web Service  | 
| Web Service Bytes Sent/Sec |  Web Service  | 
|  Web Service Connection Attempts/Sec   |  Web Service  | 
| Web Service Current Connections  |  Web Service  | 
| Web Service Get Requests/Sec  |  Web Service  | 
| Web Service Post Requests/Sec  |  Web Service  | 