# Manually Create or Edit the CloudWatch Agent Configuration File<a name="CloudWatch-Agent-Configuration-File-Details"></a>

The CloudWatch agent configuration file is a JSON file with three sections: agent, metrics, and logs\.

+ The **agent** section includes fields for overall configuration of the agent\. If you use the wizard, it does not create an `agent` section\. 

+ The **metrics** section specifies the custom metrics for collection and publishing to CloudWatch\. If you are using the agent only to collect logs, you can omit the metrics section from the file\.

+ The **logs** section specifies what log files are published to CloudWatch Logs\. This can include events from the Windows Event Log, if the server runs Windows Server\.

The following sections explain the structure and fields of this JSON file\. You can also view the schema definition for this configuration file\. The schema definition is located at `installation-directory/doc/amazon-cloudwatch-agent-schema.json` on Linux servers, and at `installation-directory/amazon-cloudwatch-agent-schema.json` on servers running Windows Server\.

## CloudWatch Agent Configuration File: Agent Section<a name="CloudWatch-Agent-Configuration-File-Agentsection"></a>

The **agent** section can include the fields listed below\. The wizard does not create an `agent` section\. Instead, the wizard omits it and uses the default values for all fields in this section\.

+ **metrics\_collection\_interval** – Optional\. Specifies how often all metrics specified in this configuration file are to be collected\. This value can be overriden for specific types of metrics\.

  This is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

  If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  The default is 60\. 

+ **region** – Specifies the region to use for the CloudWatch endpoint, when an Amazon EC2 instance is being monitored\. The metrics collected are sent to this region, such as `us-west-1`\. If you omit this field, the agent sends metrics to the region where the Amazon EC2 instance is located\.

  If you are monitoring an on\-premises server, this field is not used, and the agent reads the region from the `awscloudwatchagent` profile of the AWS configuration file\.

+ **debug** – Optional\. Specifies running the CloudWatch agent with debug log messages\. The default is `false`\. 

+ **logfile** – Specifies the location to where the CloudWatch agent writes log messages\. If you specify an empty string, the log goes to stderr\. If you don't specify this option, the default locations are the following:

  + Linux: `/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log`

  + Windows Server versions later than Windows Server 2003: `c:\\ProgramData\\Amazon\\CloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log` 
**Tip**  
We suggest you set up log rotation for this file so that it doesn't grow and fill the disk\.

The following is an example of an `agent` section:

```
"agent": {
   "metrics_collection_interval": 60,
   "region": "us-west-1",
   "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
   "debug": false
  }
```

## CloudWatch Agent Configuration File: Metrics Section<a name="CloudWatch-Agent-Configuration-File-Metricssection"></a>

On servers running either Linux or Windows Server, the **metrics** section includes the following fields:

+ **namespace** – Optional\. The namespace to use for the metrics collected by the agent\. The default is `CWAgent`\. 

+ **append\_dimensions** – Optional\. Adds Amazon EC2 metric dimensions to all metrics collected by the agent\. For each dimension, you must specify a key\-value pair, where the key matches an Amazon EC2 dimension: `ImageID:image-id`, `InstanceId:instance-id`, `InstanceType:instance-type`, or `AutoScalingGroupName:AutoScaling-group-name`\. 

+ **aggregation\_dimensions** – Specifies the dimensions on which collected metrics are to be aggregated\. For example, if you roll up metrics on the `AutoScalingGroupName` dimension, the metrics from all instances in each Auto Scaling group are aggregated and can be viewed as a whole\.

  You can roll up metrics along single or multiple dimensions\. For example, specifying `[["InstanceId"], ["InstanceType"], ["InstanceId","InstanceType"]]` aggregates metrics for Instance ID singly, Instance Type singly, and for the combination of the two dimensions\.

  You can also specify `[]` to roll up all metrics into one collection, disregarding all dimensions\.

