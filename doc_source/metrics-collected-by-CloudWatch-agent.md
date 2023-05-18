# Metrics collected by the CloudWatch agent<a name="metrics-collected-by-CloudWatch-agent"></a>

You can collect metrics from servers by installing the CloudWatch agent on the server\. You can install the agent on both Amazon EC2 instances and on\-premises servers, and on computers running either Linux, Windows Server, or macOS\. If you install the agent on an Amazon EC2 instance, the metrics it collects are in addition to the metrics enabled by default on Amazon EC2 instances\.

For information about installing the CloudWatch agent on an instance, see [Collect metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](Install-CloudWatch-Agent.md)\.

All metrics discussed in this section are collected directly by the CloudWatch agent\.

## Metrics collected by the CloudWatch agent on Windows Server instances<a name="windows-metrics-enabled-by-CloudWatch-agent"></a>

On a server running Windows Server, installing the CloudWatch agent enables you to collect the metrics associated with the counters in Windows Performance Monitor\. The CloudWatch metric names for these counters are created by putting a space between the object name and the counter name\. For example, the `% Interrupt Time` counter of the `Processor` object is given the metric name `Processor % Interrupt Time` in CloudWatch\. For more information about Windows Performance Monitor counters, see the Microsoft Windows Server documentation\.

The default namespace for metrics collected by the CloudWatch agent is `CWAgent`, although you can specify a different namespace when you configure the agent\.

## Metrics collected by the CloudWatch agent on Linux and macOS instances<a name="linux-metrics-enabled-by-CloudWatch-agent"></a>

The following table lists the metrics that you can collect with the CloudWatch agent on Linux servers and macOS computers\.


