# Amazon Simple Email Service Metrics and Dimensions<a name="ses-metricscollected"></a>

Amazon Simple Email Service sends certain data points to CloudWatch\. These data points track important metrics related to your email sending activities\. For more information, see [Retrieving Amazon SES Event Data from CloudWatch](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/event-publishing-retrieving-cloudwatch.html) in the *Amazon Simple Email Service Developer Guide*\.

## Amazon SES Metrics<a name="ses-event-metrics"></a>

The following metrics are available from Amazon SES\.


| Metric | Description | 
| --- | --- | 
|  Send  |  The number of messages that Amazon SES accepted and attempted to send\. This value may be distinct from the Delivery metric, because messages could bounce or be rejected by the email servers that receive them\. Unit: Count  | 
|  Reject  |  The number of messages that Amazon SES did not send because they contained viruses or malicious content\. Amazon SES may initially accept a message that contains a virus, but as soon as it detects the virus, it stops processing the message\. When Amazon SES rejects a message, it doesn't attempt to deliver the message to the recipient's mail server\. Unit: Count  | 
|  Bounce  |  The number of emails that resulted in a hard bounce\. A hard bounce occurs when an email is permanently rejected by the recipient's mail server\. Unit: Count  | 
|  Complaint  |  The number of emails that were marked by their recipients as spam\. Unit: Count  | 
|  Delivery  |  The number of emails that Amazon SES successfully delivered to the mail servers of the intended recipients\. Unit: Count  | 
|  Open  |  The number of emails that were opened by their recipients\. Amazon SES only tracks this metric when you use a configuration set that publishes Open events\. Additionally, Amazon SES only captures Open events for HTML emails\. Unit: Count  | 
|  Click  |  The number of emails in which the recipient clicked a link\. Amazon SES only tracks this metric when you use a configuration set that publishes Click events\. Additionally, Amazon SES only captures Click events for HTML emails\. Unit: Count  | 
|  Rendering Failure  |  The number of emails that weren't able to be sent because they contained template rendering issues\. This event type only occurs when you send templated email using the `SendTemplatedEmail` or `SendBulkTemplatedEmail` API operations\. This event type can occur when template data is missing, or when there is a mismatch between template parameters and data\. Unit: Count  | 
|  Reputation\.BounceRate  |  The percentage of messages that resulted in hard bounces\. To calculate the percentage of emails that bounced, multiply `Reputation.BounceRate` by 100\.  Unit: Percent  | 
|  Reputation\.ComplaintRate  |  The percentage of messages that were reported as spam by their recipients\. To calculate the percentage of emails that resulted in complaints, multiply `Reputation.ComplaintRate` by 100\.  Unit: Percent  | 

## Dimensions for Amazon SES Metrics<a name="ses-metric-dimensions"></a>

 CloudWatch uses the dimension names that you specify when you add a CloudWatch event destination to a configuration set in Amazon SES\. For more information, see [Set Up a CloudWatch Event Destination for Amazon SES Event Publishing](http://docs.aws.amazon.com/ses/latest/DeveloperGuide//event-publishing-add-event-destination-cloudwatch.html)\. 