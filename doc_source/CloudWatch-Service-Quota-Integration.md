# Service Quotas Integration and Usage Metrics<a name="CloudWatch-Service-Quota-Integration"></a>

You can use CloudWatch to proactively manage your AWS service quotas\. CloudWatch usage metrics provide visibility into your account's usage of resources and API operations\.

You can use these metrics to visualize your current service usage on CloudWatch graphs and dashboards\. You can use a CloudWatch metric math function to display the service quotas for those resources on your graphs\. You can also configure alarms that alert you when your usage approaches a service quota\.

For more information about service quotas, see [What Is Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) in the *Service Quotas User Guide*\.

Currently, the following services provide usage metrics that you can use to evaluate your service quotas:
+ AWS CloudHSM
+ [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Usage-Metrics.html)
+ Amazon DynamoDB
+ [Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html#service-quota-metrics)
+ [Amazon Elastic Container Registry](https://docs.aws.amazon.com/AmazonECR/latest/userguide/monitoring-usage.html)
+ AWS Key Management Service
+ [Amazon Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudwatch-metrics.html#fh-metrics-usage)
+ [AWS Robomaker](https://docs.aws.amazon.com/robomaker/latest/dg/monitoring-aws-robomaker-cloudwatch.html)

All of these services publish their usage metrics in the `AWS/Usage` metric namespace\.

**Topics**
+ [Visualizing Your Service Quotas and Setting Alarms](CloudWatch-Quotas-Visualize-Alarms.md)
+ [CloudWatch Usage Metrics](CloudWatch-Usage-Metrics.md)