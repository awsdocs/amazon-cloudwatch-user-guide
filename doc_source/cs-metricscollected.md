# Amazon CloudSearch Metrics and Dimensions<a name="cs-metricscollected"></a>

Amazon CloudSearch sends metrics to Amazon CloudWatch\. For more information, see [Monitoring an Amazon CloudSearch Domain with Amazon CloudWatch](https://docs.aws.amazon.com/cloudsearch/latest/developerguide/cloudwatch-monitoring.html) in the *Amazon CloudSearch Developer Guide*\.

## Amazon CloudSearch Metrics<a name="cloudsearch-metrics"></a>

The `AWS/CloudSearch` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
|  `SuccessfulRequests`  |  The number of search requests successfully processed by a search instance\.  Units: Count Valid statistics: Maximum, Sum  | 
|  `SearchableDocuments`  |  The number of searchable documents in the domain's search index\.  Units: Count Valid statistics: Maximum  | 
|  `IndexUtilization`  |  The percentage of the search instance's index capacity that has been used\. The Maximum value indicates the percentage of the domain's index capacity that has been used\. Units: Percent Valid statistics: Average, Maximum  | 
|  `Partitions`  |  The number of partitions the index is distributed across\. Units: Count Valid statistics: Minimum, Maximum  | 

## Dimensions for Amazon CloudSearch Metrics<a name="cloudsearch-metric-dimensions"></a>

Amazon CloudSearch sends the ClientId and DomainName dimensions to CloudWatch\.


| Dimension | Description | 
| --- | --- | 
| `ClientId` |  The AWS account ID\.  | 
| `DomainName` |  The name of the search domain\.  | 