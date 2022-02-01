# Amazon Elastic Compute Cloud \(EC2\)<a name="appinsights-metrics-ec2"></a>

CloudWatch Application Insights supports the following metrics:

**Topics**
+ [CloudWatch built\-in metrics](#appinsights-metrics-ec2-built-in)
+ [CloudWatch agent metrics \(Windows server\)](#appinsights-metrics-ec2-windows)
+ [CloudWatch agent metrics \(Linux server\)](#appinsights-metrics-ec2-linux)

## CloudWatch built\-in metrics<a name="appinsights-metrics-ec2-built-in"></a>

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

## CloudWatch agent metrics \(Windows server\)<a name="appinsights-metrics-ec2-windows"></a>

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

APP\_POOL\_WAS Total Worker Process Ping Failures

ASP\.NET Application Restarts

ASP\.NET Applications % Managed Processor Time \(estimated\)

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

Bytes Received/sec

Normal Messages Queue Length/sec

Urgent Message Queue Length/sec

Reconnect Count

Unacknowledged Message Queue Length/sec

Messages Outstanding

Messages Sent/sec

Database Update Messages/sec

Update Messages/sec

Flushes/sec

Crypto Checkpoints Saved/sec

Crypto Checkpoints Restored/sec

Registry Checkpoints Restored/sec

Registry Checkpoints Saved/sec

Cluster API Calls/sec

Resource API Calls/sec

Cluster Handles/sec

Resource Handles/sec

## CloudWatch agent metrics \(Linux server\)<a name="appinsights-metrics-ec2-linux"></a>

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