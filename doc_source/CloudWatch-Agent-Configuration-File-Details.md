# Manually Create or Edit the CloudWatch Agent Configuration File<a name="CloudWatch-Agent-Configuration-File-Details"></a>

The CloudWatch agent configuration file is a JSON file with three sections: `agent`, `metrics`, and `logs`\.
+ The `agent` section includes fields for the overall configuration of the agent\. If you use the wizard, it doesn't create an `agent` section\. 
+ The `metrics` section specifies the custom metrics for collection and publishing to CloudWatch\. If you're using the agent only to collect logs, you can omit the `metrics` section from the file\.
+ The `logs` section specifies what log files are published to CloudWatch Logs\. This can include events from the Windows Event Log if the server runs Windows Server\.

The following sections explain the structure and fields of this JSON file\. You can also view the schema definition for this configuration file\. The schema definition is located at `installation-directory/doc/amazon-cloudwatch-agent-schema.json` on Linux servers, and at `installation-directory/amazon-cloudwatch-agent-schema.json` on servers running Windows Server\.

If you create or edit the agent configuration file manually, you can give it any name\. For simplicity in troubleshooting, we recommend that you name it `/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json` on a Linux server and `$Env:ProgramData\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent.json` on servers running Windows Server\. After you have created the file, you can copy it to other servers where you want to install the agent\.

## CloudWatch Agent Configuration File: Agent Section<a name="CloudWatch-Agent-Configuration-File-Agentsection"></a>

The `agent` section can include the following fields\. The wizard doesn't create an `agent` section\. Instead, the wizard omits it and uses the default values for all fields in this section\.
+ `metrics_collection_interval` – Optional\. Specifies how often all metrics specified in this configuration file are to be collected\. You can override this value for specific types of metrics\.

  This value is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

  If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 

  The default value is 60\. 
+ `region` – Specifies the Region to use for the CloudWatch endpoint when an Amazon EC2 instance is being monitored\. The metrics collected are sent to this Region, such as `us-west-1`\. If you omit this field, the agent sends metrics to the Region where the Amazon EC2 instance is located\.

  If you are monitoring an on\-premises server, this field isn't used, and the agent reads the Region from the `AmazonCloudWatchAgent` profile of the AWS configuration file\.
