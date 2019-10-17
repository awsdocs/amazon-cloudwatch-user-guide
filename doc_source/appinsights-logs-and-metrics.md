# Logs and Metrics Supported by Amazon CloudWatch Application Insights for \.NET and SQL Server<a name="appinsights-logs-and-metrics"></a>

The following lists show the supported logs and metrics for CloudWatch Application Insights for \.NET and SQL Server\. 

**CloudWatch Application Insights for \.NET and SQL Server supports the following logs:**
+ Microsoft Internet Information Services \(IIS\) logs
+ Error log for SQL Server on EC2
+ Custom \.NET application logs, such as Log4Net
+ Windows Event Viewer, including Windows logs \(System, Application, and Security\) and Applications and Services logs

**Topics**
+ [Amazon Elastic Cloud Compute \(EC2\)](#appinsights-metrics-ec2)
+ [Elastic Load Balancer \(ELB\)](#appinsights-metrics-elb)
+ [Application ELB](#appinsights-metrics-app-elb)
+ [Amazon EC2 Auto Scaling Groups](#appinsights-metrics-as)
+ [Amazon Simple Queue Server \(SQS\)](#appinsights-metrics-sqs)
+ [Amazon Relational Database Service \(RDS\)](#appinsights-metrics-sqs)
+ [Metrics With Datapoints Requirements](#appinsights-metrics-datapoint-requirements)
+ [Recommended Metrics](#application-insights-recommended-metrics)
+ [Performance Counter Metrics](#application-insights-performance-counter)

## Amazon Elastic Cloud Compute \(EC2\)<a name="appinsights-metrics-ec2"></a>

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

LogicalDisk % Free Space

Memory % Committed Bytes In Use

Memory Available Mbytes

NetworkIn

Network Interface Bytes Total/sec

NetworkOut

NetworkPacketsIn

NetworkPacketsOut

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

SQLServer:General Statistics Processes blocked

SQLServer:SQL Statistics Batch Requests/sec

SQLServer:SQL Statistics SQL Compilations/sec

SQLServer:SQL Statistics SQL Re‐Compilations/sec

StatusCheckFailed

StatusCheckFailed\_Instance

StatusCheckFailed\_System

System Processor Queue Length

TCPv4 Connections Established

TCPv6 Connections Established

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

## Amazon Relational Database Service \(RDS\)<a name="appinsights-metrics-sqs"></a>

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

## Metrics With Datapoints Requirements<a name="appinsights-metrics-datapoint-requirements"></a>

For metrics without an obvious default threshold to alarm on, Application Insights waits until the metric has enough data points to predict a reasonable threshold to alarm on\. The metric datapoints requirement that Application Insights checks before an alarm is created are: 
+ The metric has at least 100 datapoints from the past 15 to the past 2 days\.
+ The metric has at least 100 datapoints from the last day\.

The following metrics follow these datapoint requirements\. Note that CloudWatch agent metrics require up to one hour to create alarms\. 

**AWS/ApplicationELB**

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

**AWS/AutoScaling**

GroupDesiredCapacity

GroupInServiceInstances

GroupMaxSize

GroupMinSize

GroupPendingInstances

GroupStandbyInstances

GroupTerminatingInstances

GroupTotalInstances

**AWS/EC2**

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

**AWS/ELB**

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

**AWS/RDS**

CPUCreditBalance

DatabaseConnections

DiskQueueDepth

FreeStorageSpace

FreeableMemory

NetworkReceiveThroughput

NetworkTransmitThroughput

ReadIOPS

ReadThroughput

WriteIOPS

WriteThroughput

**AWS/SQS**

ApproximateAgeOfOldestMessage

ApproximateNumberOfMessagesDelayed

ApproximateNumberOfMessagesNotVisible

ApproximateNumberOfMessagesVisible

NumberOfEmptyReceives

NumberOfMessagesDeleted

NumberOfMessagesReceived

NumberOfMessagesSent

**CWAgent**

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

SQLServer:General Statistics Processes blocked

SQLServer:SQL Statistics Batch Requests/sec

SQLServer:SQL Statistics SQL Compilations/sec

SQLServer:SQL Statistics SQL Re\-Compilations/sec

System Processor Queue Length

TCPv4 Connections Established

TCPv6 Connections Established

## Recommended Metrics<a name="application-insights-recommended-metrics"></a>

The following table lists the recommended metrics for each component type\.


| Component Type | Workload Type | Recommended Metric  | 
| --- | --- | --- | 
|  EC2 Instance  |   Web/\.NET Worker  |  CPUUtilization StatusCheckFailed Processor % Processor Time Memory % Committed Bytes In Use Memory Available Mbytes  | 
|  |  SQL Server  |  CPUUtilization StatusCheckFailed Processor % Processor Time Memory % Committed Bytes In Use Memory Available Mbytes Paging File % Usage System Processor Queue Length Network Interface Bytes Total/sec  | 
|  Classic ELB  |  Any  |  HTTPCode\_Backend\_4XX HTTPCode\_Backend\_5XX Latency SurgeQueueLength UnHealthyHostCount  | 
|  Application ELB  |  Any  |  HTTPCode\_Target\_4XX\_Count HTTPCode\_Target\_5XX\_Count TargetResponseTime UnHealthyHostCount  | 
|  RDS Instance  |  Any  |  CPUUtilization ReadLatency WriteLatency BurstBalence FailedSQLServerAgentJobsCount  | 
|  SQS Queue  |  Any  |  ApproximateAgeOfOldestMessage ApproximateNumberOfMessagesVisible NumberOfMessagesSent  | 

## Performance Counter Metrics<a name="application-insights-performance-counter"></a>

The following table lists the Performance Counter metrics and corresponding Performance Counter set names for EC2 Windows instances\.


| Performance Counter Metric Name | Performance Counter Set Name | 
| --- | --- | 
| \.NET CLR Exceptions \# of Exceps Thrown | \.NET CLR Exceptions | 
| \.NET CLR Memory % Time in GC | \.NET CLR Memory | 
| ASP\.NET Applications Errors Total/Sec | ASP\.NET Applications | 
|  ASP\.NET Applications Errors Unhandled During Execution/sec  |  ASP\.NET Applications  | 
|  ASP\.NET Applications Requests in Application Queue  |  ASP\.NET Applications  | 
|  ASP\.NET Applications Requests/Sec  |  ASP\.NET Applications  | 
|  LogicalDisk % Free Space  |  LogicalDisk  | 
|  Memory % Committed Bytes In Use  | Memory | 
|  Memory Available Mbytes  |  Memory  | 
|  Network Interface Bytes Total/sec  | Network Interface | 
|  Paging File % Usage  |  Paging File  | 
| PhysicalDisk % Disk Time | PhysicalDisk | 
| PhysicalDisk Avg\. Disk sec/Read | PhysicalDisk | 
| PhysicalDisk Avg\. Disk sec/Write | PhysicalDisk | 
| PhysicalDisk Disk Read Bytes/sec | PhysicalDisk | 
| PhysicalDisk Disk Reads/sec | PhysicalDisk | 
| PhysicalDisk Disk Write Bytes/sec | PhysicalDisk | 
| PhysicalDisk Disk Writes/sec | PhysicalDisk | 
|  Processor % Idle Time  | Processor | 
| Processor % Interrupt Time |  Processor  | 
| Processor % Processor Time |  Processor  | 
| Processor % User Time |  Processor  | 
| SQLServer:Access Methods Forwarded Records/sec |  SQLServer:Access Methods  | 
| SQLServer:Access Methods Page Splits/sec |  SQLServer:Access Methods  | 
| SQLServer:Buffer Manager Buffer cache hit ratio |  SQLServer:Buffer Manager  | 
| SQLServer:Buffer Manager Page life expectancy |  SQLServer:Buffer Manager  | 
| SQLServer:General Statistics Processes blocked |  SQLServer:General Statistics  | 
| SQLServer:SQL Statistics Batch Requests/sec | SQLServer:SQL Statistics | 
| SQLServer:SQL Statistics SQL Compilations/sec | SQLServer:SQL Statistics | 
| SQLServer:SQL Statistics SQL Re\-Compilations/sec |  SQLServer:SQL Statistics  | 
| System Processor Queue Length |  System  | 
| TCPv4 Connections Established | TCPv4 | 
| TCPv6 Connections Established | TCPv6 | 
| Web Service Bytes Received/Sec |  Web Service  | 
| Web Service Bytes Sent/Sec |  Web Service  | 
| Web Service Current Connections  |  Web Service  | 