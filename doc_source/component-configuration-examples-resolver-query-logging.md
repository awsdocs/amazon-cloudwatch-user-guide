# Amazon Route 53 Resolver query logging configuration<a name="component-configuration-examples-resolver-query-logging"></a>

The following example shows a component configuration in JSON format for Amazon Route 53 Resolver query logging configuration\.

```
{
  "logs": [
    {
      "logGroupName": "/resolver-query-log-config/logs",
      "logType": "ROUTE53_RESOLVER_QUERY_LOGS",
      "monitor": true
    }
  ]  
}
```