+ **metrics\_collected** – Required\. Specifies which metrics are to be collected\. This section includes several subsections\. 

  The contents of the `metrics_collected` section depend on whether this configuration file is for a server running Linux or Windows Server\.

### Linux<a name="CloudWatch-Agent-Linux-section"></a>

On servers running Linux, the **metrics\_collected** section of the configuration file can also contain the following fields:

+ **cpu** – Optional\. Specifies that cpu metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as three fields:

  + **resources** – Optional\. Specifies that per\-cpu metrics are to be collected\. The only allowed value is `*`\. If you include this field and value, per\-cpu metrics are collected\. 

  + **totalcpu** – Optional\. Specifies whether to report cpu metrics aggregated across all cpu cores\. The default is true\.

  + **measurement** – Specifies the array of cpu metrics to be collected\. Possible values are `time_active`, `time_guest`, `time_guest_nice`, `time_idle`, `time_iowait`, `time_irq`, `time_nice`, `time_softirq`, `time_steal`, `time_system`, `time_user`, `usage_guest`, `usage_guest_nice`, `usage_idle`, `usage_iowait`, `usage_irq`, `usage_nice`, `usage_softirq`, `usage_steal`, `usage_system`, and `usage_user`\. This field is required if you include `cpu`\.

    By default, the unit for `cpu_usage_*` metrics is `Percent`, and `cpu_time_*` metrics do not have a unit\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the cpu metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the cpu metrics\. If you specify this field, it is used in addition to dimensions specified in the global `append_dimensions` field that is used for all types of metrics collected by the agent\.

