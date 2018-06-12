# Amazon VPC VPN Metrics and Dimensions<a name="vpn-metricscollected"></a>

Amazon VPN sends data to CloudWatch as it becomes available\. For more information, see [Monitoring with CloudWatch](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/monitoring-cloudwatch.html) in the *Amazon VPC User Guide*\.

## VPN Metrics<a name="vpn-metrics"></a>

The following metrics are available from Amazon VPC VPN\.


| Metric | Description | 
| --- | --- | 
|  TunnelState  |  The state of the tunnel\. 0 indicates DOWN and 1 indicates UP\. Units: Boolean  | 
|  TunnelDataIn  |  The bytes received through the VPN tunnel\. Each metric data point represents the number of bytes received after the previous data point\. Use the Sum statistic to show the total number of bytes received during the period\. This metric counts the data after decryption\. Units: Bytes  | 
|  TunnelDataOut  |  The bytes sent through the VPN tunnel\. Each metric data point represents the number of bytes sent after the previous data point\. Use the Sum statistic to show the total number of bytes sent during the period\. This metric counts the data before encryption\. Units: Bytes  | 

## Dimensions for VPN Metrics<a name="vpn-metrics-dimensions"></a>

You can filter the Amazon VPC VPN data using the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `VpnId` |  This dimension filters the data by the VPN connection\.  | 
| `TunnelIpAddress` |  This dimension filters the data by the IP address of the tunnel for the virtual private gateway\.  | 