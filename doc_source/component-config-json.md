# Component configuration template fragment<a name="component-config-json"></a>

The following example shows a template fragment in JSON format\.

```
{
  "alarmMetrics" : [
    list of alarm metrics
  ],
  "logs" : [
    list of logs
  ],
  "instances" : [
    component nested instances configuration
  ],
  "windowsEvents" : [
    list of windows events channels configurations
  ],
  "alarms" : [
    list of CloudWatch alarms
  ]
}
```