+ **disk** – Optional\. Specifies that disk metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as two fields:

  + **resources** – Optional\. Specifies disk mount points\. This field limits CloudWatch to collect metrics from only the listed mount points\. You can specify \* as the value to collect metrics from all mount points\. The default is to collect metrics from all mount points\. 

  + **measurement** – Specifies the array of disk metrics to be collected\. Possible values are `free`, `total`, `used`, `used_percent`, `inodes_free`, `inodes_used`, and `inodes_total`\. This field is required if you include `disk`\.

    To see the default units for each `disk` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](CW_Support_For_AWS.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the disk metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the disk metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent

+ **diskio** – Optional\. Specifies that diskio metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as two fields:

  + **resources** – Optional\. If you specify an array of devices, CloudWatch collects metrics from only those devices\. Otherwise, metrics for all devices are collected\. You can also specify \* as the value to collect metrics from all devices\.

  + **measurement** – Specifies the array of diskio metrics to be collected\. Possible values are `reads`, `writes`, `read_bytes`, `write_bytes`, `read_time`, `write_time`, `io_time`, and `iops_in_progress`\. This field is required if you include `diskio`\.

    To see the default units for each `diskio` metric, see [Amazon EC2 Metrics and Dimensions](ec2-metricscollected.md)\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the diskio metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the diskio metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.

+ **swap** – Optional\. Specifies that swap memory metrics are to be collected\. This section is valid only for Linux instances\. This section can include one field:

  + **measurement** – Specifies the array of swap metrics to be collected\. Possible values are `free`, `used`, and `used_percent`\. This field is required if you include `swap`\.

    To see the default units for each `swap` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](CW_Support_For_AWS.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the swap metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\. 

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** Optional\. Additional dimensions to use for only the swap metrics\. If you specify this field, it is used in addition to dimensions specified in the global `append_dimensions` field that is used for all types of metrics collected by the agent\. is collected as a high\-resolution metric\. 

+ **mem** – Optional\. Specifies that memory metrics are to be collected\. This section is valid only for Linux instances\. This section can include one field:

  + **measurement** – Specifies the array of swap metrics to be collected\. Possible values are `active`, `available`, `available_percent`, `buffered`, `cached`, `free`, `inactive`, `total`, `used`, and `used_percent`\. This field is required if you include `mem`\.

    To see the default units for each `mem` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](CW_Support_For_AWS.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the mem metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the mem metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.

+ **net** – Optional\. Specifies that networking metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as two fields:

  + **resources** – Optional\. If you specify an array of network interfaces, CloudWatch collects metrics from only those interfaces\. Otherwise, metrics for all devices are collected\. You can also specify \* as the value to collect metrics from all interfaces\.

  + **measurement** – Specifies the array of networking metrics to be collected\. Possible values are `bytes_sent`, `bytes_recv`, `drop_in`, `drop_out`, `err_in`, `err_out`, `packets_sent`, and `packets_recv`\. This field is required if you include `net`\.

    To see the default units for each `net` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](CW_Support_For_AWS.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the net metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the net metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.

+ **netstat** – Optional\. Specifies that TCP connection state and UDP connection metrics are to be collected\. This section is valid only for Linux instances\. This section can include one field:

  + **measurement** – Specifies the array of netstat metrics to be collected\. Possible values are `tcp_close`, `tcp_close_wait`, `tcp_closing`, `tcp_established`, `tcp_fin_wait1`, `tcp_fin_wait2`, `tcp_last_ack`, `tcp_listen`, `tcp_none`, `tcp_syn_sent`, `tcp_syn_recv`, `tcp_time_wait`, and `udp_socket`\. This field is required if you include `netstat`\.

    To see the default units for each `netstat` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](CW_Support_For_AWS.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the netstat metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\.

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the netstat metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.

+ **processes** – Optional\. Specifies that process metrics are to be collected\. This section is valid only for Linux instances\. This section can include one field:

  + **measurement** – Specifies the array of processes metrics to be collected\. Possible values are `blocked`, `dead`, `idle`, `paging`, `running`, `sleeping`, `stopped`, `total`, `total_threads`, `wait`, and `zombie`\. This field is required if you include `processes`\.

    For all `processes` metrics, the default unit is `Count`\.

    Within the entry for each individual metric, you may optionally specify one or both of the following:

    + **rename** – Specifies a different name for this metric\.

    + **unit** – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

  + **metrics\_collection\_interval** – Optional\. Specifies how often to collect the processes metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  + **append\_dimensions** – Optional\. Additional dimensions to use for only the process metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.

The following is an example of a `metrics` section for a Linux server:

```
  "metrics": {
    "metrics_collected": {
      "cpu": {
        "resources": [
          "*"
        ],
        "measurement": [
          {"name": "cpu_usage_idle", "rename": "CPU_USAGE_IDLE", "unit": "Percent"},
          {"name": "cpu_usage_nice", "unit": "Percent"},
          "cpu_usage_guest"
        ],
        "totalcpu": false,
        "metrics_collection_interval": 10,
        "append_dimensions": {
          "test": "test1",
          "date": "2017-10-01"
        }
      },
      "netstat": {
        "measurement": [
          "tcp_established",
          "tcp_syn_sent",
          "tcp_close"
        ],
        "metrics_collection_interval": 60
      },
      "processes": {
        "measurement": [
          "running",
          "sleeping",
          "dead"
        ]
      }
    },
    "append_dimensions": {
      "ImageId": "${aws:ImageId}",
      "InstanceId": "${aws:InstanceId}",
      "InstanceType": "${aws:InstanceType}",
      "AutoScalingGroupName": "${aws:AutoScalingGroupName}"
    },
    "aggregation_dimensions" : [["AutoScalingGroupName"], ["InstanceId", "InstanceType"],[]]
  }
```

### Windows Server<a name="CloudWatch-Agent-Windows-section"></a>

In the **metrics\_collected** section for Windows Server, you can have subsections for each Windows performance object, such as `Memory`, `Processor`, and `LogicalDisk`\. For information about what objects and counters are available, see the Microsoft Windows documentation\.

Within the subsection for each object, you specify a `measurement` array of the counters to collect\. The `measurement` array is required for each object that you specify in the configuration file\. You can also specify a `resources` field to name the instances from which to collect metrics\. You can also specify `*` for `resources`, to collect separate metrics for every instance\. If you omit `resources`, the data for all instances is aggregated into one set\.

Within each object section, you can also specify the following optional fields:

+ **metrics\_collection\_interval** – Optional\. Specifies how often to collect the metrics for this object, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

  This is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

  If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

+ **append\_dimensions** –Optional\. Additional dimensions to use for only the metrics for this object\. If you specify this field, it is used in addition to dimensions specified in the global `append_dimensions` field that is used for all types of metrics collected by the agent\. 

Within each counter section, you can also specify the following optional fields:

+ **rename** – Specifies a different name to be used in CloudWatch for this metric\.

+ **unit** – Specifies the unit to use for this metric\. The unit you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)