+ `credentials` – Specifies an IAM role to use when sending metrics and logs to a different AWS account\. If specified, this field contains one parameter, `role_arn`\.
  + `role_arn` – Specifies the Amazon Resource Name \(ARN\) of an IAM role to use for authentication when sending metrics and logs to a different AWS account\. For more information, see [Sending Metrics and Logs to a Different Account](CloudWatch-Agent-common-scenarios.md#CloudWatch-Agent-send-to-different-AWS-account)\.
+ `debug` – Optional\. Specifies running the CloudWatch agent with debug log messages\. The default value is `false`\. 
+ `logfile` – Specifies the location where the CloudWatch agent writes log messages\. If you specify an empty string, the log goes to stderr\. If you don't specify this option, the default locations are the following:
  + Linux: `/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log`
  + Windows Server: `c:\\ProgramData\\Amazon\\CloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log` 

  The CloudWatch agent automatically rotates the log file that it creates\. A log file is rotated out when it reaches 100 MB in size\. The agent keeps the rotated log files for up to seven days, and it keeps as many as five backup log files that have been rotated out\. Backup log files have a timestamp appended to their filename\. The timestamp shows the date and time that the file was rotated out: for example, `amazon-cloudwatch-agent-2018-06-08T21-01-50.247.log.gz`\.
+ `omit_hostname` – Optional\. By default, the hostname is published as a dimension of metrics that are collected by the agent\. Set this value to `true` to prevent the hostname from being published as a dimension\. The default value is `false`\. 
+ `run_as_user` – Optional\. Specifies a user to use to run the CloudWatch agent\. If you don't specify this parameter, the root user is used\. This option is valid only on Linux servers\.

  If you specify this option, the user must exist before you start the CloudWatch agent\. For more information, see [Running the CloudWatch Agent as a Different User](CloudWatch-Agent-common-scenarios.md#CloudWatch-Agent-run-as-user)\.

The following is an example of an `agent` section\.

```
"agent": {
   "metrics_collection_interval": 60,
   "region": "us-west-1",
   "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
   "debug": false,
   "run_as_user": "cwagent"
  }
```

## CloudWatch Agent Configuration File: Metrics Section<a name="CloudWatch-Agent-Configuration-File-Metricssection"></a>

On servers running either Linux or Windows Server, the `metrics` section includes the following fields:
+ `namespace` – Optional\. The namespace to use for the metrics collected by the agent\. The default value is `CWAgent`\. The maximum length is 255 characters\. The following is an example:

  ```
  {
    "metrics": {
      "namespace": "Development/Product1Metrics",
     ......
     },
  }
  ```
+ `append_dimensions` – Optional\. Adds Amazon EC2 metric dimensions to all metrics collected by the agent\. The only supported key\-value pairs are shown in the following list\. Any other key\-value pairs are ignored\.
  + `"ImageID":"${aws:ImageId}"` sets the instance's AMI ID as the value of the `ImageID` dimension\.
  + `"InstanceId":"${aws:InstanceId}"` sets the instance's instance ID as the value of the `InstanceID` dimension\.
  + `"InstanceType":"${aws:InstanceType}"` sets the instance's instance type as the value of the `InstanceType` dimension\.
  + `"AutoScalingGroupName":"${aws:AutoScalingGroupName}"` sets the instance's Auto Scaling group name as the value of the `AutoScalingGroupName` dimension\.

  If you want to append dimensions to metrics with arbitrary key\-value pairs, use the `append_dimensions` parameter in the field for that particular type of metric\.

  If you specify a value that depends on Amazon EC2 metadata and you use proxies, you must make sure that the server can access the endpoint for Amazon EC2\. For more information about these endpoints, see [Amazon Elastic Compute Cloud \(Amazon EC2\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region) in the *Amazon Web Services General Reference*\.
+ `aggregation_dimensions` – Optional\. Specifies the dimensions that collected metrics are to be aggregated on\. For example, if you roll up metrics on the `AutoScalingGroupName` dimension, the metrics from all instances in each Auto Scaling group are aggregated and can be viewed as a whole\.

  You can roll up metrics along single or multiple dimensions\. For example, specifying `[["InstanceId"], ["InstanceType"], ["InstanceId","InstanceType"]]` aggregates metrics for instance ID singly, instance type singly, and for the combination of the two dimensions\.

  You can also specify `[]` to roll up all metrics into one collection, disregarding all dimensions\.
+ `endpoint_override` – Specifies a FIPS endpoint or private link to use as the endpoint where the agent sends metrics\. Specifying this and setting a private link enables you to send the metrics to an Amazon VPC endpoint\. For more information, see [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)\. 

  The value of `endpoint_override` must be a string that is a URL\.

  For example, the following part of the metrics section of the configuration file sets the agent to use a VPC Endpoint when sending metrics\. 

  ```
  {
    "metrics": {
      "endpoint_override": "vpce-XXXXXXXXXXXXXXXXXXXXXXXXX.monitoring.us-east-1.vpce.amazonaws.com",
     ......
     },
  }
  ```
+ `metrics_collected` – Required\. Specifies which metrics are to be collected, including custom metrics collected through `StatsD` or `collectd`\. This section includes several subsections\. 

  The contents of the `metrics_collected` section depend on whether this configuration file is for a server running Linux or Windows Server\.
+ `force_flush_interval` – Specifies in seconds the maximum amount of time that metrics remain in the memory buffer before being sent to the server\. No matter the setting for this, if the size of the metrics in the buffer reaches 40 KB or 20 different metrics, the metrics are immediately sent to the server\.

  The default value is 60\.
+ `credentials` – Specifies an IAM role to use when sending metrics to a different account\. If specified, this field contains one parameter, `role_arn`\.
  + `role_arn` – Specifies the ARN of an IAM role to use for authentication when sending metrics to a different account\. For more information, see [Sending Metrics and Logs to a Different Account](CloudWatch-Agent-common-scenarios.md#CloudWatch-Agent-send-to-different-AWS-account)\. If specified here, this value overrides the `role_arn` specified in the `agent` section of the configuration file, if any\.

### Linux<a name="CloudWatch-Agent-Linux-section"></a>

On servers running Linux, the metrics\_collected section of the configuration file can also contain the following fields\.

 Many of these fields can include a `measurement` sections that lists the metrics you want to collect for that resource\. These `measurement` sections can either specify the complete metric name such as `swap_used`, or just the part of the metric name that will be appended to the type of resource\. For example, specifying `reads` in the `measurement` section of the `diskio` section causes the `diskio_reads` metric to be collected\.
+ `collectd` – Optional\. Specifies that you want to retrieve custom metrics using the `collectd` protocol\. You use `collectd` software to send the metrics to the CloudWatch agent\. For more information about the configuration options available for collectd, see [Retrieve Custom Metrics with collectd](CloudWatch-Agent-custom-metrics-collectd.md)\. 
+ `cpu` – Optional\. Specifies that CPU metrics are to be collected\. This section is valid only for Linux instances\. You must include at least one of the `resources` and `totalcpu` fields for any CPU metrics to be collected\. This section can include the following fields:
  + `resources` – Optional\. Specify this field with a value of `*` to cause per\-cpu metrics are to be collected\. The only allowed value is `*`\. 
  + `totalcpu` – Optional\. Specifies whether to report cpu metrics aggregated across all cpu cores\. The default is true\.
  + `measurement` – Specifies the array of cpu metrics to be collected\. Possible values are `time_active`, `time_guest`, `time_guest_nice`, `time_idle`, `time_iowait`, `time_irq`, `time_nice`, `time_softirq`, `time_steal`, `time_system`, `time_user`, `usage_active`, `usage_guest`, `usage_guest_nice`, `usage_idle`, `usage_iowait`, `usage_irq`, `usage_nice`, `usage_softirq`, `usage_steal`, `usage_system`, and `usage_user`\. This field is required if you include `cpu`\.

    By default, the unit for `cpu_usage_*` metrics is `Percent`, and `cpu_time_*` metrics don't have a unit\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the cpu metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the cpu metrics\. If you specify this field, it's used in addition to dimensions specified in the global `append_dimensions` field that is used for all types of metrics that the agent collects\.
+ `disk` – Optional\. Specifies that disk metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as two fields:
  + `resources` – Optional\. Specifies an array of disk mount points\. This field limits CloudWatch to collect metrics from only the listed mount points\. You can specify `*` as the value to collect metrics from all mount points\. The default value is to collect metrics from all mount points\. 
  + `measurement` – Specifies the array of disk metrics to be collected\. Possible values are `free`, `total`, `used`, `used_percent`, `inodes_free`, `inodes_used`, and `inodes_total`\. This field is required if you include `disk`\.
**Note**  
The `disk` metrics have a dimension for `Partition`, which means that the number of custom metrics generated is dependent on the number of partitions associated with your instance\. The number of disk partitions you have depends on which AMI you are using and the number of Amazon EBS volumes you attach to the server\.

    To see the default units for each `disk` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](metrics-collected-by-CloudWatch-agent.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `ignore_file_system_types` – Specifies file system types to exclude when collecting disk metrics\. Valid values include `sysfs`, `devtmpfs`, and so on\.
  + `drop_device` – Setting this to `true` causes `Device` to not be included as a dimension for disk metrics\.

    Preventing `Device` from being used as a dimension can be useful on instances that use the Nitro system because on those instances the device names change for each disk mount when the instance is rebooted\. This can cause inconsistent data in your metrics and cause alarms based on these metrics to go to `INSUFFICIENT DATA` state\.

    The default is `false`\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the disk metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the disk metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.
+ `diskio` – Optional\. Specifies that disk i/o metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as two fields:
  + `resources` – Optional\. If you specify an array of devices, CloudWatch collects metrics from only those devices\. Otherwise, metrics for all devices are collected\. You can also specify \* as the value to collect metrics from all devices\.
  + `measurement` – Specifies the array of diskio metrics to be collected\. Possible values are `reads`, `writes`, `read_bytes`, `write_bytes`, `read_time`, `write_time`, `io_time`, and `iops_in_progress`\. This field is required if you include `diskio`\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the diskio metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the diskio metrics\. If you specify this field, it is used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.
+ `swap` – Optional\. Specifies that swap memory metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as three fields:
  + `measurement` – Specifies the array of swap metrics to be collected\. Possible values are `free`, `used`, and `used_percent`\. This field is required if you include `swap`\.

    To see the default units for each `swap` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](metrics-collected-by-CloudWatch-agent.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the swap metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\. 

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the swap metrics\. If you specify this field, it is used in addition to dimensions specified in the global `append_dimensions` field that is used for all types of metrics collected by the agent\. It's collected as a high\-resolution metric\. 
+ `mem` – Optional\. Specifies that memory metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as three fields:
  + `measurement` – Specifies the array of memory metrics to be collected\. Possible values are `active`, `available`, `available_percent`, `buffered`, `cached`, `free`, `inactive`, `total`, `used`, and `used_percent`\. This field is required if you include `mem`\.

    To see the default units for each `mem` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](metrics-collected-by-CloudWatch-agent.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the mem metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the mem metrics\. If you specify this field, it's used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics that the agent collects\.
+ `net` – Optional\. Specifies that networking metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as four fields:
  + `resources` – Optional\. If you specify an array of network interfaces, CloudWatch collects metrics from only those interfaces\. Otherwise, metrics for all devices are collected\. You can also specify `*` as the value to collect metrics from all interfaces\.
  + `measurement` – Specifies the array of networking metrics to be collected\. Possible values are `bytes_sent`, `bytes_recv`, `drop_in`, `drop_out`, `err_in`, `err_out`, `packets_sent`, and `packets_recv`\. This field is required if you include `net`\.

    To see the default units for each `net` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](metrics-collected-by-CloudWatch-agent.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the net metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the net metrics\. If you specify this field, it's used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.
+ `netstat` – Optional\. Specifies that TCP connection state and UDP connection metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as three fields:
  + `measurement` – Specifies the array of netstat metrics to be collected\. Possible values are `tcp_close`, `tcp_close_wait`, `tcp_closing`, `tcp_established`, `tcp_fin_wait1`, `tcp_fin_wait2`, `tcp_last_ack`, `tcp_listen`, `tcp_none`, `tcp_syn_sent`, `tcp_syn_recv`, `tcp_time_wait`, and `udp_socket`\. This field is required if you include `netstat`\.

    To see the default units for each `netstat` metric, see [Metrics Collected by the CloudWatch Agent on Linux Instances](metrics-collected-by-CloudWatch-agent.md#linux-metrics-enabled-by-CloudWatch-agent)\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the netstat metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information about high\-resolution metrics, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\.
  + `append_dimensions` – Optional\. Additional dimensions to use for only the netstat metrics\. If you specify this field, it's used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.
+ `processes` – Optional\. Specifies that process metrics are to be collected\. This section is valid only for Linux instances\. This section can include as many as three fields:
  + `measurement` – Specifies the array of processes metrics to be collected\. Possible values are `blocked`, `dead`, `idle`, `paging`, `running`, `sleeping`, `stopped`, `total`, `total_threads`, `wait`, and `zombies`\. This field is required if you include `processes`\.

    For all `processes` metrics, the default unit is `Count`\.

    Within the entry for each individual metric, you might optionally specify one or both of the following:
    + `rename` – Specifies a different name for this metric\.
    + `unit` – Specifies the unit to use for this metric, overriding the default unit for the metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + `metrics_collection_interval` – Optional\. Specifies how often to collect the processes metrics, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

    This value is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

    If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
  + `append_dimensions` – Optional\. Additional dimensions to use for only the process metrics\. If you specify this field, it's used in addition to dimensions specified in the `append_dimensions` field that is used for all types of metrics collected by the agent\.
+ `procstat` – Optional\. Specifies that you want to retrieve metrics from individual processes\. For more information about the configuration options available for procstat, see [Collect Process Metrics with the procstat Plugin](CloudWatch-Agent-procstat-process-metrics.md)\. 
+ `statsd` – Optional\. Specifies that you want to retrieve custom metrics using the `StatsD` protocol\. The CloudWatch agent acts as a daemon for the protocol\. You use any standard `StatsD` client to send the metrics to the CloudWatch agent\. For more information about the configuration options available for StatsD, see [Retrieve Custom Metrics with StatsD ](CloudWatch-Agent-custom-metrics-statsd.md)\. 

The following is an example of a `metrics` section for a Linux server\. In this example, three CPU metrics, three netstat metrics, three process metrics, and one disk metric are collected, and the agent is set up to receive additional metrics from a `collectd` client\.

```
  "metrics": {
    "metrics_collected": {
      "collectd": {},
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
       "disk": {
        "measurement": [
          "used_percent"
        ],
        "resources": [
          "*"
        ],
        "drop_device": true
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

In the `metrics_collected` section for Windows Server, you can have subsections for each Windows performance object, such as `Memory`, `Processor`, and `LogicalDisk`\. For information about what objects and counters are available, see the Microsoft Windows documentation\.

Within the subsection for each object, you specify a `measurement` array of the counters to collect\. The `measurement` array is required for each object that you specify in the configuration file\. You can also specify a `resources` field to name the instances to collect metrics from\. You can also specify `*` for `resources` to collect separate metrics for every instance\. If you omit `resources`, the data for all instances is aggregated into one set\. For objects that don't have instances, omit `resources`\.

Within each object section, you can also specify the following optional fields:
+ `metrics_collection_interval` – Optional\. Specifies how often to collect the metrics for this object, overriding the global `metrics_collection_interval` specified in the `agent` section of the configuration file\.

  This value is specified in seconds\. For example, specifying 10 sets metrics to be collected every 10 seconds, and setting it to 300 specifies metrics to be collected every 5 minutes\.

  If you set this value below 60 seconds, each metric is collected as a high\-resolution metric\. For more information, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\. 
+ `append_dimensions` – Optional\. Specifies additional dimensions to use for only the metrics for this object\. If you specify this field, it's used in addition to dimensions specified in the global `append_dimensions` field that is used for all types of metrics collected by the agent\. 

Within each counter section, you can also specify the following optional fields:
+ `rename` – Specifies a different name to be used in CloudWatch for this metric\.
+ `unit` – Specifies the unit to use for this metric\. The unit that you specify must be a valid CloudWatch metric unit, as listed in the `Unit` description in [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.

There are two other optional sections that you can include in `metrics_collected`:
+ `statsd` – Enables you to retrieve custom metrics using the `StatsD` protocol\. The CloudWatch agent acts as a daemon for the protocol\. You use any standard `StatsD` client to send the metrics to the CloudWatch agent\. For more information, see [Retrieve Custom Metrics with StatsD ](CloudWatch-Agent-custom-metrics-statsd.md)\.
+ `procstat` – Enables you to retrieve metrics from individual processes\. For more information, see [Collect Process Metrics with the procstat Plugin](CloudWatch-Agent-procstat-process-metrics.md)\.

The following is an example `metrics` section for use on Windows Server\. In this example, many Windows metrics are collected, and the computer is also set to receive additional metrics from a `StatsD` client\.

```
"metrics": {
    "metrics_collected": {
      "statsd": {},
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
    "aggregation_dimensions" : [["ImageId"], ["InstanceId", "InstanceType"], ["d1"],[]]
    }
  }
```

## CloudWatch Agent Configuration File: Logs Section<a name="CloudWatch-Agent-Configuration-File-Logssection"></a>

The `logs` section includes the following fields:
+ `logs_collected` – Required if the `logs` section is included\. Specifies which log files and Windows event logs are to be collected from the server\. It can include two fields, `files` and `windows_events`\.
  + `files` – Specifies which regular log files the CloudWatch agent is to collect\. It contains one field, `collect_list`, which further defines these files\.
    + `collect_list` – Required if `files` is included\. Contains an array of entries, each of which specifies one log file to collect\. Each of these entries can include the following fields:
      + `file_path` – Specifies the path of the log file to upload to CloudWatch Logs\. Standard Unix glob matching rules are accepted, with the addition of `**` as a *super asterisk*\. For example, specifying `/var/log/**.log` causes all `.log` files in the `/var/log` directory tree to be collected\. For more examples, see [Glob Library](https://github.com/gobwas/glob)\.

        You can also use the standard asterisk as a standard wildcard\. For example, `/var/log/system.log*` matches files such as `system.log_1111`, `system.log_2222`, and so on in `/var/log`\.

        Only the latest file is pushed to CloudWatch Logs based on file modification time\. We recommend that you use wildcards to specify a series of files of the same type, such as `access_log.2018-06-01-01` and `access_log.2018-06-01-02`, but not multiple kinds of files, such as `access_log_80` and `access_log_443`\. To specify multiple kinds of files, add another log stream entry to the agent configuration file so that each kind of log file goes to a different log stream\.
      + `auto_removal` – Optional\. If this is true, the CloudWatch agent automatically removes old log files after they are uploaded to CloudWatch Logs\. The agent only removes complete files from logs that create multiple files, such as logs that create separate files for each date\. If a log continuously writes to a single file, it is not removed\.

        If you already have a log file rotation or removal method in place, we recommend that you omit this field or set it to `false`\.

        If you omit this field, the default value of `false` is used\.
      + `log_group_name` – Optional\. Specifies what to use as the log group name in CloudWatch Logs\. As part of the name, you can use `{instance_id}`, `{hostname}`, `{local_hostname}`, and `{ip_address}` as variables within the name\. `{hostname}` retrieves the hostname from the EC2 metadata, and `{local_hostname}` uses the hostname from the network configuration file\.

        If you use these variables to create many different log groups, keep in mind the limit of 5000 log groups per account per Region\.

        Allowed characters include a–z, A–Z, 0–9, '\_' \(underscore\), '\-' \(hyphen\), '/' \(forward slash\), and '\.' \(period\)\.

        We recommend that you specify this field to prevent confusion\. If you omit this field, the file path up to the final dot is used as the log group name\. For example, if the file path is `/tmp/TestLogFile.log.2017-07-11-14`, the log group name is `/tmp/TestLogFile.log`\. 
      + `log_stream_name` – Optional\. Specifies what to use as the log stream name in CloudWatch Logs\. As part of the name, you can use `{instance_id}`, `{hostname}`, `{local_hostname}`, and `{ip_address}` as variables within the name\. `{hostname}` retrieves the hostname from the EC2 metadata, and `{local_hostname}` uses the hostname from the network configuration file\.

        If you omit this field, the value of the `log_stream_name` parameter in the global `logs` section is used\. If that is also omitted, the default value of `{instance_id}` is used\.

        If a log stream doesn't already exist, it's created automatically\.
      + `timezone` – Optional\. Specifies the time zone to use when putting timestamps on log events\. The valid values are `UTC` and `Local`\. The default value is `Local`\.

        This parameter is ignored if you don't specify a value for `timestamp_format`\.
      + `timestamp_format` – Optional\. Specifies the timestamp format, using plaintext and special symbols that start with %\. If you omit this field, the current time is used\. If you use this field, you can use the symbols in the following list as part of the format\.

        If a single log entry contains two time stamps that match the format, the first time stamp is used\.

        This list of symbols is different than the list used by the older CloudWatch Logs agent\. For a summary of these differences, see [Timestamp Differences Between the Unified CloudWatch Agent and the Older CloudWatch Logs Agent](CloudWatch-Agent-common-scenarios.md#CloudWatch-Agent-logs-timestamp-differences)  
`%y`  
Year without century as a zero\-padded decimal number\. For example, `19` to represent 2019\.  
`%Y`  
Year with century as a decimal number\. For example, `2019`\.  
`%b`  
Month as the locale's abbreviated name  
`%B`  
Month as the locale's full name  
`%m`  
Month as a zero\-padded decimal number  
`%-m`  
Month as a decimal number \(not zero\-padded\)  
`%d`  
Day of the month as a zero\-padded decimal number  
`%-d`  
Day of the month as a decimal number \(not zero\-padded\)  
`%A`  
Full name of weekday, such as `Monday`  
`%a`  
Abbreviation of weekday, such as `Mon`  
`%H`  
Hour \(in a 24\-hour clock\) as a zero\-padded decimal number  
`%I`  
Hour \(in a 12\-hour clock\) as a zero\-padded decimal number  
`%-I`  
Hour \(in a 12\-hour clock\) as a decimal number \(not zero\-padded\)  
`%p`  
AM or PM  
`%M`  
Minutes as a zero\-padded decimal number  
`%-M`  
Minutes as a decimal number \(not zero\-padded\)  
`%S`  
Seconds as a zero\-padded decimal number  
`%-S`  
Seconds as a decimal number \(not zero padded\)  
`%f`  
Fractional seconds as a decimal number \(1\-9 digits\), zero\-padded on the left\.  
`%Z`  
Time zone, for example `PST`  
`%z`  
Time zone, expressed as the offset between the local time zone and UTC\. For example, `-0700`\. Only this format is supported\. For example, `-07:00` isn't a valid format\.
      + `multi_line_start_pattern` – Specifies the pattern for identifying the start of a log message\. A log message is made of a line that matches the pattern and any subsequent lines that don't match the pattern\.

        If you omit this field, multi\-line mode is disabled, and any line that begins with a non\-whitespace character closes the previous log message and starts a new log message\.

        If you include this field, you can specify `{timestamp_format}` to use the same regular expression as your timestamp format\. Otherwise, you can specify a different regular expression for CloudWatch Logs to use to determine the start lines of multi\-line entries\.
      + `encoding` – Specified the encoding of the log file so that it can be read correctly\. If you specify an incorrect coding, there might be data loss because characters that can't be decoded are replaced with other characters\.

        The default value is `utf-8`\. The following are all possible values:

         `ascii, big5, euc-jp, euc-kr, gbk, gb18030, ibm866, iso2022-jp, iso8859-2, iso8859-3, iso8859-4, iso8859-5, iso8859-6, iso8859-7, iso8859-8, iso8859-8-i, iso8859-10, iso8859-13, iso8859-14, iso8859-15, iso8859-16, koi8-r, koi8-u, macintosh, shift_jis, utf-8, utf-16, windows-874, windows-1250, windows-1251, windows-1252, windows-1253, windows-1254, windows-1255, windows-1256, windows-1257, windows-1258, x-mac-cyrillic` 
  + The `windows_events` section specifies the type of Windows events to collect from servers running Windows Server\. It includes the following fields:
    + `collect_list` – Required if `windows_events` is included\. Specifies the types and levels of Windows events to be collected\. Each log to be collected has an entry in this section, which can include the following fields:
      + `event_name` – Specifies the type of Windows events to log\. This is equivalent to the Windows event log channel name: for example, `System`, `Security`, `Application`, and so on\. This field is required for each type of Windows event to log\.
**Note**
When CloudWatch retrieves messages from a Windows log channel, it looks up that log channel based on its `Full Name` property\.  Meanwhile, the Windows Event Viewer navigation pane displays the `Log Name` property of log channels\.  The `Full Name` and `Log Name` do not always match\.  To confirm the `Full Name` of a log channel, right-click on it in the Windows Event viewer and open the Properties dialog\.
      + `event_levels` – Specifies the levels of event to log\. You must specify each level to log\. Possible values include `INFORMATION`, `WARNING`, `ERROR`, `CRITICAL`, and `VERBOSE`\. This field is required for each type of Windows event to log\.
      + `log_group_name` – Required\. Specifies what to use as the log group name in CloudWatch Logs\. 
      + `log_stream_name` – Optional\. Specifies what to use as the log stream name in CloudWatch Logs\. As part of the name, you can use `{instance_id}`, `{hostname}`, `{local_hostname}`, and `{ip_address}` as variables within the name\. `{hostname}` retrieves the hostname from the EC2 metadata, and `{local_hostname}` uses the hostname from the network configuration file\.

        If you omit this field, the value of the `log_stream_name` parameter in the global `logs` section is used\. If that is also omitted, the default value of `{instance_id}` is used\.

        If a log stream doesn't already exist, it's created automatically\.
      + `event_format` – Optional\. Specifies the format to use when storing Windows events in CloudWatch Logs\. `xml` uses the XML format as in Windows Event Viewer\. `text` uses the legacy CloudWatch Logs agent format\.
+ `log_stream_name` – Required\. Specifies the default log stream name to be used for any logs or Windows events that don't have individual log stream names defined in the `log_stream_name` parameter within their entry in `collect_list`\.
+ `endpoint_override` – Specifies a FIPS endpoint or private link to use as the endpoint where the agent sends logs\. Specifying this field and setting a private link enables you to send the logs to an Amazon VPC endpoint\. For more information, see [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)\. 

  The value of `endpoint_override` must be a string that is a URL\.

  For example, the following part of the logs section of the configuration file sets the agent to use a VPC Endpoint when sending logs\. 

  ```
  {
    "logs": {
      "endpoint_override": "vpce-XXXXXXXXXXXXXXXXXXXXXXXXX.logs.us-east-1.vpce.amazonaws.com",
     ......
     },
  }
  ```
+ `force_flush_interval` – Specifies in seconds the maximum amount of time that logs remain in the memory buffer before being sent to the server\. No matter the setting for this field, if the size of the logs in the buffer reaches 1 MB, the logs are immediately sent to the server\. The default value is 5\.
+ `credentials` – Specifies an IAM role to use when sending logs to a different AWS account\. If specified, this field contains one parameter, `role_arn`\.
  + `role_arn` – Specifies the ARN of an IAM role to use for authentication when sending logs to a different AWS account\. For more information, see [Sending Metrics and Logs to a Different Account](CloudWatch-Agent-common-scenarios.md#CloudWatch-Agent-send-to-different-AWS-account)\. If specified here, this overrides the `role_arn` specified in the `agent` section of the configuration file, if any\.
+ `metrics_collected` – Specifies that the agent is to collect metrics embedded in logs\. Currently, the `metrics_collected` field can contain only the `emf` field\.
  + `emf` – Specifies that the agent is to collect logs that are in embedded metric format\. You can generate metric data from these logs\. For more information, see [Ingesting High\-Cardinality Logs and Generating Metrics with CloudWatch Embedded Metric Format](CloudWatch_Embedded_Metric_Format.md)\.

The following is an example of a `logs` section\.

```
"logs":
   {
       "logs_collected": {
           "files": {
               "collect_list": [
                   {
                       "file_path": "c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log",
                       "log_group_name": "amazon-cloudwatch-agent.log",
                       "log_stream_name": "my_log_stream_name_1",
                       "timestamp_format": "%H: %M: %S%y%b%-d"
                   },
                   {
                       "file_path": "c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\test.log",
                       "log_group_name": "test.log",
                       "log_stream_name": "my_log_stream_name_2"
                   }
               ]
           },
           "windows_events": {
               "collect_list": [
                   {
                       "event_name": "System",
                       "event_levels": [
                           "INFORMATION",
                           "ERROR"
                       ],
                       "log_group_name": "System",
                       "log_stream_name": "System"
                   },
                   {
                       "event_name": "CustomizedName",
                       "event_levels": [
                           "INFORMATION",
                           "ERROR"
                       ],
                       "log_group_name": "CustomizedLogGroup",
                       "log_stream_name": "CustomizedLogStream"
                   }
               ]
           }
       },
       "log_stream_name": "my_log_stream_name"
}
```

## How the CloudWatch Agent Handles Sparse Log Files<a name="CloudWatch-Agent-sparse-log-files"></a>

Sparse files are files with both empty blocks and real contents\. A sparse file uses disk space more efficiently by writing brief information representing the empty blocks to disk instead of the actual null bytes which makes up the block\. This makes the actual size of a sparse file usually much smaller than its apparent size\.

However, the CloudWatch agent doesn’t treat sparse files differently than it treats normal files\. When the agent reads a sparse file, the empty blocks are treated as "real" blocks filled with null bytes\. Because of this, the CloudWatch agent publishes as many bytes as the apparent size of a sparse file to CloudWatch\. 

Configuring the CloudWatch agent to publish a sparse file can cause higher than expected CloudWatch costs, so we recommend not to do so\. For example, `/var/logs/lastlog` in Linux is usually a very sparse file, and we recommend that you don't publish it to CloudWatch\. 

## CloudWatch Agent Configuration File: Complete Examples<a name="CloudWatch-Agent-Configuration-File-Complete-Example"></a>

The following is an example of a complete CloudWatch agent configuration file for a Linux server\.

The items listed in the `measurement` sections for the metrics you want to collect can either specify the complete metric name such or just the part of the metric name that will be appended to the type of resource\. For example, specifying either `reads` or `diskio_reads` in the `measurement` section of the `diskio` section will cause the `diskio_reads` metric to be collected\.

This example includes both ways of specifying metrics in the `measurement` section\.

```
    {
      "agent": {
        "metrics_collection_interval": 10,
        "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log"
      },
      "metrics": {
        "namespace": "MyCustomNamespace",
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
             "ignore_file_system_types": [
              "sysfs", "devtmpfs"
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
        "aggregation_dimensions" : [["ImageId"], ["InstanceId", "InstanceType"], ["d1"],[]],
        "force_flush_interval" : 30
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
        "log_stream_name": "my_log_stream_name",
        "force_flush_interval" : 15
      }
    }
```

The following is an example of a complete CloudWatch agent configuration file for a server running Windows Server\.

```
{
      "agent": {
        "metrics_collection_interval": 60,
        "logfile": "c:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\Logs\\amazon-cloudwatch-agent.log"
      },
      "metrics": {
        "namespace": "MyCustomNamespace",
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
                  "ERROR"
                ],
                "log_group_name": "System",
                "log_stream_name": "System",
                "event_format": "xml"
              },
              {
                "event_name": "CustomizedName",
                "event_levels": [
                  "WARNING",
                  "ERROR"
                ],
                "log_group_name": "CustomizedLogGroup",
                "log_stream_name": "CustomizedLogStream",
                "event_format": "xml"
              }
            ]
          }
        },
        "log_stream_name": "example_log_stream_name"
      }
    }
```

## Save the CloudWatch Agent Configuration File Manually<a name="Saving-Agent-Configuration-File"></a>

If you create or edit the CloudWatch agent configuration file manually, you can give it any name\. For simplicity in troubleshooting, we recommend that you name it `/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json` on a Linux server and `$Env:ProgramData\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent.json` on servers running Windows Server\. After you have created the file, you can copy it to other servers where you want to run the agent\.

## Uploading the CloudWatch Agent Configuration File to Systems Manager Parameter Store<a name="Upload-CloudWatch-Agent-Configuration-To-Parameter-Store"></a>

If you plan to use the SSM Agent to install the CloudWatch agent on servers, after you manually edit the CloudWatch agent configuration file, you can upload it to Systems Manager Parameter Store\. To do so, use the Systems Manager `put-parameter` command\.

To be able to store the file in Parameter Store, you must use an IAM role with sufficient permissions\. For more information, see [Create IAM Roles and Users for Use with the CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)\.

Use the following command, where *parameter name* is the name to be used for this file in Parameter Store and *configuration\_file\_pathname* is the path and file name of the configuration file that you edited\.

```
aws ssm put-parameter --name "parameter name" --type "String" --value file://configuration_file_pathname
```
