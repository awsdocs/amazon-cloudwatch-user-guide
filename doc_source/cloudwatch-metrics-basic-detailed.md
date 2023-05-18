# Basic monitoring and detailed monitoring<a name="cloudwatch-metrics-basic-detailed"></a>

CloudWatch provides two categories of monitoring: *basic monitoring* and *detailed monitoring*\.

Many AWS services offer basic monitoring by publishing a default set of metrics to CloudWatch with no charge to customers\. By default, when you start using one of these AWS services, basic monitoring is automatically enabled\. For a list of services that offer basic monitoring, see [AWS services that publish CloudWatch metrics](aws-services-cloudwatch-metrics.md)\.

Detailed monitoring is offered by only some services\. It also incurs charges\. To use it for an AWS service, you must choose to activate it\. For more information about pricing, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing)\.

Detailed monitoring options differ based on the services that offer it\. For example, Amazon EC2 detailed monitoring provides more frequent metrics, published at one\-minute intervals, instead of the five\-minute intervals used in Amazon EC2 basic monitoring\. Detailed monitoring for Amazon S3 and Amazon Managed Streaming for Apache Kafka means more fine\-grained metrics\. 

In different AWS services, detailed monitoring also has different names\. For example, in Amazon EC2 it is called detailed monitoring, in AWS Elastic Beanstalk it is called enhanced monitoring, and in Amazon S3 it is called request metrics\.

Using detailed monitoring for Amazon EC2 helps you better manage your Amazon EC2 resources, so that you can find trends and take action faster\. For Amazon S3 request metrics are available at one\-minute intervals to help you quickly identify and act on operational issues\. On Amazon MSK, when you enable the `PER_BROKER`, `PER_TOPIC_PER_BROKER`, or `PER_TOPIC_PER_PARTITION` level monitoring, you get additional metrics that provide more visibility\.

The following table lists the services that offer detailed monitoring\. It also includes links to the documentation for those services that explain more about the detailed monitoring and provide instructions for how to activate it\.


| Service | Documentation | 
| --- | --- | 
|  Amazon API Gateway  |  [ Dimensions for API Gateway metrics](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-metrics-and-dimensions.html#api-gateway-metricdimensions)  | 
|  Amazon CloudFront  |  [ Viewing additional CloudFront distribution metrics](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/viewing-cloudfront-metrics.html#monitoring-console.distributions-additional)  | 
|  Amazon EC2  |  [ Enable or turn off detailed monitoring for your instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html)  | 
|  Elastic Beanstalk  |  [ Enhanced health reporting and monitoring](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/health-enhanced.html)  | 
|  Amazon Kinesis Data Streams  |  [ Enhanced Shard\-level Metrics](https://docs.aws.amazon.com/streams/latest/dev/monitoring-with-cloudwatch.html#kinesis-metrics-shard)  | 
|  Amazon MSK  |  [Amazon MSK Metrics for Monitoring with CloudWatch](https://docs.aws.amazon.com/msk/latest/developerguide/metrics-details.html)  | 
|  Amazon S3  |  [Amazon S3 request metrics in CloudWatch](https://docs.aws.amazon.com/AmazonS3/latest/userguide/metrics-dimensions.html#s3-request-cloudwatch-metrics)  | 