# Amazon Simple Storage Service Metrics and Dimensions<a name="s3-metricscollected"></a>

Amazon Simple Storage Service sends data points to CloudWatch for several metrics, such as object counts and bytes stored, once a day\. For more information, see [Monitoring Amazon S3 with CloudWatch](http://docs.aws.amazon.com/AmazonS3/latest/dev/cloudwatch-monitoring.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Amazon S3 CloudWatch Metrics<a name="s3-cloudwatch-metrics"></a>

The `AWS/S3` namespace includes the following daily storage metrics for buckets\.


| Metric | Description | 
| --- | --- | 
| BucketSizeBytes |  The amount of data in bytes stored in a bucket in the Standard storage class, Standard \- Infrequent Access \(Standard\_IA\) storage class, or the Reduced Redundancy Storage \(RRS\) storage class\. Valid storage type filters: `StandardStorage`, or `StandardIAStorage`, or `ReducedRedundancyStorage` \(see `StorageType` dimension\) Units: Bytes Valid statistics: Average  | 
| NumberOfObjects |  The total number of objects stored in a bucket for all storage classes except for the `GLACIER` storage class\. Valid storage type filters: `AllStorageTypes` only \(see `StorageType` dimension\) Units: Count Valid statistics: Average  | 

The `AWS/S3` namespace includes the following request metrics\.


| Metric | Description | 
| --- | --- | 
| AllRequests |  The total number of HTTP requests made to a bucket, regardless of type\. If you use a metrics configuration with a filter, this metric returns only the HTTP requests made to the objects in the bucket that meet the filter requirements\. Units: Count Valid statistics: Sum  | 
| GetRequests |  The number of HTTP GET requests made for objects in a bucket\. This doesn't include list operations\. Paginated list\-oriented requests, such as [List Multipart Uploads](http://docs.aws.amazon.com/AmazonS3/latest/API//mpUploadListMPUpload.html), [List Parts](http://docs.aws.amazon.com/AmazonS3/latest/API//mpUploadListParts.html), [Get Bucket Object Versions](http://docs.aws.amazon.com/AmazonS3/latest/API//RESTBucketGETVersion.html), and others, are not included in this metric\. Units: Count Valid statistics: Sum  | 
| PutRequests |  The number of HTTP PUT requests made for objects in a bucket\. Units: Count Valid statistics: Sum  | 
| DeleteRequests |  The number of HTTP DELETE requests made for objects in a bucket\. This also includes [Delete Multiple Objects](http://docs.aws.amazon.com/AmazonS3/latest/API/multiobjectdeleteapi.html) requests\. Units: Count Valid statistics: Sum  | 
| HeadRequests |  The number of HTTP HEAD requests made to a bucket\. Units: Count Valid statistics: Sum  | 
| PostRequests |  The number of HTTP POST requests made to a bucket\. Units: Count Valid statistics: Sum  | 
| ListRequests |  The number of HTTP requests that list the contents of a bucket\. Units: Count Valid statistics: Sum  | 
| BytesDownloaded |  The number bytes downloaded for requests made to a bucket, where the response includes a body\. Units: Bytes Valid statistics: Average \(bytes per request\), Sum \(bytes per period\), Sample Count, Min, Max  | 
| BytesUploaded |  The number bytes uploaded to a bucket that contain a request body\. Units: Bytes Valid statistics: Average \(bytes per request\), Sum \(bytes per period\), Sample Count, Min, Max  | 
| 4xxErrors |  The number of HTTP 4xx client error status code requests made to a bucket with a value of 0 or 1\. The `average` statistic shows the error rate, and the `sum` statistic shows the count of that type of error, during each period\. Units: Count Valid statistics: Average \(reports per request\), Sum \(reports per period\), Min, Max, Sample Count  | 
| 5xxErrors |  The number of HTTP 5xx server error status code requests made to a bucket with a value of either 0 or 1\. The `average` statistic shows the error rate, and the `sum` statistic shows the count of that type of error, during each period\. Units: Counts Valid statistics: Average \(reports per request\), Sum \(reports per period\), Min, Max, Sample Count  | 
| FirstByteLatency |  The per\-request time from the complete request being received by a bucket to when the response starts to be returned\. Units: Milliseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| TotalRequestLatency |  The elapsed per\-request time from the first byte received to the last byte sent to a bucket\. This includes the time taken to receive the request body and send the response body, which is not included in `FirstByteLatency`\. Units: Milliseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 

## Amazon S3 CloudWatch Dimensions<a name="s3-cloudwatch-dimensions"></a>

The following dimensions are used to filter Amazon S3 metrics\.


| Dimension | Description | 
| --- | --- | 
| BucketName |  Filters the data you request for the identified bucket only\.  | 
| StorageType |  Filters the data stored in a bucket by the type of storage\. The types are `StandardStorage` for the Standard storage class, `StandardIAStorage` for the Standard\_IA storage class, `ReducedRedundancyStorage` for the Reduced Redundancy Storage \(RRS\) class, and `AllStorageTypes`\. Note that the `AllStorageTypes` type does not include the `GLACIER` storage class\.  | 
| FilterId |  Filters metrics configurations that you specify for request metrics on a bucket, for example, a prefix or a tag\. You specify a filter ID when you create a metrics configuration\.  | 