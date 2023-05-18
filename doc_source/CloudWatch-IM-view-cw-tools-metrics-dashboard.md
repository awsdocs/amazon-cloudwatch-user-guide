# Using CloudWatch Metrics with Amazon CloudWatch Internet Monitor<a name="CloudWatch-IM-view-cw-tools-metrics-dashboard"></a>

Amazon CloudWatch Internet Monitor publishes metrics to your account, including metrics for availability, round\-trip time, and throughput \(bytes per second\), which you can view in CloudWatch Metrics in the CloudWatch console\. To find all metrics for your monitor, in the CloudWatch Metrics dashboard, see the custom namespace `AWS/InternetMonitor`\. 

Metrics are aggregated across all internet traffic to your VPCs, CloudFront distributions, or WorkSpaces directories in the monitor, and to all traffic to each AWS Region and internet edge location that is monitored\. Regions are defined by the service location, which can either be all locations or a specific Region, such as `us-east-1`\. 

Note: *city\-networks* are client locations and ASNs, typically internet service providers \(ISPs\)\.

Internet Monitor provides the following metrics\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-IM-view-cw-tools-metrics-dashboard.html)

**Note**  
To see examples for using several of these metrics to help determine values to choose for a city\-networks maximum for your monitor, see [Choosing a city\-network maximum value](IMCityNetworksMaximum.md)\.

For more information, see [Use Amazon CloudWatch metrics](working_with_metrics.md)\.