| Metric | Description | 
| --- | --- | 
|  `cpu_time_active` |  The amount of time that the CPU is active in any capacity\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_guest` |  The amount of time that the CPU is running a virtual CPU for a guest operating system\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_guest_nice` |  The amount of time that the CPU is running a virtual CPU for a guest operating system, which is low\-priority and can be interrupted by other processes\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_idle` |  The amount of time that the CPU is idle\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_iowait` |  The amount of time that the CPU is waiting for I/O operations to complete\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_irq` |  The amount of time that the CPU is servicing interrupts\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_nice` |  The amount of time that the CPU is in user mode with low\-priority processes, which can easily be interrupted by higher\-priority processes\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_softirq` |  The amount of time that the CPU is servicing software interrupts\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_steal` |  The amount of time that the CPU is in *stolen time*, which is time spent in other operating systems in a virtualized environment\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_system` |  The amount of time that the CPU is in system mode\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_time_user` |  The amount of time that the CPU is in user mode\. This metric is measured in hundredths of a second\. Unit: None  | 
|  `cpu_usage_active` |  The percentage of time that the CPU is active in any capacity\. Unit: Percent  | 
|  `cpu_usage_guest` |  The percentage of time that the CPU is running a virtual CPU for a guest operating system\. Unit: Percent  | 
|  `cpu_usage_guest_nice` |  The percentage of time that the CPU is running a virtual CPU for a guest operating system, which is low\-priority and can be interrupted by other processes\. Unit: Percent  | 
|  `cpu_usage_idle` |  The percentage of time that the CPU is idle\. Unit: Percent  | 
|  `cpu_usage_iowait` |  The percentage of time that the CPU is waiting for I/O operations to complete\. Unit: Percent  | 
|  `cpu_usage_irq` |  The percentage of time that the CPU is servicing interrupts\. Unit: Percent  | 
|  `cpu_usage_nice` |  The percentage of time that the CPU is in user mode with low\-priority processes, which higher\-priority processes can easily interrupt\. Unit: Percent  | 
|  `cpu_usage_softirq` |  The percentage of time that the CPU is servicing software interrupts\. Unit: Percent  | 
|  `cpu_usage_steal` |  The percentage of time that the CPU is in *stolen time*, or time spent in other operating systems in a virtualized environment\. Unit: Percent  | 
|  `cpu_usage_system` |  The percentage of time that the CPU is in system mode\. Unit: Percent  | 
|  `cpu_usage_user` |  The percentage of time that the CPU is in user mode\. Unit: Percent  | 
|  `disk_free` |  Free space on the disks\. Unit: Bytes  | 
|  `disk_inodes_free` |  The number of available index nodes on the disk\. Unit: Count  | 
|  `disk_inodes_total` |  The total number of index nodes reserved on the disk\. Unit: Count  | 
|  `disk_inodes_used` |  The number of used index nodes on the disk\. Unit: Count  | 
|  `disk_total` |  Total space on the disks, including used and free\. Unit: Bytes  | 
|  `disk_used` |  Used space on the disks\. Unit: Bytes  | 
|  `disk_used_percent` |  The percentage of total disk space that is used\. Unit: Percent  | 
|  `diskio_iops_in_progress` |  The number of I/O requests that have been issued to the device driver but have not yet completed\. Unit: Count  | 
|  `diskio_io_time` |  The amount of time that the disk has had I/O requests queued\. Unit: Milliseconds The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `diskio_reads` |  The number of disk read operations\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `diskio_read_bytes` |  The number of bytes read from the disks\. Unit: Bytes The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `diskio_read_time` |  The amount of time that read requests have waited on the disks\. Multiple read requests waiting at the same time increase the number\. For example, if 5 requests all wait for an average of 100 milliseconds, 500 is reported\. Unit: Milliseconds The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `diskio_writes` |  The number disk write operations\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `diskio_write_bytes` |  The number of bytes written to the disks\. Unit: Bytes The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `diskio_write_time` |  The amount of time that write requests have waited on the disks\. Multiple write requests waiting at the same time increase the number\. For example, if 8 requests all wait for an average of 1000 milliseconds, 8000 is reported\. Unit: Milliseconds The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `ethtool_bw_in_allowance_exceeded` |  The number of packets queued and/or dropped because the inbound aggregate bandwidth exceeded the maximum for the instance\.\. This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](CloudWatch-Agent-network-performance.md) Unit: None  | 
|  `ethtool_bw_out_allowance_exceeded` |  The number of packets queued and/or dropped because the outbound aggregate bandwidth exceeded the maximum for the instance\.\. This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](CloudWatch-Agent-network-performance.md) Unit: None  | 
|  `ethtool_conntrack_allowance_exceeded` |  The number of packets dropped because connection tracking exceeded the maximum for the instance and new connections could not be established\. This can result in packet loss for traffic to or from the instance\.  This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](CloudWatch-Agent-network-performance.md) Unit: None  | 
|  `ethtool_linklocal_allowance_exceeded` |  The number of packets dropped because the PPS of the traffic to local proxy services exceeded the maximum for the network interface\. This impacts traffic to the DNS service, the Instance Metadata Service, and the Amazon Time Sync Service\.  This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](CloudWatch-Agent-network-performance.md) Unit: None  | 
|  `ethtool_pps_allowance_exceeded` |  The number of packets queued and/or dropped because the bidirectional PPS exceeded the maximum for the instance\.  This metric is collected only if you have listed it in the `ethtool` subsection of the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Collect network performance metrics](CloudWatch-Agent-network-performance.md)\. Unit: None  | 
|  `mem_active` |  The amount of memory that has been used in some way during the last sample period\. Unit: Bytes  | 
|  `mem_available` |  The amount of memory that is available and can be given instantly to processes\. Unit: Bytes  | 
|  `mem_available_percent` |  The percentage of memory that is available and can be given instantly to processes\. Unit: Percent  | 
|  `mem_buffered` |  The amount of memory that is being used for buffers\. Unit: Bytes  | 
|  `mem_cached` |  The amount of memory that is being used for file caches\. Unit: Bytes  | 
|  `mem_free` |  The amount of memory that isn't being used\. Unit: Bytes  | 
|  `mem_inactive` |  The amount of memory that hasn't been used in some way during the last sample period Unit: Bytes  | 
|  `mem_total` |  The total amount of memory\. Unit: Bytes  | 
|  `mem_used` |  The amount of memory currently in use\. Unit: Bytes  | 
|  `mem_used_percent` |  The percentage of memory currently in use\. Unit: Percent  | 
|  `net_bytes_recv` |  The number of bytes received by the network interface\. Unit: Bytes The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_bytes_sent` |  The number of bytes sent by the network interface\. Unit: Bytes The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_drop_in` |  The number of packets received by this network interface that were dropped\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_drop_out` |  The number of packets transmitted by this network interface that were dropped\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_err_in` |  The number of receive errors detected by this network interface\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_err_out` |  The number of transmit errors detected by this network interface\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_packets_sent` |  The number of packets sent by this network interface\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `net_packets_recv` |  The number of packets received by this network interface\. Unit: Count The only statistic that should be used for this metric is `Sum`\. Do not use `Average`\.  | 
|  `netstat_tcp_close` |  The number of TCP connections with no state\. Unit: Count  | 
|  `netstat_tcp_close_wait` |  The number of TCP connections waiting for a termination request from the client\. Unit: Count  | 
|  `netstat_tcp_closing` |  The number of TCP connections that are waiting for a termination request with acknowledgement from the client\. Unit: Count  | 
|  `netstat_tcp_established` |  The number of TCP connections established\. Unit: Count  | 
|  `netstat_tcp_fin_wait1` |  The number of TCP connections in the `FIN_WAIT1` state during the process of closing a connection\. Unit: Count  | 
|  `netstat_tcp_fin_wait2` |  The number of TCP connections in the `FIN_WAIT2` state during the process of closing a connection\. Unit: Count  | 
|  `netstat_tcp_last_ack` |  The number of TCP connections waiting for the client to send acknowledgement of the connection termination message\. This is the last state right before the connection is closed down\. Unit: Count  | 
|  `netstat_tcp_listen` |  The number of TCP ports currently listening for a connection request\. Unit: Count  | 
|  `netstat_tcp_none` |  The number of TCP connections with inactive clients\. Unit: Count  | 
|  `netstat_tcp_syn_sent` |  The number of TCP connections waiting for a matching connection request after having sent a connection request\. Unit: Count  | 
|  `netstat_tcp_syn_recv` |  The number of TCP connections waiting for connection request acknowledgement after having sent and received a connection request\. Unit: Count  | 
|  `netstat_tcp_time_wait` |  The number of TCP connections currently waiting to ensure that the client received the acknowledgement of its connection termination request\. Unit: Count  | 
|  `netstat_udp_socket` |  The number of current UDP connections\. Unit: Count  | 
|  `processes_blocked` |  The number of processes that are blocked\. Unit: Count  | 
|  `processes_dead` |  The number of processes that are dead, indicated by the `X` state code on Linux\. This metric is not collected on macOS computers\. Unit: Count  | 
|  `processes_idle` |  The number of processes that are idle \(sleeping for more than 20 seconds\)\. Available only on FreeBSD instances\. Unit: Count  | 
|  `processes_paging` |  The number of processes that are paging, indicated by the `W` state code on Linux\. This metric is not collected on macOS computers\. Unit: Count  | 
|  `processes_running` |  The number of processes that are running, indicated by the `R` state code\. Unit: Count  | 
|  `processes_sleeping` |  The number of processes that are sleeping, indicated by the `S` state code\. Unit: Count  | 
|  `processes_stopped` |  The number of processes that are stopped, indicated by the `T` state code\. Unit: Count  | 
|  `processes_total` |  The total number of processes on the instance\. Unit: Count  | 
|  `processes_total_threads` |  The total number of threads making up the processes\. This metric is available only on Linux instances\. This metric is not collected on macOS computers\. Unit: Count  | 
|  `processes_wait` |  The number of processes that are paging, indicated by the `W` state code on FreeBSD instances\. This metric is available only on FreeBSD instances, and is not available on Linux, Windows Server, or macOS instances\. Unit: Count  | 
|  `processes_zombies` |  The number of zombie processes, indicated by the `Z` state code\. Unit: Count  | 
|  `swap_free` |  The amount of swap space that isn't being used\. Unit: Bytes  | 
|  `swap_used` |  The amount of swap space currently in use\. Unit: Bytes  | 
|  `swap_used_percent` |  The percentage of swap space currently in use\. Unit: Percent  | 

