# Amazon VPC NAT Gateway Metrics and Dimensions<a name="nat-gateway-metricscollected"></a>

NAT gateway metric data is provided at 1\-minute frequency\. For more information, see [Monitoring Your NAT Gateway with Amazon CloudWatch](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway-cloudwatch.html) in the *Amazon VPC User Guide*\.

## NAT Gateway Metrics<a name="nat-gateway-metrics"></a>

The following metrics are available from the NAT gateway service\.


| Metric | Description | 
| --- | --- | 
|  PacketsOutToDestination  |  The number of packets sent out through the NAT gateway to the destination\. A value greater than zero indicates that there is traffic going to the internet from clients that are behind the NAT gateway\. If the value for `PacketsOutToDestination` is less than the value for `PacketsInFromSource`, there may be data loss during NAT gateway processing\. Unit: Count  | 
|  PacketsOutToSource  |  The number of packets sent through the NAT gateway to the clients in your VPC\. A value greater than zero indicates that there is traffic coming from the internet to clients that are behind the NAT gateway\. If the value for `PacketsOutToSource` is less than the value for `PacketsInFromDestination`, there may be data loss during NAT gateway processing, or traffic being actively blocked by the NAT gateway\. Unit: Count  | 
|  PacketsInFromSource  |  The number of packets received by the NAT gateway from clients in your VPC\. If the value for `PacketsOutToDestination` is less than the value for `PacketsInFromSource`, there may be data loss during NAT gateway processing\. Unit: Count  | 
|  PacketsInFromDestination  |  The number of packets received by the NAT gateway from the destination\. If the value for `PacketsOutToSource` is less than the value for `PacketsInFromDestination`, there may be data loss during NAT gateway processing, or traffic being actively blocked by the NAT gateway\. Unit: Count  | 
|  BytesOutToDestination  |  The number of bytes sent out through the NAT gateway to the destination\. A value greater than zero indicates that there is traffic going to the internet from clients that are behind the NAT gateway\. If the value for `BytesOutToDestination` is less than the value for `BytesInFromSource`, there may be data loss during NAT gateway processing\. Unit: Bytes  | 
|  BytesOutToSource  |  The number of bytes sent through the NAT gateway to the clients in your VPC\. A value greater than zero indicates that there is traffic coming from the internet to clients that are behind the NAT gateway\. If the value for `BytesOutToSource` is less than the value for `BytesInFromDestination`, there may be data loss during NAT gateway processing, or traffic being actively blocked by the NAT gateway\. Units: Bytes  | 
|  BytesInFromSource  |  The number of bytes received by the NAT gateway from clients in your VPC\. If the value for `BytesOutToDestination` is less than the value for `BytesInFromSource`, there may be data loss during NAT gateway processing\. Units: Bytes  | 
|  BytesInFromDestination  |  The number of bytes received by the NAT gateway from the destination\. If the value for `BytesOutToSource` is less than the value for `BytesInFromDestination`, there may be data loss during NAT gateway processing, or traffic being actively blocked by the NAT gateway\. Units: Bytes  | 
|  ErrorPortAllocation  |  The number of times the NAT gateway could not allocate a source port\.  A value greater than zero indicates that too many concurrent connections are open through the NAT gateway\. Units: Count  | 
|  ActiveConnectionCount  |  The total number of concurrent active TCP connections through the NAT gateway\. A value of zero indicates that there are no active connections through the NAT gateway\. Units: Count  | 
|  ConnectionAttemptCount  |  The number of connection attempts made through the NAT gateway\. If the value for `ConnectionEstablishedCount` is less than the value for `ConnectionAttemptCount`, this indicates that clients behind the NAT gateway attempted to establish new connections for which there was no response\. Unit: Count  | 
|  ConnectionEstablishedCount  |  The number of connections established through the NAT gateway\. If the value for `ConnectionEstablishedCount` is less than the value for `ConnectionAttemptCount`, this indicates that clients behind the NAT gateway attempted to establish new connections for which there was no response\. Unit: Count  | 
|  IdleTimeoutCount  |  The number of connections that transitioned from the active state to the idle state\. An active connection transitions to idle if it was not closed gracefully and there was no activity for the last 350 seconds\. A value greater than zero indicates that there are connections that have been moved to an idle state\. If the value for `IdleTimeoutCount` increases, it may indicate that clients behind the NAT gateway are re\-using stale connections\.  Unit: Count  | 
|  PacketsDropCount  |  The number of packets dropped by the NAT gateway\. A value greater than zero may indicate an ongoing transient issue with the NAT gateway\. If this value is high, see the [AWS service health dashboard](http://status.aws.amazon.com/)\. Units: Count  | 

## Dimensions for NAT Gateway Metrics<a name="nat-gateway-metrics-dimensions"></a>

You can filter the NAT gateway data using the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| NatGatewayId | This dimension filters data by the NAT gateway ID\. | 