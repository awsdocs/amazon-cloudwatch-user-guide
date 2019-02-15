# How Amazon CloudWatch Works<a name="cloudwatch_architecture"></a>

Amazon CloudWatch is basically a metrics repository\. An AWS service—such as Amazon EC2—puts metrics into the repository, and you retrieve statistics based on those metrics\. If you put your own custom metrics into the repository, you can retrieve statistics on these metrics as well\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/CW-Overview.png)

You can use metrics to calculate statistics and then present the data graphically in the CloudWatch console\. For more information about the other AWS resources that generate and send metrics to CloudWatch, see [AWS Services That Publish CloudWatch Metrics](aws-services-cloudwatch-metrics.md)\.

You can configure alarm actions to stop, start, or terminate an Amazon EC2 instance when certain criteria are met\. In addition, you can create alarms that initiate Amazon EC2 Auto Scaling and Amazon Simple Notification Service \(Amazon SNS\) actions on your behalf\. For more information about creating CloudWatch alarms, see [Alarms](cloudwatch_concepts.md#CloudWatchAlarms)\.

AWS Cloud computing resources are housed in highly available data center facilities\. To provide additional scalability and reliability, each data center facility is located in a specific geographical area, known as a *region*\. Each region is designed to be completely isolated from the other regions, to achieve the greatest possible failure isolation and stability\. Amazon CloudWatch does not aggregate data across regions\. Therefore, metrics are completely separate between regions\. For more information, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#cw_region) in the *Amazon Web Services General Reference*\.