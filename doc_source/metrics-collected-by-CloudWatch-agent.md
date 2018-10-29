# Metrics Collected by the CloudWatch Agent<a name="metrics-collected-by-CloudWatch-agent"></a>

You can collect metrics from servers by installing the CloudWatch agent on the server\. You can install the agent on both Amazon EC2 instances and on\-premises servers, and on servers running either Linux or Windows Server\. If you install the agent on an Amazon EC2 instance, the metrics it collects are in addition to the metrics enabled by default on Amazon EC2 instances\.

For information about installing the CloudWatch agent on an instance, see [Collect Metrics and Logs from Amazon EC2 Instances and On\-Premises Servers with the CloudWatch Agent](Install-CloudWatch-Agent.md)\.

## Metrics Collected by the CloudWatch Agent on Windows Server Instances<a name="windows-metrics-enabled-by-CloudWatch-agent"></a>

On a server running Windows Server, installing the CloudWatch agent enables you to collect the metrics associated with the counters in Windows Performance Monitor\. The CloudWatch metric names for these counters are created by putting a space between the object name and the counter name\. For example, the `% Interrupt Time` counter of the `Processor` object is given the metric name `Processor % Interrupt Time` in CloudWatch\. For more information about Windows Performance Monitor counters, see the Microsoft Windows Server documentation\.

The default namespace for metrics collected by the CloudWatch agent is `CWAgent`, although you can specify a different namespace when you configure the agent\.

## Metrics Collected by the CloudWatch Agent on Linux Instances<a name="linux-metrics-enabled-by-CloudWatch-agent"></a>

The metrics that you can collect with the CloudWatch agent on Linux instances are listed in the following table\.


