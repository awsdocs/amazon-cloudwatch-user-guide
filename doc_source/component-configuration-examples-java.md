# Java<a name="component-configuration-examples-java"></a>

The following example shows a component configuration in JSON format for Java\.

```
{
  "alarmMetrics": [ {
    "alarmMetricName": "java_lang_threading_threadcount",
    "monitor": true
  },
  {
    "alarmMetricName": "java_lang_memory_heapmemoryusage_used",
    "monitor": true
  },
  {
    "alarmMetricName": "java_lang_memory_heapmemoryusage_committed",
    "monitor": true
  }],
  "logs": [ ],
  "JMXPrometheusExporter": {
      "hostPort": "8686",
      "prometheusPort": "9404"
  }
}
```

**Note**  
Application Insights does not support configuring authentication for Prometheus JMX exporter\. For information about how to set up authentication, see the [Prometheus JMX exporter example configuration](https://github.com/prometheus/jmx_exporter#configuration)\.