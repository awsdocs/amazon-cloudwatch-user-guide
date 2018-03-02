# Amazon CloudFront Metrics and Dimensions<a name="cf-metricscollected"></a>

Amazon CloudFront sends metrics to Amazon CloudWatch for web distributions\. Metrics and dimensions are not available for RTMP distributions\. For more information, see [Monitoring CloudFront Activity Using CloudWatch](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/monitoring-using-cloudwatch.html) in the *Amazon CloudFront Developer Guide*\.

## Amazon CloudFront Metrics<a name="cloudfront-metrics"></a>

The `AWS/CloudFront` namespace includes the following metrics\.

**Note**  
Only one statistic, Average or Sum, is applicable for each metric\. However, all statistics are available through the console, API, and AWS Command Line Interface\. In the following table, each metric specifies the statistic that is applicable to that metric\.


| Metric | Description | 
| --- | --- | 
|  `Requests` |  The number of requests for all HTTP methods and for both HTTP and HTTPS requests\. Valid Statistics: Sum Units: None  | 
|  `BytesDownloaded` |  The number of bytes downloaded by viewers for `GET`, `HEAD`, and `OPTIONS` requests\. Valid Statistics: Sum Units: None  | 
| `BytesUploaded` | The number of bytes uploaded to your origin with CloudFront using `POST` and `PUT` requests\. Valid Statistics: Sum Units: None  | 
| `TotalErrorRate` | The percentage of all requests for which the HTTP status code is `4xx` or `5xx`\. Valid Statistics: Average Units: Percent  | 
| `4xxErrorRate` | The percentage of all requests for which the HTTP status code is `4xx`\. Valid Statistics: Average Units: Percent  | 
| `5xxErrorRate` | The percentage of all requests for which the HTTP status code is `5xx`\. Valid Statistics: Average Units: Percent  | 

## Dimensions for CloudFront Metrics<a name="cloudfront-metricdimensions"></a>

CloudFront metrics use the CloudFront namespace and provide metrics for two dimensions:


| Dimension | Description | 
| --- | --- | 
| `DistributionId` | The CloudFront ID of the distribution for which you want to display metrics\. | 
| `Region` | The region for which you want to display metrics\. This value must be `Global`\. The `Region` dimension is different from the region in which CloudFront metrics are stored, which is US East \(N\. Virginia\)\.  | 