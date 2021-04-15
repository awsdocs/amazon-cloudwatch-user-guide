# Component configuration sections<a name="component-config-sections"></a>

A component configuration includes several major sections\. Sections in a component configuration can be listed in any order\.
+ **alarmMetrics \(optional\)**

  A list of [metrics](#component-config-metric) to monitor for the component\. All component types can have an alarmMetrics section\. 
+ **logs \(optional\)**

  A list of [logs](#component-configuration-log) to monitor for the component\. Only EC2 instances can have a logs section\. 
+ **subComponents \(optional\)**

  Nested instance and volume subComponent configuration for the component\. The following types of components can have nested instances and a subComponents section: ELB, ASG, custom\-grouped EC2 instances , and EC2 instances\.
+ **alarms \(optional\)**

  A list of [alarms](#component-configuration-alarms) to monitor for the component\. All component types can have an alarm section\.
+ **windowsEvents \(optional\)**

  A list of [windows events](#windows-events) to monitor for the component\. Only Windows on EC2 instances have a `windowsEvents` section\.
+ **JMXPrometheusExporter \(optional\)**

  JMXPrometheus Exporter configuration\.

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
+ [JMX Prometheus Exporter](#component-configuration-prometheus)
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
  + `SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP`
  + `MYSQL`
  + `MYSQL_SLOW_QUERY`
  + `POSTGRESQL`
  + `WINDOWS_EVENTS`
  + `STEP_FUNCTION`
  + `IIS`
  + `APPLICATION`
  + `DEFAULT`
  + `CUSTOM`
  + `ORACLE_ALERT`
  + `ORACLE_LISTENER`
+ **encoding \(optional\)**

  The type of encoding of the logs to be monitored\. The specified encoding should be included in the list of [CloudWatch agent supported encodings](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AgentReference.html)\. If not provided, CloudWatch Application Insights uses the default encoding type for the log type:
  + For `MYSQL`/`POSTGRESQL`/`WINDOWS_EVENTS`/`SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP`/`APPLICATION`/`DEFAULT`: utf\-8 encoding
  + For `SQL_SERVER`: utf\-16 encoding
  + For `IIS`: ascii encoding
+ **monitor \(optional\)**

  Boolean that indicates whether to monitor the logs\. The default value is `true`\.

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