The following is an example `metrics` section for use on Windows Server:

```
"metrics": {
    "metrics_collected": {
      "Processor": {
        "measurement": [
          {"name": "% Idle Time", "rename": "CPU_IDLE", "unit": "Percent"},
          "% Interrupt Time",
          "% User Time",
          "% Processor Time"
        ],
        "resources": [
          "*"
        ],
        "append_dimensions": {
          "d1": "win_foo",
          "d2": "win_bar"
        }
      },
      "LogicalDisk": {
        "measurement": [
          {"name": "% Idle Time", "unit": "Percent"},
          {"name": "% Disk Read Time", "rename": "DISK_READ"},
          "% Disk Write Time"
        ],
        "resources": [
          "*"
        ]
      },
      "Memory": {
        "metrics_collection_interval": 5,
        "measurement": [
          "Available Bytes",
          "Cache Faults/sec",
          "Page Faults/sec",
          "Pages/sec"
        ],
        "append_dimensions": {
          "d3": "win_bo"
        }
      },
      "Network Interface": {
        "metrics_collection_interval": 5,
        "measurement": [
          "Bytes Received/sec",
          "Bytes Sent/sec",
          "Packets Received/sec",
          "Packets Sent/sec"
        ],
        "resources": [
          "*"
        ],
        "append_dimensions": {
          "d3": "win_bo"
        }
      },
      "System": {
        "measurement": [
          "Context Switches/sec",
          "System Calls/sec",
          "Processor Queue Length"
        ],
        "append_dimensions": {
          "d1": "win_foo",
          "d2": "win_bar"
        }
      }
    },
    "append_dimensions": {
      "ImageId": "${aws:ImageId}",
      "InstanceId": "${aws:InstanceId}",
      "InstanceType": "${aws:InstanceType}",
      "AutoScalingGroupName": "${aws:AutoScalingGroupName}"
    },
    "aggregation_dimensions" : [["ImageId"], ["InstanceId", "InstanceType"], ["d1"],[]],
    }
  }
```

## CloudWatch Agent Configuration File: Logs Section<a name="CloudWatch-Agent-Configuration-File-Logssection"></a>

The **logs** section includes the following fields:

