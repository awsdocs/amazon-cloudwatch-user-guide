# Amazon Lex Metrics<a name="lex-metricscollected"></a>

The following tables describe the Amazon Lex metrics\.

## CloudWatch Metrics for Amazon Lex Runtime<a name="cloudwatch-dimensions-for-aws-lex-runtime"></a>

The following table describes the Amazon Lex runtime metrics\.


| Metric | Description | 
| --- | --- | 
| RuntimeInvalidLambdaResponses |  The number of invalid Lambda responses in the specified period\. Valid dimension for the `PostContent`operation with the `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimension for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html)  | 
| RuntimeLambdaErrors | The number of AWS Lambda runtime errors in the specified period\.Valid dimension for the `PostContent` operation with the `Text `or `Speech`` InputMode` :[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html)Valid dimension for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html)  | 
| MissedUtteranceCount |  The number of utterances that were not recognized in the specified period\. Valid dimensions for the `PostContent` operation with the `Text `or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimensions for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html)  | 
| RuntimePollyErrors |  The number of invalid Amazon Polly responses in the specified period\. Valid dimension for the `PostContent` operation with the `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimension for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html)  | 
| RuntimeRequestCount |  The number of runtime requests in the specified period\. Valid dimensions for the `PostContent` operation with the `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimensions for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Unit: Count  | 
| RuntimeSucessfulRequestLatency |  The latency for successful requests between the time that the request was made and the response was passed back\. Valid dimensions for the `PostContent` operation with the `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimensions for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Unit: Milliseconds  | 
| RuntimeSystemErrors |  The number of system errors in the specified period\. The response code range for a system error is 500 to 599\. Valid dimension for the `PostContent` operation with the `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimension for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Unit: Count  | 
| RuntimeThrottledEvents |  The number of throttled requests\. Amazon Lex throttles a request when it receives more requests than the limit of transactions per second set for your account\. If the limit set for your account is frequently exceeded, you can request a limit increase\. To request an increase, see [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.  Valid dimension for the `PostContent` operation with the `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimension for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Unit: Count  | 
| RuntimeUserErrors |  The number of user errors in the specified period\. The response code range for a user error is 400 to 499\. Valid dimension for the `PostContent` operation with `Text` or `Speech` `InputMode`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Valid dimension for the `PostText` operation: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/lex-metricscollected.html) Unit: Count  | 

Amazon Lex runtime metrics use the `AWS/Lex` namespace, and provide metrics in the following dimensions\. You can group metrics by dimenstions in the CloudWatch console:


| Dimension | Description | 
| --- | --- | 
| BotName, BotAlias, Operation, InputMode | Groups metrics by the bot's alias, the bot's name, the operation \(PostContent\), and by whether the input was text or speech\. | 
| BotName, BotVersion, Operation, InputMode | Groups metrics by the bot's name, the version of the bot, the operation \(PostContent\), and by whether the input was text or speech\. | 
| BotName, BotVersion, Operation | Groups metrics by the bot's name, the bots version, and by the operation, PostText\. | 
| BotName, BotAlias, Operation | Groups metrics by the bot's name, the bot's alias, and by the operation, PostText\. | 

## CloudWatch Metrics for Amazon Lex Channel Associations<a name="cloudwatch-dimensions-for-aws-lex-channels"></a>

A channel association is the association between Amazon Lex and a messaging channel, such as Facebook\. The following table describes the Amazon Lex channel association metrics\.


| Metric | Description | 
| --- | --- | 
| BotChannelAuthErrors | The number of authentication errors returned by the messaging channel in the specified time period\. An authentication error indicates that the secret token provided during channel creation is invalid or has expired\.   | 
| BotChannelConfigurationErrors | The number of configuration errors in the specified period\. A configuration error indicates that one or more configuration entries for the channel are invalid\.  | 
| BotChannelInboundThrottledEvents | The number of times that messages that were sent by the messaging channel were throttled by Amazon Lex in the specified period\.   | 
| BotChannelOutboundThrottledEvents | The number of times that outbound events from Amazon Lex to the messaging channel were throttled in the specified time period\.  | 
| BotChannelRequestCount | The number of requests made on a channel in the specified time period\.  | 
| BotChannelResponseCardErrors | The number of times that Amazon Lex could not post response cards in the specified period\.  | 
| BotChannelSystemErrors | The number of internal errors that occurred in Amazon Lex for a channel in the specified period\.  | 

Amazon Lex channel association metrics use the `AWS/Lex` namespace, and provide metrics for the following dimension\. You can group metrics by dimenstions in the CloudWatch console::


| Dimension | Description | 
| --- | --- | 
| BotAlias, BotChannelName, BotName, Source | Group metrics by the bot's alias, the channel name, the bot's name, and by the source of traffic\. | 