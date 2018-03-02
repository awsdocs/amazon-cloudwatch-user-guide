# AWS Direct Connect Metrics and Dimensions<a name="dx-metricscollected"></a>

By default, CloudWatch provides AWS Direct Connect metric data in 5\-minute intervals\. You can optionally view data in 1\-minute intervals\. For more information, see [Monitoring with CloudWatch](http://docs.aws.amazon.com/directconnect/latest/UserGuide/monitoring-cloudwatch.html) in the *AWS Direct Connect User Guide*\.

## AWS Direct Connect Metrics<a name="dx-metrics"></a>

The following metrics are available from AWS Direct Connect\. Metrics are currently available for AWS Direct Connect physical connections only\.


| Metric | Description | 
| --- | --- | 
|  ConnectionState  |  The state of the connection\. 0 indicates DOWN and 1 indicates UP\. Units: Boolean  | 
|  ConnectionBpsEgress  |  The bit rate for outbound data from the AWS side of the connection\. The number reported is the aggregate over the specified time period \(5 minutes by default, 1 minute minimum\)\. Units: Bits per second  | 
|  ConnectionBpsIngress  |  The bit rate for inbound data to the AWS side of the connection\. The number reported is the aggregate over the specified time period \(5 minutes by default, 1 minute minimum\)\. Units: Bits per second  | 
|  ConnectionPpsEgress  | The packet rate for outbound data from the AWS side of the connection\. The number reported is the aggregate over the specified time period \(5 minutes by default, 1 minute minimum\)\. Units: Packets per second | 
|  ConnectionPpsIngress  | The packet rate for inbound data to the AWS side of the connection\. The number reported is the aggregate over the specified time period \(5 minutes by default, 1 minute minimum\)\. Units: Packets per second | 
|  ConnectionCRCErrorCount  |  The number of times cyclic redundancy check \(CRC\) errors are observed for the data received at the connection\. Units: Integer  | 
| ConnectionLightLevelTx |  Indicates the health of the fiber connection for egress \(outbound\) traffic from the AWS side of the connection\. This metric is available for connections with 10 Gbps port speeds only\. Units: dBm  | 
|  ConnectionLightLevelRx  |  Indicates the health of the fiber connection for ingress \(inbound\) traffic to the AWS side of the connection\. This metric is available for connections with 10 Gbps port speeds only\. Units: dBm  | 

## Dimensions for AWS Direct Connect Metrics<a name="dx-metrics-dimensions"></a>

You can filter the AWS Direct Connect data using the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `ConnectionId` |  This dimension filters the data by the AWS Direct Connect connection\.  | 