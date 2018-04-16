# AWS Namespaces<a name="aws-namespaces"></a>

CloudWatch namespaces are containers for metrics\. Metrics in different namespaces are isolated from each other, so that metrics from different applications are not mistakenly aggregated into the same statistics\. All AWS services that provide Amazon CloudWatch data use a namespace string, beginning with "AWS/"\. When you create custom metrics, you must also specify a namespace as a container for custom metrics\. Namespaces for custom metrics cannot start with "AWS/"\. The following services push metric data points to CloudWatch\.


****  

| AWS Product | Namespace | 
| --- | --- | 
|  Amazon API Gateway  |  `AWS/ApiGateway`  | 
|  AppStream 2\.0  |  `AWS/AppStream`  | 
|  Amazon EC2 Auto Scaling  |  `AWS/AutoScaling`  | 
|  AWS Billing  |  `AWS/Billing`  | 
|  Amazon CloudFront  |  `AWS/CloudFront`  | 
|  Amazon CloudSearch  |  `AWS/CloudSearch`  | 
|  Amazon CloudWatch Events  |  `AWS/Events`  | 
|  Amazon CloudWatch Logs  |  `AWS/Logs`  | 
|  Amazon Connect  |  `AWS/Connect`  | 
|  AWS Database Migration Service  |  `AWS/DMS`  | 
|  AWS Direct Connect  |  `AWS/DX`  | 
|  Amazon DynamoDB  |  `AWS/DynamoDB`  | 
|  Amazon EC2  |  `AWS/EC2`  | 
|  Amazon EC2  |  `AWS/EC2Spot` \(Spot Instances\)  | 
|  Amazon Elastic Container Service  |  `AWS/ECS`  | 
|  AWS Elastic Beanstalk  |  `AWS/ElasticBeanstalk`  | 
|  Amazon Elastic Block Store  |  `AWS/EBS`  | 
|  Amazon Elastic File System  |  `AWS/EFS`  | 
|  Elastic Load Balancing  |  `AWS/ELB` \(Classic Load Balancers\)  | 
|  Elastic Load Balancing  |  `AWS/ApplicationELB` \(Application Load Balancers\)  | 
|  Elastic Load Balancing  |  `AWS/NetworkELB` \(Network Load Balancers\)  | 
|  Amazon Elastic Transcoder  |  `AWS/ElasticTranscoder`  | 
|  Amazon ElastiCache  |  `AWS/ElastiCache`  | 
|  Amazon Elasticsearch Service  |  `AWS/ES`  | 
|  Amazon EMR  |  `AWS/ElasticMapReduce`  | 
|  Amazon GameLift  |  `AWS/GameLift`  | 
|  Amazon Inspector |  `AWS/Inspector`  | 
|  AWS IoT  |  `AWS/IoT`  | 
|  AWS Key Management Service  |  `AWS/KMS`  | 
|  Amazon Kinesis Data Analytics  |  `AWS/KinesisAnalytics`  | 
|  Amazon Kinesis Data Firehose  |  `AWS/Firehose`  | 
|  Amazon Kinesis Data Streams  |  `AWS/Kinesis`  | 
|  Amazon Kinesis Video Streams  |  `AWS/KinesisVideo`  | 
|  AWS Lambda  |  `AWS/Lambda`  | 
|  Amazon Lex  |  `AWS/Lex`  | 
|  Amazon Machine Learning  |  `AWS/ML`  | 
|  AWS OpsWorks  |  `AWS/OpsWorks`  | 
|  Amazon Polly  |  `AWS/Polly`  | 
|  Amazon Redshift  |  `AWS/Redshift`  | 
|  Amazon Relational Database Service  |  `AWS/RDS`  | 
|  Amazon Route 53  |  `AWS/Route53`  | 
|  Amazon SageMaker  |  `AWS/SageMaker`  | 
|  AWS Shield Advanced  |  `AWS/DDoSProtection`  | 
|  Amazon Simple Email Service  |  `AWS/SES`  | 
|  Amazon Simple Notification Service  |  `AWS/SNS`  | 
|  Amazon Simple Queue Service  |  `AWS/SQS`  | 
|  Amazon Simple Storage Service  |  `AWS/S3`  | 
|  Amazon Simple Workflow Service  |  `AWS/SWF`  | 
|  AWS Step Functions  |  `AWS/States`  | 
|  AWS Storage Gateway  |  `AWS/StorageGateway`  | 
| Amazon Translate | `AWS/Translate` | 
|  Amazon VPC  |  `AWS/NATGateway` \(NAT gateway\)  | 
| Amazon VPC | `AWS/VPN` \(VPN\) | 
|  AWS WAF  |  `WAF`  | 
|  Amazon WorkSpaces  |  `AWS/WorkSpaces`  | 