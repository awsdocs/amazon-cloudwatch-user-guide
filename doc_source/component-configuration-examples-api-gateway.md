# API Gateway REST API stages<a name="component-configuration-examples-api-gateway"></a>

The following example shows a component configuration in JSON format for API Gateway REST API stages\.

```
{ 
     "alarmMetrics" : [ 
         {
             "alarmMetricName" : "4XXError",   
             "monitor" : true
         }, 
         {
             "alarmMetricName" : "5XXError",   
             "monitor" : true
         } 
     ],
    "logs" : [
        { 
            "logType" : "API_GATEWAY_EXECUTION",   
            "monitor" : true  
        },
        { 
            "logType" : "API_GATEWAY_ACCESS",   
            "monitor" : true  
        }
    ]
}
```
