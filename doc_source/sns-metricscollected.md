# Amazon Simple Notification Service Metrics and Dimensions<a name="sns-metricscollected"></a>

Amazon Simple Notification Service sends data points to CloudWatch for several metrics\. All active topics automatically send five\-minute metrics to CloudWatch\. Detailed monitoring, or one\-minute metrics, is currently unavailable for Amazon Simple Notification Service\. A topic stays active for six hours from the last activity \(for example, any API call\) on the topic\. For more information, see [Monitoring Amazon SNS with Amazon CloudWatch](https://docs.aws.amazon.com/sns/latest/dg/MonitorSNSwithCloudWatch.html) in the *Amazon Simple Notification Service Developer Guide*\.

## Amazon SNS Metrics<a name="SNS_metricscollected"></a>

Amazon SNS sends the following metrics to CloudWatch\.


| Metric | Description | 
| --- | --- | 
|  `NumberOfMessagesPublished`  |  The number of messages published to your Amazon SNS topics\. Units: *Count* Valid Statistics: Sum  | 
|  `NumberOfNotificationsDelivered`  |  The number of messages successfully delivered from your Amazon SNS topics to subscribing endpoints\. For a delivery attempt to succeed, the endpoint's subscription must accept the message\. A subscription accepts a message if a\.\) it lacks a filter policy or b\.\) its filter policy includes attributes that match those assigned to the message\. If the subscription rejects the message, the delivery attempt is not counted for this metric\. Units: *Count* Valid Statistics: Sum  | 
|  `NumberOfNotificationsFailed`  |  The number of messages that Amazon SNS failed to deliver\.  For Amazon SQS, email, SMS, or mobile push endpoints, the metric increments by 1 when Amazon SNS stops attempting message deliveries\. For HTTP or HTTPS endpoints, the metric includes every failed delivery attempt, including retries that follow the initial attempt\. For all other endpoints, the count increases by 1 when the message fails to deliver \(regardless of the number of attempts\)\. This metric does not include messages that were rejected by subscription filter policies\. You can control the number of retries for HTTP endpoints\. For more information, see [ Setting Amazon SNS Delivery Retry Policies for HTTP/HTTPS Endpoints](https://docs.aws.amazon.com/sns/latest/dg/DeliveryPolicies.html)\. Units: *Count* Valid Statistics: Sum, Average  | 
| NumberOfNotificationsFilteredOut |  The number of messages that were rejected by subscription filter policies\. A filter policy rejects a message when the message attributes do not match the policy attributes\. Units: *Count* Valid Statistics: Sum, Average  | 
| NumberOfNotificationsFilteredOut\-NoMessageAttributes |  The number of messages that were rejected by subscription filter policies because the messages have no attributes\. Units: *Count* Valid Statistics: Sum, Average  | 
| NumberOfNotificationsFilteredOut\-InvalidAttributes |  The number of messages that were rejected by subscription filter policies because the message's attributes are invalid – for example, because the attribute JSON is incorrectly formatted\. Units: *Count* Valid Statistics: Sum, Average  | 
|  `PublishSize`  |  The size of messages published\. Units: *Bytes* Valid Statistics: Minimum, Maximum, Average and Count  | 
| SMSMonthToDateSpentUSD |  The charges you have accrued since the start of the current calendar month for sending SMS messages\. You can set an alarm for this metric to know when your month\-to\-date charges are close to the monthly SMS spend limit for your account\. When Amazon SNS determines that sending an SMS message would incur a cost that exceeds this limit, it stops publishing SMS messages within minutes\. For information about setting your monthly SMS spend limit, or for information about requesting a spend limit increase with AWS, see [ Setting SMS Messaging Preferences](https://docs.aws.amazon.com/sns/latest/dg/sms_preferences.html)\.  Units: *USD* Valid Statistics: Maximum  | 
|  `SMSSuccessRate`  |  The rate of successful SMS message deliveries\. Units: *Count* Valid Statistics: Sum, Average, Data Samples  | 

## Dimensions for Amazon Simple Notification Service Metrics<a name="sns-metric-dimensions"></a>

Amazon Simple Notification Service sends the following dimensions to CloudWatch\.


|  Dimension  |  Description  | 
| --- | --- | 
|  Application  |  Filters on application objects, which represent an app and device registered with one of the supported push notification services, such as and \.  | 
|  Application,Platform  |  Filters on application and platform objects, where the platform objects are for the supported push notification services, such as and \.  | 
| Country |  Filters on the destination country or region of an SMS message\. The country or region is represented by its ISO 3166\-1 alpha\-2 code\.  | 
|  Platform  |  Filters on platform objects for the push notification services, such as and \.  | 
|  TopicName  |  Filters on Amazon SNS topic names\.  | 
| SMSType |  Filters on the message type of SMS message\. Can be *promotional* or *transactional*\.  | 