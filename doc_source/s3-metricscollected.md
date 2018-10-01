# Amazon Simple Storage Service Metrics and Dimensions<a name="s3-metricscollected"></a>

Amazon Simple Storage Service sends data points to CloudWatch for several metrics, such as object counts and bytes stored, once a day\. For more information, see [Monitoring Amazon S3 with CloudWatch](https://docs.aws.amazon.com/AmazonS3/latest/dev/cloudwatch-monitoring.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Amazon S3 CloudWatch Daily Storage Metrics for Buckets<a name="s3-cloudwatch-metrics"></a>

The `AWS/S3` namespace includes the following daily storage metrics for buckets\.


| Metric | Description | 
| --- | --- | 
| BucketSizeBytes |  The amount of data in bytes stored in a bucket in the Standard storage class, Standard \- Infrequent Access \(Standard\_IA\) storage class, OneZone \- Infraquent Access \(OneZone\_IA\), Reduced Redundancy Storage \(RRS\) class, or Glacier \(GLACIER\) storage class Valid storage type filters: `StandardStorage`, `GlacierS3ObjectOverhead`, `StandardIAStorage`, `StandardIAObjectOverhead`, `OneZoneIAStorage`, `OneZoneIAObjectOverhead`, `ReducedRedundancyStorage`, `GlacierStorage`, and `GlacierObjectOverhead` \(see the `StorageType` dimension\) Units: Bytes Valid statistics: Average  | 
| NumberOfObjects |  The total number of objects stored in a bucket for all storage classes Valid storage type filters: `AllStorageTypes` \(see the `StorageType` dimension\) Units: Count Valid statistics: Average  | 

The `AWS/S3` namespace includes the following request metrics\.

## Amazon S3 CloudWatch Request Metrics<a name="s3-request-cloudwatch-metrics"></a>


| Metric | Description | 
| --- | --- | 
| AllRequests |  The total number of HTTP requests made to an Amazon S3 bucket, regardless of type\. If you’re using a metrics configuration with a filter, then this metric only returns the HTTP requests made to the objects in the bucket that meet the filter's requirements\. Units: Count Valid statistics: Sum  | 
| GetRequests |  The number of HTTP GET requests made for objects in an Amazon S3 bucket\. This doesn't include list operations\. Units: Count Valid statistics: Sum  Paginated list\-oriented requests, like [List Multipart Uploads](https://docs.aws.amazon.com/AmazonS3/latest/API//mpUploadListMPUpload.html), [List Parts](https://docs.aws.amazon.com/AmazonS3/latest/API//mpUploadListParts.html), [Get Bucket Object versions](https://docs.aws.amazon.com/AmazonS3/latest/API//RESTBucketGETVersion.html), and others, are not included in this metric\.   | 
| PutRequests |  The number of HTTP PUT requests made for objects in an Amazon S3 bucket\. Units: Count Valid statistics: Sum  | 
| DeleteRequests |  The number of HTTP DELETE requests made for objects in an Amazon S3 bucket\. This also includes [Delete Multiple Objects](https://docs.aws.amazon.com/AmazonS3/latest/API/multiobjectdeleteapi.html) requests\. This metric shows the number of requests, not the number of objects deleted\. Units: Count Valid statistics: Sum  | 
| HeadRequests |  The number of HTTP HEAD requests made to an Amazon S3 bucket\. Units: Count Valid statistics: Sum  | 
| PostRequests |  The number of HTTP POST requests made to an Amazon S3 bucket\. Units: Count Valid statistics: Sum  [Delete Multiple Objects](https://docs.aws.amazon.com/AmazonS3/latest/API/multiobjectdeleteapi.html) and [SELECT Object Content](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectSELECTContent.html) requests are not included in this metric\.    | 
| SelectRequests |  The number of Amazon S3 [SELECT Object Content](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectSELECTContent.html) requests made for objects in an Amazon S3 bucket\.  Units: Count Valid statistics: Sum  | 
| SelectScannedBytes |  The number of bytes of data scanned with Amazon S3 [SELECT Object Content](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectSELECTContent.html) requests in an Amazon S3 bucket\.   Units: Bytes  Valid statistics: Average \(bytes per request\), Sum \(bytes per period\), Sample Count, Min, Max  | 
| SelectReturnedBytes |  The number of bytes of data returned with Amazon S3 [SELECT Object Content](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectSELECTContent.html) requests in an Amazon S3 bucket\.   Units: Bytes  Valid statistics: Average \(bytes per request\), Sum \(bytes per period\), Sample Count, Min, Max  | 
| ListRequests |  The number of HTTP requests that list the contents of a bucket\. Units: Count Valid statistics: Sum  | 
| BytesDownloaded |  The number bytes downloaded for requests made to an Amazon S3 bucket, where the response includes a body\. Units: Bytes Valid statistics: Average \(bytes per request\), Sum \(bytes per period\), Sample Count, Min, Max  | 
| BytesUploaded |  The number bytes uploaded that contain a request body, made to an Amazon S3 bucket\. Units: Bytes Valid statistics: Average \(bytes per request\), Sum \(bytes per period\), Sample Count, Min, Max  | 
| 4xxErrors |  The number of HTTP 4xx client error status code requests made to an Amazon S3 bucket with a value of either 0 or 1\. The `average` statistic shows the error rate, and the `sum` statistic shows the count of that type of error, during each period\. Units: Count Valid statistics: Average \(reports per request\), Sum \(reports per period\), Min, Max, Sample Count  | 
| 5xxErrors |  The number of HTTP 5xx server error status code requests made to an Amazon S3 bucket with a value of either 0 or 1\. The `average` statistic shows the error rate, and the `sum` statistic shows the count of that type of error, during each period\. Units: Counts Valid statistics: Average \(reports per request\), Sum \(reports per period\), Min, Max, Sample Count  | 
| FirstByteLatency |  The per\-request time from the complete request being received by an Amazon S3 bucket to when the response starts to be returned\. Units: Milliseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 
| TotalRequestLatency |  The elapsed per\-request time from the first byte received to the last byte sent to an Amazon S3 bucket\. This includes the time taken to receive the request body and send the response body, which is not included in `FirstByteLatency`\. Units: Milliseconds Valid statistics: Average, Sum, Min, Max, Sample Count  | 

## Amazon S3 CloudWatch Dimensions<a name="s3-cloudwatch-dimensions"></a>

The following dimensions are used to filter Amazon S3 metrics\.


|  Dimension  |  Description  | 
| --- | --- | 
|  BucketName  |  This dimension filters the data you request for the identified bucket only\.  | 
|  StorageType  |  This dimension filters the data that you have stored in a bucket by the type of storage\. The types are `StandardStorage` for the STANDARD storage class, `StandardIAStorage` for the STANDARD\_IA storage class, `OneZoneIAStorage` for the ONEZONE\_IA storage class, `ReducedRedundancyStorage` for the REDUCED\_REDUNDANCY storage class, `GlacierStorage` for the GLACIER storage class, and `AllStorageTypes`\. The `AllStorageTypes` type includes the STANDARD, STANDARD\_IA, ONEZONE\_IA, REDUCED\_REDUNDANCY, and GLACIER storage classes\.   | 
| FilterId | This dimension filters metrics configurations that you specify for request metrics on a bucket, for example, a prefix or a tag\. You specify a filter id when you create a metrics configuration\. For more information, see [Metrics Configurations for Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/metrics-configurations.html)\. | 