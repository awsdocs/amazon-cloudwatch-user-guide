# Amazon Simple Notification Service Metrics and Dimensions<a name="sns-metricscollected"></a>

Amazon Simple Notification Service sends data points to CloudWatch for several metrics\. All active topics automatically send five\-minute metrics to CloudWatch\. Detailed monitoring, or one\-minute metrics, is currently unavailable for Amazon Simple Notification Service\. A topic stays active for six hours from the last activity \(for example, any API call\) on the topic\. For more information, see [Monitoring Amazon SNS with Amazon CloudWatch](http://docs.aws.amazon.com/sns/latest/dg/MonitorSNSwithCloudWatch.html) in the *Amazon Simple Notification Service Developer Guide*\.

## Amazon Simple Notification Service Metrics<a name="sns-metrics"></a>

The `AWS/SNS` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| `NumberOfMessagesPublished` |  The number of messages published\. Units: Count Valid Statistics: Sum  | 
| `PublishSize` |  The size of messages published\. Units: Bytes Valid Statistics: Minimum, Maximum, Average and Count  | 
| `NumberOfNotificationsDelivered` |  The number of messages successfully delivered\. Units: Count Valid Statistics: Sum  | 
| `NumberOfNotificationsFailed` |  The number of messages that Amazon SNS failed to deliver\. This metric is applied after Amazon SNS stops attempting message deliveries to Amazon SQS, email, SMS, or mobile push endpoints\. Each delivery attempt to an HTTP or HTTPS endpoint adds 1 to the metric\. For all other endpoints, the count increases by 1 when the message is not delivered \(regardless of the number of attempts\)\. You can control the number of retries for HTTP endpoints; for more information, see [Setting Amazon SNS Delivery Retry Policies for HTTP/HTTPS Endpoints](http://docs.aws.amazon.com/sns/latest/dg/DeliveryPolicies.html)\. Units: Count Valid Statistics: Sum, Average  | 
| `SMSSuccessRate` |  The rate of successful SMS message deliveries\. Units: Count Valid Statistics: Sum, Average, Data Samples  | 

## Dimensions for Amazon Simple Notification Service Metrics<a name="sns-metric-dimensions"></a>

Amazon SNS sends the following dimensions to CloudWatch\.


|  Dimension  |  Description  | 
| --- | --- | 
|  Application  |  Filters on application objects, which represent an app and device registered with one of the supported push notification services, such as APNS and GCM\.  | 
|  Application,Platform  |  Filters on application and platform objects, where the platform objects are for the supported push notification services, such as APNS and GCM\.  | 
|  Country  |  Filters on the destination country of an SMS message\. The country is represented by its ISO 3166\-1 alpha\-2 code\.  | 
|  Platform  |  Filters on platform objects for the push notification services, such as APNS and GCM\.  | 
|  TopicName  |  Filters on Amazon SNS topic names\.  | 
|  SMSType  |  Filters on the message type of SMS message\. Can be *promotional* or *transactional*\.  | 