# Amazon VPC Network Address Translation \(NAT\) gateways<a name="component-configuration-examples-nat-gateway"></a>

The following example shows a component configuration in JSON format for NAT gateways\.

```
{
  "alarmMetrics": [
    {
      "alarmMetricName": "ErrorPortAllocation",
      "monitor": true
    },
    {
      "alarmMetricName": "IdleTimeoutCount",
      "monitor": true
    }
  ]
}
```