+ **logs\_collected** – Required if the **logs** section is included\. Specifies which log file and Windows event logs are to be collected from the server\. It can include two fields, **files** and **windows\_events**\.

  + **files** specifies which regular log files are to be collected by the CloudWatch agent\. It contains one field, **collect\_list**, which further defines these files\.

    + **collect\_list** – Required if **files** is included\. Contains an array of entries, each of which specifies one log file to collect\. Each of these entries can include the following fields:

      + **file\_path** – Specifies the path of the log file to upload to CloudWatch Logs\. Standard Unix glob matching rules are accepted, with the addition of `**` as a *super asterisk*\. For example, specifying `/var/log/**.log` causes all `.log` files in the `/var/log` directory tree to be collected\. For more examples, see [Glob Library](https://github.com/gobwas/glob)\.

      + **log\_group\_name** – Optional\. Specifies what to use as the log group name in CloudWatch Logs\. If this is not specified, the file path up to the final dot is used as the log group name\. For example, if the file path is `/tmp/TestLogFile.log.2017-07-11-14`, the log group name is `/tmp/TestLogFile.log`\. This field is recommended to prevent confusion\.

      + **log\_stream\_name** – Optional\. Specifies what to use as the log stream name in CloudWatch Logs\. As part of the name, you can use `{instance_id}`, `{hostname}`, and `{ip_address}` as variables within the name\. If you omit this field, the default of `{instance_id}` is used\.

      + **timestamp\_format** – Optional\. Specifies the time stamp format, using plain text and special symbols that start with %\. If you omit this field, the current time is used\. If you use this field, you can use the following as part of the format:   
**%y**  
Year without century as a zero\-padded decimal number  
**%Y**  
Year with century as a decimal number  
**%b**  
Month as the locale's abbreviated name  
**%B**  
Month as the locale's full name  
**%m**  
Month as a zero\-padded decimal number  
**%\-m**  
Month as a decimal number \(not zero\-padded\)  
**%d**  
Day of the month as a zero\-padded decimal number  
**%\-d**  
Day of the month as a decimal number \(not zero\-padded\)  
**%A**  
Full name of weekday, such as `Monday`  
**%a**  
Abbreviation of weekday, such as `Mon`  
**%H**  
Hour \(in a 24\-hour clock\) as a zero\-padded decimal number  
**%l**  
Hour \(in a 12\-hour clock\) as a zero\-padded decimal number  
**%\-l**  
Hour \(in a 12\-hour clock\) as decimal number \(not zero\-padded\)  
**%p**  
AM or PM  
**%M**  
Minutes as a zero\-padded decimal number  
**%\-M**  
Minutes as a decimal number \(not zero\-padded\)  
**%S**  
Seconds as a zero\-padded decimal number  
**%\-S**  
Seconds as a decimal number \(not zero padded\)  
**%Z**  
Time zone, for example `PST`  
**%z**  
Time zone, expressed as the offset between the local time zone and UTC\. For example, `-0700`\.

      + **multi\_line\_start\_pattern** – If you omit this field, multi\-line mode is disabled\. If you include this field, you can specify `{timestamp_format}` to use the same regular expression as your time stamp format\. Otherwise, you can specify a different regular expression for CloudWatch Logs to use to determine the start lines of multi\-line entries\.

+ The **windows\_events** section specifies the type of Windows events to collect from servers running Windows Server\. It includes the following fields:

  + **collect\_list** – Required if **windows\_events** is included\. Specifies the types and levels of Windows events to be collected\. Each log to be collected has an entry in this section, which can include the following fields:

    + **event\_name** – Specifies the type of Windows events to log\. Possible values include `System`, `Security`, `Application`, `Setup`, and `Forwarded Events`\. This field is required for each type of Windows event to log\.

    + **event\_levels** – Specifies the levels of event to log\. You must specify each level to log\. Possible values include `INFORMATION`, `WARNING`, `ERROR`, and `SUCCESS`\. This field is required for each type of Windows event to log\.

    + **log\_group\_name** – Required\. Specifies what to use as the log group name in CloudWatch Logs\. 

    + **log\_stream\_name** – Optional\. Specifies what to use as the log stream name in CloudWatch Logs\. As part of the name, you can use `{instance_id}`, `{hostname}`, and `{ip_address}` as variables within the name\. If you omit this field, the default of `{instance_id}` is used\.

+ **log\_stream\_name** – Required\. Specified the default log stream name to be used for any logs or Windows events that do not have individual log stream names defined in their entry in **collect\_list**\.

The following is an example of a `logs` section:

```
"logs":{
   "logs_collected":{
      "files":{
         "collect_list":[
            {
               "file_path":"c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log",
               "log_group_name":"amazon-cloudwatch-agent.log",
               "log_stream_name":"my_log_stream_name_1",
               "timestamp_format":"%H:%M:%S %y %b %-d"
            },
            {
               "file_path":"c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\test.log",
               "log_group_name":"test.log",
               "log_stream_name":"my_log_stream_name_2"
            }
         ]
      },
      "windows_events":{
         "collect_list":[
            {
               "event_name":"System",
               "event_levels":[
                  "INFORMATION",
                  "SUCCESS"
               ],
               "log_group_name":"System",
               "log_stream_name":"System"
            },
            {
               "event_name":"Application",
               "event_levels":[
                  "INFORMATION",
                  "SUCCESS"
               ],
               "log_group_name":"Application",
               "log_stream_name":"Application"
            }
         ]
      }
   },
   "log_stream_name":"my_log_stream_name"
}
```

## CloudWatch Agent Configuration File: Complete Examples<a name="CloudWatch-Agent-Configuration-File-Complete-Example"></a>

The following is an example of a complete agent configuration file for a Linux server\.

```
    {
      "agent": {
        "metrics_collection_interval": 10,
        "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log"
      },
      "metrics": {
        "metrics_collected": {
          "cpu": {
            "resources": [
              "*"
            ],
            "measurement": [
              {"name": "cpu_usage_idle", "rename": "CPU_USAGE_IDLE", "unit": "Percent"},
              {"name": "cpu_usage_nice", "unit": "Percent"},
              "cpu_usage_guest"
            ],
            "totalcpu": false,
            "metrics_collection_interval": 10,
            "append_dimensions": {
              "customized_dimension_key_1": "customized_dimension_value_1",
              "customized_dimension_key_2": "customized_dimension_value_2"
            }
          },
          "disk": {
            "resources": [
              "/",
              "/tmp"
            ],
            "measurement": [
              {"name": "free", "rename": "DISK_FREE", "unit": "Gigabytes"},
              "total",
              "used"
            ],
            "metrics_collection_interval": 60,
            "append_dimensions": {
              "customized_dimension_key_3": "customized_dimension_value_3",
              "customized_dimension_key_4": "customized_dimension_value_4"
            }
          },
          "diskio": {
            "resources": [
              "*"
            ],
            "measurement": [
              "reads",
              "writes",
              "read_time",
              "write_time",
              "io_time"
            ],
            "metrics_collection_interval": 60
          },
          "swap": {
            "measurement": [
              "swap_used",
              "swap_free",
              "swap_used_percent"
            ]
          },
          "mem": {
            "measurement": [
              "mem_used",
              "mem_cached",
              "mem_total"
            ],
            "metrics_collection_interval": 1
          },
          "net": {
            "resources": [
              "eth0"
            ],
            "measurement": [
              "bytes_sent",
              "bytes_recv",
              "drop_in",
              "drop_out"
            ]
          },
          "netstat": {
            "measurement": [
              "tcp_established",
              "tcp_syn_sent",
              "tcp_close"
            ],
            "metrics_collection_interval": 60
          },
          "processes": {
            "measurement": [
              "running",
              "sleeping",
              "dead"
            ]
          }
        },
        "append_dimensions": {
          "ImageId": "${aws:ImageId}",
          "InstanceId": "${aws:InstanceId}",
          "InstanceType": "${aws:InstanceType}",
          "AutoScalingGroupName": "${aws:AutoScalingGroupName}"
        },
        "aggregation_dimensions" : [["ImageId"], ["InstanceId", "InstanceType"], ["d1"],[]]
      },
      "logs": {
        "logs_collected": {
          "files": {
            "collect_list": [
              {
                "file_path": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
                "log_group_name": "amazon-cloudwatch-agent.log",
                "log_stream_name": "amazon-cloudwatch-agent.log",
                "timezone": "UTC"
              },
              {
                "file_path": "/opt/aws/amazon-cloudwatch-agent/logs/test.log",
                "log_group_name": "test.log",
                "log_stream_name": "test.log",
                "timezone": "Local"
              }
            ]
          }
        },
        "log_stream_name": "my_log_stream_name"
      }
    }
```

The following is an example of a complete agent configuration file for a server running Windows Server\.

```
{
      "agent": {
        "metrics_collection_interval": 60,
        "logfile": "c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log"
      },
      "metrics": {
        "metrics_collected": {
          "Processor": {
            "measurement": [
              {"name": "% Idle Time", "rename": "CPU_IDLE", "unit": "Percent"},
              "% Interrupt Time",
              "% User Time",
              "% Processor Time"
            ],
            "resources": [
              "*"
            ],
            "append_dimensions": {
              "customized_dimension_key_1": "customized_dimension_value_1",
              "customized_dimension_key_2": "customized_dimension_value_2"
            }
          },
          "LogicalDisk": {
            "measurement": [
              {"name": "% Idle Time", "unit": "Percent"},
              {"name": "% Disk Read Time", "rename": "DISK_READ"},
              "% Disk Write Time"
            ],
            "resources": [
              "*"
            ]
          },
          "customizedObjectName": {
            "metrics_collection_interval": 60,
            "customizedCounterName": [
              "metric1",
              "metric2"
            ],
            "resources": [
              "customizedInstaces"
            ]
          },
          "Memory": {
            "metrics_collection_interval": 5,
            "measurement": [
              "Available Bytes",
              "Cache Faults/sec",
              "Page Faults/sec",
              "Pages/sec"
            ]
          },
          "Network Interface": {
            "metrics_collection_interval": 5,
            "measurement": [
              "Bytes Received/sec",
              "Bytes Sent/sec",
              "Packets Received/sec",
              "Packets Sent/sec"
            ],
            "resources": [
              "*"
            ],
            "append_dimensions": {
              "customized_dimension_key_3": "customized_dimension_value_3"
            }
          },
          "System": {
            "measurement": [
              "Context Switches/sec",
              "System Calls/sec",
              "Processor Queue Length"
            ]
          }
        },
        "append_dimensions": {
          "ImageId": "${aws:ImageId}",
          "InstanceId": "${aws:InstanceId}",
          "InstanceType": "${aws:InstanceType}",
          "AutoScalingGroupName": "${aws:AutoScalingGroupName}"
        },
        "aggregation_dimensions" : [["ImageId"], ["InstanceId", "InstanceType"], ["d1"],[]]
      },
      "logs": {
        "logs_collected": {
          "files": {
            "collect_list": [
              {
                "file_path": "c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log",
                "log_group_name": "amazon-cloudwatch-agent.log",
                "timezone": "UTC"
              },
              {
                "file_path": "c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\test.log",
                "log_group_name": "test.log",
                "timezone": "Local"
              }
            ]
          },
          "windows_events": {
            "collect_list": [
              {
                "event_name": "System",
                "event_levels": [
                  "INFORMATION",
                  "SUCCESS"
                ],
                "log_group_name": "System",
                "log_stream_name": "System",
                "event_format": "xml"
              },
              {
                "event_name": "Application",
                "event_levels": [
                  "WARNING",
                  "ERROR"
                ],
                "log_group_name": "Application",
                "log_stream_name": "Application",
                "event_format": "xml"
              }
            ]
          }
        },
        "log_stream_name": "example_log_stream_name"
      }
    }
```

## Upload the CloudWatch Agent Configuration File to Systems Manager Parameter Store<a name="Upload-CloudWatch-Agent-Configuration-To-Parameter-Store"></a>

If you are installing the CloudWatch agent on an Amazon EC2 instance or on an on\-premises server that has the SSM Agent installed, then after you manually edit the CloudWatch agent configuration file you must upload it to Systems Manager Parameter Store\. To do so, you use the Systems Manager `put-parameter` command\.

To be able to store the file in Parameter Store, you must use an IAM role with sufficient permissions\. For more information, see [Create IAM Roles and Users for Use With CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)\.

Use the following command, where `parameter name` is the name to be used for this file in Parameter Store, and `configuration_file_pathname` is the path and filename of the configuration file you have edited\.

```
aws ssm put-parameter --name "parameter name" --type "String" --value file://configuration_file_pathname
```