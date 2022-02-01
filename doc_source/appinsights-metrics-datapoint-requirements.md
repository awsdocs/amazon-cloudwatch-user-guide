# Metrics with data points requirements<a name="appinsights-metrics-datapoint-requirements"></a>

For metrics without an obvious default threshold to alarm on, Application Insights waits until the metric has enough data points to predict a reasonable threshold to alarm on\. The metric data points requirement that CloudWatch Application Insights checks before an alarm is created are: 
+ The metric has at least 100 data points from the past 15 to the past 2 days\.
+ The metric has at least 100 data points from the last day\.

The following metrics follow these data points requirements\. Note that CloudWatch agent metrics require up to one hour to create alarms\. 

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
+ [AWS/SNS](#appinsights-metrics-datapoint-requirements-sns)

## AWS/ApplicationELB<a name="appinsights-metrics-datapoint-requirements-app-elb"></a>

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

## AWS/AutoScaling<a name="appinsights-metrics-datapoint-requirements-autoscaling"></a>

GroupDesiredCapacity

GroupInServiceInstances

GroupMaxSize

GroupMinSize

GroupPendingInstances

GroupStandbyInstances

GroupTerminatingInstances

GroupTotalInstances

## AWS/EC2<a name="appinsights-metrics-datapoint-requirements-ec2"></a>

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

## Elastic Block Store \(EBS\)<a name="appinsights-metrics-datapoint-requirements-ebs"></a>

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

## AWS/ELB<a name="appinsights-metrics-datapoint-requirements-elb"></a>

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

## AWS/RDS<a name="appinsights-metrics-datapoint-requirements-rds"></a>

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

## AWS/Lambda<a name="appinsights-metrics-datapoint-requirements-lambda"></a>

Errors

DeadLetterErrors

Duration

Throttles

IteratorAge

ProvisionedConcurrencySpilloverInvocations

## AWS/SQS<a name="appinsights-metrics-datapoint-requirements-sqs"></a>

ApproximateAgeOfOldestMessage

ApproximateNumberOfMessagesDelayed

ApproximateNumberOfMessagesNotVisible

ApproximateNumberOfMessagesVisible

NumberOfEmptyReceives

NumberOfMessagesDeleted

NumberOfMessagesReceived

NumberOfMessagesSent

## AWS/CWAgent<a name="appinsights-metrics-datapoint-requirements-cwagent"></a>

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

## AWS/DynamoDB<a name="appinsights-metrics-datapoint-requirements-dynamo"></a>

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

## AWS/S3<a name="appinsights-metrics-datapoint-requirements-s3"></a>

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

## AWS/States<a name="appinsights-metrics-datapoint-requirements-states"></a>

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

## AWS/ApiGateway<a name="appinsights-metrics-datapoint-requirements-api-gateway"></a>

4XXError 

IntegrationLatency

Latency

DataProcessed

CacheHitCount

CacheMissCount

## AWS/SNS<a name="appinsights-metrics-datapoint-requirements-sns"></a>

NumberOfNotificationsDelivered

NumberOfMessagesPublished

NumberOfNotificationsFailed

NumberOfNotificationsFilteredOut

NumberOfNotificationsFilteredOut\-InvalidAttributes

NumberOfNotificationsFilteredOut\-NoMessageAttributes

NumberOfNotificationsRedrivenToDlq

NumberOfNotificationsFailedToRedriveToDlq

SMSSuccessRate