## Definitions of memory metrics collected by the CloudWatch agent<a name="CloudWatch-agent-metrics-definitions"></a>

When the CloudWatch agent collects memory metrics, the source is the host's memory management subsystem\. For example, the Linux kernel exposes OS\-maintained data in `/proc`\. For memory, the data is in `/proc/meminfo`\. 

Each different operating system and architecture has different calculations of the resources that are used by processes\. For more information, see the following sections\.

During each collection interval, the CloudWatch agent on each instance collects the instance resources and calculates the resources being used by all processes which are running in that instance\. This information is reported back to CloudWatch metrics\. You can configure the length of the collection interval in the CloudWatch agent configuration file\. For more information, see [ CloudWatch agent configuration file: Agent section](CloudWatch-Agent-Configuration-File-Details.md#CloudWatch-Agent-Configuration-File-Agentsection)\.

The following list explains how the memory metrics that the CloudWatch agent collects are defined\.
+ **Active Memory**– Memory that is being used by a process\. In other words, the memory used by current running apps\.
+  **Available Memory**– The memory that can be instantly given to the processes without the system going into swap \(also known as virtual memory\)\. 
+ **Buffer Memory**– The data area shared by hardware devices or program processes that operate at different speeds and priorities\.
+ **Cached Memory**– Stores program instructions and data that are used repeatedly in the operation of programs that the CPU is likely to need next\.
+ **Free Memory**– Memory that is not being used at all and is readily available\. It is completely free for the system to be used when needed\.
+ **Inactive Memory**– Pages that have not been accessed "recently"\.
+ **Total Memory**– The size of the actual physical memory RAM\.
+ **Used Memory**– Memory that is currently in use by programs and processes\.

**Topics**
+ [Linux: Metrics collected and calculations used](#CloudWatch-agent-metrics-definitions-calculations)
+ [macOS: Metrics collected and calculations used](#CloudWatch-agent-metrics-definitions-calculations)
+ [Windows: Metrics collected](#CloudWatch-agent-metrics-definitions-calculations)
+ [Example: Calculating memory metrics on Linux](#CloudWatch-agent-metrics-definitions-LinuxExample)

### Linux: Metrics collected and calculations used<a name="CloudWatch-agent-metrics-definitions-calculations"></a>

Metrics collected and units:
+ Active \(Bytes\)
+ Available \(Bytes\)
+ Available Percent \(Percent\)
+ Buffered \(Bytes\)
+ Cached \(Bytes\)
+ Free \(Bytes\)
+ Inactive \(Bytes\)
+ Total \(Bytes\)
+ Used \(Bytes\)
+ Used Percent \(Percent\)

**Used memory** = Total Memory \- Free Memory \- Cached memory \- Buffer memory

**Total memory** = Used Memory \+ Free Memory \+ Cached memory \+ Buffer memory

### macOS: Metrics collected and calculations used<a name="CloudWatch-agent-metrics-definitions-calculations"></a>

Metrics collected and units:
+ Active \(Bytes\)
+ Available \(Bytes\)
+ Available Percent \(Percent\)
+ Free \(Bytes\)
+ Inactive \(Bytes\)
+ Total \(Bytes\)
+ Used \(Bytes\)
+ Used Percent \(Percent\)

**Available memory** = Free Memory \+ Inactive memory

**Used memory** = Total Memory \- Available memory

**Total memory** = Available Memory \+ Used Memory

### Windows: Metrics collected<a name="CloudWatch-agent-metrics-definitions-calculations"></a>

The metrics collected on Windows hosts are listed below\. All of these metrics have `None` for `Unit`\.
+ Available bytes
+ Cache Faults/sec
+ Page Faults/sec
+ Pages/sec

There are no calculations used for Windows metrics becuase the CloudWatch agent parses events from performance counters\.

### Example: Calculating memory metrics on Linux<a name="CloudWatch-agent-metrics-definitions-LinuxExample"></a>

As an example, suppose that entering the **free \-m** command on a Linux host shows the following results:

```
             total       used       free     shared    buff/cache     Available
Mem:         7441         117       6592          0           732          7101
Swap:           0           0          0
```

In this example, the CloudWatch agent will collect the following values:
+ `mem_total`: 7441\.92 MB
+ `mem_available`: 7101\.85 MB \(free \+ cache\)
+ `mem_free`: 6592\.85 MB
+ `mem_cached`: 2\.64 MB
+ `mem_used`: 116\.84 MB \(total \- free \-buff/cache\)
+ `mem_buffered`: 729\.59 MB
+ `mem_available_percent`: 95\.43%
+ `mem_used_percent`: 1\.56%