| Metric | Description | 
| --- | --- | 
|  cpu\_time\_active |  The amount of time that the CPU is active in any capacity\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_guest |  The amount of time that the CPU is running a virtual CPU for a guest operating system\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_guest\_nice |  The amount of time that the CPU is running a virtual CPU for a guest operating system which is low\-priority and can be interrupted by other processes\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_idle |  The amount of time that the CPU is idle\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_iowait |  The amount of time that the CPU is waiting for I/O operations to complete\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_irq |  The amount of time that the CPU is servicing interrupts\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_nice |  The amount of time that the CPU is in user mode with low\-priority processes which can easily be interrupted by higher\-priority processes\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_softirq |  The amount of time that the CPU is servicing software interrupts\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_steal |  The amount of time that the CPU is in *stolen time*, which is time spent in other operating systems in a virtualized environment\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_system |  The amount of time that the CPU is in system mode\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_time\_user |  The amount of time that the CPU is in user mode\. This metric is measured in hundredths of a second\. Unit: None  | 
|  cpu\_usage\_active |  The percentage of time that the CPU is active in any capacity\. Unit: Percent  | 
|  cpu\_usage\_guest |  The percentage of time that the CPU is running a virtual CPU for a guest operating system\. Unit: Percent  | 
|  cpu\_usage\_guest\_nice |  The percentage of time that the CPU is running a virtual CPU for a guest operating system which is low\-priority and can be interrupted by other processes\. Unit: Percent  | 
|  cpu\_usage\_idle |  The percentage of time that the CPU is idle\. Unit: Percent  | 
|  cpu\_usage\_iowait |  The percentage of time that the CPU is waiting for I/O operations to complete\. Unit: Percent  | 
|  cpu\_usage\_irq |  The percentage of time that the CPU is servicing interrupts\. Unit: Percent  | 
|  cpu\_usage\_nice |  The percentage of time that the CPU is in user mode with low\-priority processes which can easily be interrupted by higher\-priority processes\. Unit: Percent  | 
|  cpu\_usage\_softirq |  The percentage of time that the CPU is servicing software interrupts\. Unit: Percent  | 
|  cpu\_usage\_steal |  The percentage of time that the CPU is in *stolen time*, which is time spent in other operating systems in a virtualized environment\. Unit: Percent  | 
|  cpu\_usage\_system |  The percentage of time that the CPU is in system mode\. Unit: Percent  | 
|  cpu\_usage\_user |  The percentage of time that the CPU is in user mode\. Unit: Percent  | 
|  disk\_free |  Free space on the disks\. Unit: Bytes  | 
|  disk\_inodes\_free |  The number of available index nodes on the disk\. Unit: Count  | 
|  disk\_inodes\_total |  The total number of index nodes reserved on the disk\. Unit: Count  | 
|  disk\_inodes\_used |  The number of used index nodes on the disk\. Unit: Count  | 
|  disk\_total |  Total space on the disks, including used and free\. Unit: Bytes  | 
|  disk\_used |  Used space on the disks\. Unit: Bytes  | 
|  disk\_used\_percent |  The percentage of total disk space that is used\. Unit: Percent  | 
|  diskio\_iops\_in\_progress |  The number of I/O requests that have been issued to the device driver but have not yet completed\. Unit: Count  | 
|  diskio\_io\_time |  The amount of time that the disk has had I/O requests queued\. Unit: Milliseconds  | 
|  diskio\_reads |  The number of disk read operations\. Unit: Count  | 
|  diskio\_read\_bytes |  The number of bytes read from the disks\. Unit: Bytes  | 
|  diskio\_read\_time |  The amount of time that read requests have waited on the disks\. Multiple read requests waiting at the same time all increase the number\. For example, if 5 requests all wait for an average of 100 milliseconds, then 500 is reported\. Unit: Milliseconds  | 
|  diskio\_writes |  The number disk write operations\. Unit: Count  | 
|  diskio\_write\_bytes |  The number of bytes written to the disks\. Unit: Bytes  | 
|  diskio\_write\_time |  The amount of time that write requests have waited on the disks\. Multiple write requests waiting at the same time all increase the number\. For example, if 8 requests all wait for an average of 1000 milliseconds, then 8000 is reported\. Unit: Milliseconds  | 
|  mem\_active |  The amount of memory that has been used in some way during the last sample period\. Unit: Bytes  | 
|  mem\_available |  The amount of memory that is available and can be given instantly to processes\. Unit: Bytes  | 
|  mem\_available\_percent |  The percentage of memory that is available and can be given instantly to processes\. Unit: Percent  | 
|  mem\_buffered |  The amount of memory that is being used for buffers\. Unit: Bytes  | 
|  mem\_cached |  The amount of memory that is being used for file caches\. Unit: Bytes  | 
|  mem\_free |  The amount of memory that is not being used\. Unit: Bytes  | 
|  mem\_inactive |  The amount of memory that has not been used in some way during the last sample period Unit: Bytes  | 
|  mem\_total |  The total amount of memory\. Unit: Bytes  | 
|  mem\_used |  The amount of memory currently in use\. Unit: Bytes  | 
|  mem\_used\_percent |  The percentage of memory currently in use\. Unit: Percent  | 
|  net\_bytes\_recv |  The number of bytes received by the network interface\. Unit: Bytes  | 
|  net\_bytes\_sent |  The number of bytes sent by the network interface\. Unit: Bytes  | 
|  net\_drop\_in |  The number of packets received by this network interface which were dropped\. Unit: Count  | 
|  net\_drop\_out |  The number of packets transmitted by this network interface which were dropped\. Unit: Count  | 
|  net\_err\_in |  The number of receive errors detected by this network interface\. Unit: Count  | 
|  net\_err\_out |  The number of transmit errors detected by this network interface\. Unit: Count  | 
|  net\_packets\_sent |  The number of packets sent by this network interface\. Unit: Count  | 
|  net\_packets\_recv |  The number of packets received by this network interface\. Unit: Count  | 
|  netstat\_tcp\_close |  The number of TCP connections with no state\. Unit: Count  | 
|  netstat\_tcp\_close\_wait |  The number of TCP connections waiting for a termination request from the client\. Unit: Count  | 
|  netstat\_tcp\_closing |  The number of TCP connections that are waiting for a termination request with acknowledgement from the client\. Unit: Count  | 
|  netstat\_tcp\_established |  The number of TCP connections established\. Unit: Count  | 
|  netstat\_tcp\_fin\_wait1 |  The number of TCP connections in the FIN\_WAIT1 state, during the process of closing a connection\. Unit: Count  | 
|  netstat\_tcp\_fin\_wait2 |  The number of TCP connections in the FIN\_WAIT2 state, during the process of closing a connection\. Unit: Count  | 
|  netstat\_tcp\_last\_ack |  The number of TCP connections waiting for the client to send acknowledgement of the connection termination message\. This is the last state right before the connection is closed down\. Unit: Count  | 
|  netstat\_tcp\_listen |  The number of TCP ports currently listening for a connection request\. Unit: Count  | 
|  netstat\_tcp\_none |  The number of TCP connections with inactive clients\. Unit: Count  | 
|  netstat\_tcp\_syn\_sent |  The number of TCP connections waiting for a matching connection request after having sent a connection request\. Unit: Count  | 
|  netstat\_tcp\_syn\_recv |  The number of TCP connections waiting for connection request acknowledgement after having sent and received a connection request\. Unit: Count  | 
|  netstat\_tcp\_time\_wait |  The number of TCP connections currently waiting to ensure the client received the acknowledgement of its connection termination request\. Unit: Count  | 
|  netstat\_udp\_socket |  The number of current UDP connections\. Unit: Count  | 
|  processes\_blocked |  The number of processes that are blocked\. Unit: Count  | 
|  processes\_dead |  The number of processes that are "dead," which is indicated by the X state code on Linux\. Unit: Count  | 
|  processes\_idle |  The number of processes that are idle \(sleeping for more than 20 seconds\)\. Available only on FreeBSD instances\. Unit: Count  | 
|  processes\_paging |  The number of processes that are paging, which is indicated by the W state code on Linux\. Unit: Count  | 
|  processes\_running |  The number of processes that are running, indicated by the R state code\. Unit: Count  | 
|  processes\_sleeping |  The number of processes that are sleeping, indicated by the S state code\. Unit: Count  | 
|  processes\_stopped |  The number of processes that are stopped, indicated by the T state code\. Unit: Count  | 
|  processes\_total |  The total number of processes on the instance\. Unit: Count  | 
|  processes\_total\_threads |  The total number of threads making up the processes\. This metric is available only on Linux instances\. Unit: Count  | 
|  processes\_wait |  The number of processes that are paging, which is indicated by the W state code on FreeBSD instances\. This metric is available only on FreeBSD instances\. Unit: Count  | 
|  processes\_zombies |  The number of zombie processes, indicated by the Z state code\. Unit: Count  | 
|  swap\_free |  The amount of swap space that is not being used\. Unit: Bytes  | 
|  swap\_used |  The amount of swap space currently in use\. Unit: Bytes  | 
|  swap\_used\_percent |  The percentage of swap space currently in use\. Unit: Percent  | 