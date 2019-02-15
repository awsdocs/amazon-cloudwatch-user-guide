# Collect Process Metrics with the procstat Plugin<a name="CloudWatch-Agent-procstat-process-metrics"></a>

The *procstat* plugin enables you to collect metrics from individual processes\. It is supported on Linux servers, and servers running Windows Server 2008 or later\.

**Topics**
+ [Configuring the CloudWatch Agent for procstat](#CloudWatch-Agent-procstat-configuration)
+ [Metrics Collected by procstat](#CloudWatch-Agent-procstat-process-metrics-collected)

## Configuring the CloudWatch Agent for procstat<a name="CloudWatch-Agent-procstat-configuration"></a>

To use the procstat plugin, add a `procstat` section in the `metrics_collected` section of the CloudWatch agent configuration file\. There are three ways to specify the processes to monitor\. You can use only one of these methods, but you can use that method to specify one or more processes to monitor\.
+ **pid\_file**: Selects processes by the names of the process identification number \(PID\) files they create\. 
+ **exe**: Selects the processes that have process names that match the string that you specify, using regular expression matching rules\. For more information, see [Syntax](https://github.com/google/re2/wiki/Syntax)\.
+ **pattern**: Selects processes by the command lines used to start the processes\. All processes are selected that have command lines matching the specified string using regular expression matching rules\. The entire command line is checked, including parameters and options used with the command\.

The CloudWatch agent uses only one of these methods, even if you include more than one of the above sections\. If you specify more than one section, the CloudWatch agent uses the `pid_file` section if it is present\. If not, it uses the `exe` section\.

On Linux servers, the strings that you specify in an `exe` or `pattern` section are evaluated as regular expressions\. On servers running Windows Server, these strings are evaluated as WMI queries\. For more information, see [LIKE Operator](https://docs.microsoft.com/en-us/windows/desktop/WmiSdk/like-operator)\.

Whichever method you use, you can include an optional `metrics_collection_interval` parameter, which specifies how often in seconds to collect those metrics\. If you omit this, the default of 60 seconds is used\.

In the examples in the following sections, the `procstat` section is the only section included in the `metrics_collected` section of the agent configuration file\. Actual configuration files can also include other sections in `metrics_collected`\. For more information, see [ Manually Create or Edit the CloudWatch Agent Configuration File](CloudWatch-Agent-Configuration-File-Details.md)\.

### Configuring with pid\_file<a name="CloudWatch-Agent-procstat-configuration-pidfile"></a>

The following example `procstat` section monitors the processes that create the PID files `example1.pid` and `example2.pid`\. Different metrics are collected from each process\. Metrics collected from the process that creates `example2.pid` are collected every 10 seconds, while the metrics collected from the `example1.pid` process are collected every 60 seconds, the default\. 

```
{
    "metrics": {
        "metrics_collected": {
            "procstat": [
                {
                    "pid_file": "/var/run/example1.pid",
                    "measurement": [
                        "cpu_usage",
                        "memory_rss"
                    ]
                },
                {
                    "pid_file": "/var/run/example2.pid",
                    "measurement": [
                        "read_bytes",
                        "read_count",
                        "write_bytes"
                    ],
                    "metrics_collection_interval": 10
                }
            ]
        }
    }
}
```

### Configuring with exe<a name="CloudWatch-Agent-procstat-configuration-exe"></a>

The following example `procstat` section monitors all processes with names that match the strings `agent` or `plugin`\. The same metrics are collected from each process\. 

```
{
    "metrics": {
        "metrics_collected": {
            "procstat": [
                {
                    "exe": "agent",
                    "measurement": [
                        "cpu_time",
                        "cpu_time_system",
                        "cpu_time_user"
                    ]
                },
                {
                    "exe": "plugin",
                    "measurement": [
                        "cpu_time",
                        "cpu_time_system",
                        "cpu_time_user"
                    ]
                }
            ]
        }
    }
}
```

### Configuring with pattern<a name="CloudWatch-Agent-procstat-configuration-pattern"></a>

The following example `procstat` section monitors all processes with command lines that match the strings `config` or `-c`\. The same metrics are collected from each process\. 

```
{
    "metrics": {
        "metrics_collected": {
            "procstat": [
                {
                    "pattern": "config",
                    "measurement": [
                        "rlimit_memory_data_hard",
                        "rlimit_memory_data_soft",
                        "rlimit_memory_stack_hard",
                        "rlimit_memory_stack_soft"
                    ]
                },
                {
                    "pattern": "-c",
                    "measurement": [
                        "rlimit_memory_data_hard",
                        "rlimit_memory_data_soft",
                        "rlimit_memory_stack_hard",
                        "rlimit_memory_stack_soft"
                    ]
                }
            ]
        }
    }
}
```

## Metrics Collected by procstat<a name="CloudWatch-Agent-procstat-process-metrics-collected"></a>

The following table lists the metrics that you can collect with the procstat plugin\.

The CloudWatch agent adds `procstat` to the beginning of the following metric names\. There is a different syntax depending on whether it was collected from a Linux server or a server running Windows Server\. For example, the `cpu_time` metric appears as `procstat_cpu_time` when collected from Linux, and as `procstat cpu_time` when collected from Windows Server\.


| Metric Name | Available On | Description | 
| --- | --- | --- | 
|  cpu\_time |  Linux |  The amount of time that the process uses the CPU\. This metric is measured in hundredths of a second\. Unit: Count  | 
|  cpu\_time\_system |  Linux, Windows Server |  The amount of time that the process is in system mode\. This metric is measured in hundredths of a second\. Type: Float Unit: Count  | 
|  cpu\_time\_user |  Linux, Windows Server |  The amount of time that the process is in user mode\. This metric is measured in hundredths of a second\. Unit: Count  | 
|  cpu\_usage |  Linux, Windows Server |  The percentage of time that the process is active in any capacity\. Unit: Percent  | 
|  memory\_data |  Linux |  The amount of memory the process uses for data\. Unit: Bytes  | 
|  memory\_locked |  Linux |  The amount of memory the process has locked\. Unit: Bytes  | 
|  memory\_rss |  Linux, Windows Server |  The amount of real memory \(resident set\) that the process is using\. Unit: Bytes  | 
|  memory\_stack |  Linux |  The amount of stack memory the process is using\. Unit: Bytes  | 
|  memory\_swap |  Linux |  The amount of swap memory the process is using\. Unit: Bytes  | 
|  memory\_vms |  Linux, Windows Server |  The amount of virtual memory the process is using\. Unit: Bytes  | 
|  read\_bytes |  Linux, Windows Server |  The number of bytes the process has read from disks\. Unit: Bytes  | 
|  write\_bytes |  Linux, Windows Server |  The number of bytes the process has written to disks\. Unit: Bytes  | 
|  read\_count |  Linux, Windows Server |  The number of disk read operations executed by the process\. Unit: Count  | 
|  write\_count |  Linux, Windows Server |  The number of disk write operations that the process has executed\. Unit: Count  | 
|  involuntary\_context\_switches |  Linux |  The number of times the process was involuntarily context\-switched\.  Unit: Count  | 
|  voluntary\_context\_switches |  Linux |  The number of times the process was context\-switched voluntarily\.  Unit: Count  | 
|  realtime\_priority |  Linux |  The current usage of realtime priority for the process\. Unit: Count  | 
|  nice\_priority |  Linux |  The current usage of nice priority for the process\. Unit: Count  | 
|  signals\_pending |  Linux |  The number of signals pending to be handled by the process\. Unit: Count  | 
|  rlimit\_cpu\_time\_hard |  Linux |  The hard CPU time resource limit for the process\. Unit: Count  | 
|  rlimit\_cpu\_time\_soft |  Linux |  The soft CPU time resource limit for the process\. Unit: Count  | 
|  rlimit\_file\_locks\_hard |  Linux |  The hard file locks resource limit for the process\. Unit: Count  | 
|  rlimit\_file\_locks\_soft |  Linux |  The soft file locks resource limit for the process\. Unit: Count  | 
|  rlimit\_memory\_data\_hard |  Linux |  The hard resource limit on the process for memory used for data\. Unit: Bytes  | 
|  rlimit\_memory\_data\_soft |  Linux |  The soft resource limit on the process for memory used for data\. Unit: Bytes  | 
|  rlimit\_memory\_locked\_hard |  Linux |  The hard resource limit on the process for locked memory\. Unit: Bytes  | 
|  rlimit\_memory\_locked\_soft |  Linux |  The soft resource limit on the process for locked memory\. Unit: Bytes  | 
|  rlimit\_memory\_rss\_hard |  Linux |  The hard resource limit on the process for physical memory\. Unit: Bytes  | 
|  rlimit\_memory\_rss\_soft |  Linux |  The soft resource limit on the process for physical memory\. Unit: Bytes  | 
|  rlimit\_memory\_stack\_hard |  Linux |  The hard resource limit on the process stack\. Unit: Bytes  | 
|  rlimit\_memory\_stack\_soft |  Linux |  The soft resource limit on the process stack\. Unit: Bytes  | 
|  rlimit\_memory\_vms\_hard |  Linux |  The hard resource limit on the process for virtual memory\. Unit: Bytes  | 
|  rlimit\_memory\_vms\_soft |  Linux |  The soft resource limit on the process for virtual memory\. Unit: Bytes  | 
|  rlimit\_nice\_priority\_hard |  Linux |  The hard resource limit on the ceiling for the process's nice priority value\. Unit: Count  | 
|  rlimit\_nice\_priority\_soft |  Linux |  The soft resource limit on the ceiling for the process's nice priority value\. Unit: Count  | 
|  rlimit\_num\_fds\_hard |  Linux |  The hard resource limit on the file descriptors for the process\. Unit: Count  | 
|  rlimit\_num\_fds\_soft |  Linux |  The soft resource limit on the file descriptors for the process\. Unit: Count  | 
|  rlimit\_realtime\_priority\_hard |  Linux |  The hard resource limit on the ceiling for the process's real\-time priority value\. Unit: Count  | 
|  rlimit\_realtime\_priority\_soft |  Linux |  The soft resource limit on the ceiling for the process's real\-time priority value\. Unit: Count  | 
|  rlimit\_signals\_pending\_hard |  Linux |  The hard resource limit on the number of signals that are pending for delivery to the process\. Unit: Count  | 
|  rlimit\_signals\_pending\_soft |  Linux |  The soft resource limit on the number of signals that are pending for delivery to the process\. Unit: Count  | 
|  num\_fds |  Linux |  The number of file descriptors the process is using\. Unit: Count  | 
|  num\_threads |  Linux, Windows Server |  The number of threads in the process\. Unit: Count  | 
|  pid |  Linux, Windows Server |  Process identifier \(ID\)\. Unit: Count  | 
|  pid\_count |  Linux, Windows Server |  The number of process IDs associated with the process\. On Linux servers this metric is named `procstat_lookup_pid_count` and on Windows Server it is `procstat_lookup pid_count`\. Unit: Count  | 