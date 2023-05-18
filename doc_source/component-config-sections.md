# Component configuration sections<a name="component-config-sections"></a>

A component configuration includes several major sections\. Sections in a component configuration can be listed in any order\.
+ **alarmMetrics \(optional\)**

  A list of [metrics](#component-config-metric) to monitor for the component\. All component types can have an alarmMetrics section\. 
+ **logs \(optional\)**

  A list of [logs](#component-configuration-log) to monitor for the component\. Only EC2 instances can have a logs section\. 
+ **processes \(optional\)**

  A list of [processes](#component-configuration-process) to monitor for the component\. Only EC2 instances can have a processes section\. 
+ **subComponents \(optional\)**

  Nested instance and volume subComponent configuration for the component\. The following types of components can have nested instances and a subComponents section: ELB, ASG, custom\-grouped EC2 instances , and EC2 instances\.
+ **alarms \(optional\)**

  A list of [alarms](#component-configuration-alarms) to monitor for the component\. All component types can have an alarm section\.
+ **windowsEvents \(optional\)**

  A list of [windows events](#windows-events) to monitor for the component\. Only Windows on EC2 instances have a `windowsEvents` section\.
+ **JMXPrometheusExporter \(optional\)**

  JMXPrometheus Exporter configuration\.
+ **hanaPrometheusExporter \(optional\)**

  SAP HANA Prometheus Exporter configuration\.
+ **haClusterPrometheusExporter \(optional\)**

  HA Cluster Prometheus Exporter configuration\.
+ **netWeaverPrometheusExporter \(optional\) **

  SAP NetWeaver Prometheus Exporter configuration\.

The following example shows the syntax for the **subComponents section fragment** in JSON format\.

```
[
  {
    "subComponentType" : "AWS::EC2::Instance",
    "alarmMetrics" : [
      list of alarm metrics
    ],
    "logs" : [
      list of logs
    ],
    "processes": [
      list of processes
    ],
    "windowsEvents" : [
      list of windows events channels configurations
    ]
  },
  {
    "subComponentType" : "AWS::EC2::Volume",
    "alarmMetrics" : [
      list of alarm metrics
    ]
  }
]
```

## Component configuration section properties<a name="component-config-properties"></a>

This section describes the properties of each component configuration section\.

**Topics**
+ [Metric](#component-config-metric)
+ [Log](#component-configuration-log)
+ [Process](#component-configuration-process)
+ [JMX Prometheus Exporter](#component-configuration-prometheus)
+ [HANA Prometheus Exporter](#component-configuration-hana-prometheus)
+ [HA Cluster Prometheus Exporter](#component-configuration-ha-cluster-prometheus)
+ [NetWeaver Prometheus Exporter](#component-configuration-netweaver-prometheus)
+ [Windows Events](#windows-events)
+ [Alarm](#component-configuration-alarms)

### Metric<a name="component-config-metric"></a>

Defines a metric to be monitored for the component\.

**JSON** 

```
{
  "alarmMetricName" : "monitoredMetricName",
  "monitor" : true/false
}
```

**Properties**
+ **alarmMetricName \(required\)**

  The name of the metric to be monitored for the component\. For metrics supported by Application Insights, see [Logs and metrics supported by Amazon CloudWatch Application Insights](appinsights-logs-and-metrics.md)\. 
+ **monitor \(optional\)**

  Boolean to indicate whether to monitor the metric\. The default value is `true`\.

### Log<a name="component-configuration-log"></a>

Defines a log to be monitored for the component\.

**JSON** 

```
{
  "logGroupName" : "logGroupName",
  "logPath" : "logPath",
  "logType" : "logType",
  "encoding" : "encodingType",
  "monitor" : true/false
}
```

**Properties**
+ **logGroupName \(required\)**

  The CloudWatch log group name to be associated to the monitored log\. For the log group name constraints, see [CreateLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogGroup.html)\.
+ **logPath \(required for EC2 instance components; not required for components that do not use CloudWatch Agent, such as AWS Lambda\)**

  The path of the logs to be monitored\. The log path must be an absolute Windows system file path\. For more information, see [CloudWatch Agent Configuration File: Logs Section](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html#CloudWatch-Agent-Configuration-File-Logssection)\. 
+ **logType \(required\)**

  The log type decides the log patterns against which Application Insights analyzes the log\. The log type is selected from the following:
  + `SQL_SERVER`
  + `MYSQL`
  + `MYSQL_SLOW_QUERY`
  + `POSTGRESQL`
  + `ORACLE_ALERT`
  + `ORACLE_LISTENER`
  + `IIS`
  + `APPLICATION`
  + `WINDOWS_EVENTS`
  + `WINDOWS_EVENTS_ACTIVE_DIRECTORY`
  + `WINDOWS_EVENTS_DNS`
  + `WINDOWS_EVENTS_IIS`
  + `WINDOWS_EVENTS_SHAREPOINT`
  + `SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP`
  + `SQL_SERVER_FAILOVER_CLUSTER_INSTANCE`
  + `DEFAULT`
  + `CUSTOM`
  + `STEP_FUNCTION`
  + `API_GATEWAY_ACCESS`
  + `API_GATEWAY_EXECUTION`
  + `SAP_HANA_LOGS`
  + `SAP_HANA_TRACE`
  + `SAP_HANA_HIGH_AVAILABILITY`
  + `SAP_NETWEAVER_DEV_TRACE_LOGS`
  + `PACEMAKER_HIGH_AVAILABILITY`
+ **encoding \(optional\)**

  The type of encoding of the logs to be monitored\. The specified encoding should be included in the list of [CloudWatch agent supported encodings](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)\. If not provided, CloudWatch Application Insights uses the default encoding of type utf\-8, except for: 
  +  `SQL_SERVER`: utf\-16 encoding
  +  `IIS`: ascii encoding
+ **monitor \(optional\)**

  Boolean that indicates whether to monitor the logs\. The default value is `true`\.

### Process<a name="component-configuration-process"></a>

Defines a process to be monitored for the component\.

**JSON** 

```
{
  "processName" : "monitoredProcessName",
  "alarmMetrics" : [
      list of alarm metrics
  ] 
}
```

**Properties**
+ **processName \(required\)**

  The name of the process to be monitored for the component\. The process name must not contain a process stem, such as `sqlservr` or `sqlservr.exe`\.
+ **alarmMetrics \(required\)**

  A list of [metrics](#component-config-metric) to monitor for this process\. To view process metrics supported by CloudWatch Application Insights, see [ Amazon Elastic Compute Cloud \(EC2\) ](appinsights-metrics-ec2.md)\.

### JMX Prometheus Exporter<a name="component-configuration-prometheus"></a>

Defines the JMX Prometheus Exporter settings\.

**JSON** 

```
"JMXPrometheusExporter": {
  "jmxURL" : "JMX URL",
  "hostPort" : "The host and port",
  "prometheusPort" : "Target port to emit Prometheus metrics"
}
```

**Properties**
+ **jmxURL \(optional\)**

  A complete JMX URL to connect to\.
+ **hostPort \(optional\)**

  The host and port to connect to through remote JMX\. Only one of `jmxURL` and `hostPort` can be specified\.
+ **prometheusPort \(optional\)**

  The target port to send Prometheus metrics to\. If not specified, the default port 9404 is used\.

### HANA Prometheus Exporter<a name="component-configuration-hana-prometheus"></a>

Defines the HANA Prometheus Exporter settings\.

**JSON** 

```
"hanaPrometheusExporter": {
    "hanaSid": "SAP HANA  SID",
    "hanaPort": "HANA database port",
    "hanaSecretName": "HANA secret name",
    "prometheusPort": "Target port to emit Prometheus metrics"
}
```

**Properties**
+ **hanaSid**

  The three\-character SAP system ID \(SID\) of the SAP HANA system\.
+ **hanaPort**

  The HANA database port by which the exporter will query HANA metrics\.
+ **hanaSecretName**

  The AWS Secrets Manager secret that stores HANA monitoring user credentials\. The HANA Prometheus exporter uses these credentials to connect to the database and query HANA metrics\.
+ **prometheusPort \(optional\)**

  The target port to which Prometheus sends metrics\. If not specified, the default port 9668 is used\.

### HA Cluster Prometheus Exporter<a name="component-configuration-ha-cluster-prometheus"></a>

Defines the HA Cluster Prometheus Exporter settings\.

**JSON** 

```
"haClusterPrometheusExporter": {
    "prometheusPort": "Target port to emit Prometheus metrics"
}
```

**Properties**
+ **prometheusPort \(optional\)**

  The target port to which Prometheus sends metrics\. If not specified, the default port 9664 is used\.

### NetWeaver Prometheus Exporter<a name="component-configuration-netweaver-prometheus"></a>

Defines the NetWeaver Prometheus Exporter settings\.

**JSON** 

```
"netWeaverPrometheusExporter": {
    "sapSid": "SAP NetWeaver  SID",
    "instanceNumbers": [ "Array of instance Numbers of SAP NetWeaver system "],
"prometheusPort": "Target port to emit Prometheus metrics"
}
```

**Properties**
+ **sapSid**

  The 3 character SAP system ID \(SID\) of the SAP NetWeaver system\.
+ **instanceNumbers**

  Array of the instance Numbers of SAP NetWeaver system\.

  **Example: **`"instanceNumbers": [ "00", "01"]`
+ **prometheusPort \(optional\)**

  The target port to which to send Prometheus metrics\. If not specified, the default port `9680` is used\.

### Windows Events<a name="windows-events"></a>

Defines Windows Events to log\.

**JSON** 

```
{
  "logGroupName" : "logGroupName",
  "eventName" : "eventName",
  "eventLevels" : ["ERROR","WARNING","CRITICAL","INFORMATION","VERBOSE"],
  "monitor" : true/false
}
```

**Properties**
+ **logGroupName \(required\)**

  The CloudWatch log group name to be associated to the monitored log\. For the log group name constraints, see [CreateLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogGroup.html)\.
+ **eventName \(required\)**

  The type of Windows Events to log\. It is equivalent to the Windows Event log channel name\. For example, System, Security, CustomEventName, etc\. This field is required for each type of Windows event to log\. 
+ **eventLevels \(required\)**

  The levels of event to log\. You must specify each level to log\. Possible values include `INFORMATION`, `WARNING`, `ERROR`, `CRITICAL`, and `VERBOSE`\. This field is required for each type of Windows Event to log\.
+ **monitor \(optional\)**

  Boolean that indicates whether to monitor the logs\. The default value is `true`\.

### Alarm<a name="component-configuration-alarms"></a>

Defines a CloudWatch alarm to be monitored for the component\.

**JSON** 

```
{
  "alarmName" : "monitoredAlarmName",
  "severity" : HIGH/MEDIUM/LOW
}
```

**Properties**
+ **alarmName \(required\)**

  The name of the CloudWatch alarm to be monitored for the component\.
+ **severity \(optional\)**

  Indicates the degree of outage when the alarm goes off\. 