# What Is Amazon CloudWatch?<a name="WhatIsCloudWatch"></a>

Amazon CloudWatch monitors your Amazon Web Services \(AWS\) resources and the applications you run on AWS in real time\. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications\.

The CloudWatch home page automatically displays metrics about every AWS service you use\. You can additionally create custom dashboards to display metrics about your custom applications, and display custom collections of metrics that you choose\.

You can create alarms that watch metrics and send notifications or automatically make changes to the resources you are monitoring when a threshold is breached\. For example, you can monitor the CPU usage and disk reads and writes of your Amazon EC2 instances and then use that data to determine whether you should launch additional instances to handle increased load\. You can also use this data to stop under\-used instances to save money\.

With CloudWatch, you gain system\-wide visibility into resource utilization, application performance, and operational health\.

## Accessing CloudWatch<a name="accessing_cloudwatch"></a>

You can access CloudWatch using any of the following methods:
+ **Amazon CloudWatch console** – [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)
+ **AWS CLI** – For more information, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.
+ **CloudWatch API** – For more information, see the [Amazon CloudWatch API Reference](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/Welcome.html)\.
+ **AWS SDKs** – For more information, see [Tools for Amazon Web Services](http://aws.amazon.com/tools)\.

## Related AWS Services<a name="related_services"></a>

The following services are used along with Amazon CloudWatch:
+ **Amazon Simple Notification Service \(Amazon SNS\)** coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\. You use Amazon SNS with CloudWatch to send messages when an alarm threshold has been reached\. For more information, see [Setting Up Amazon SNS Notifications](US_SetupSNS.md)\.
+ **Amazon EC2 Auto Scaling** enables you to automatically launch or terminate Amazon EC2 instances based on user\-defined policies, health status checks, and schedules\. You can use a CloudWatch alarm with Amazon EC2 Auto Scaling to scale your EC2 instances based on demand\. For more information, see [Dynamic Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scale-based-on-demand.html) in the *Amazon EC2 Auto Scaling User Guide*\.
+ **AWS CloudTrail** enables you to monitor the calls made to the Amazon CloudWatch API for your account, including calls made by the AWS Management Console, AWS CLI, and other services\. When CloudTrail logging is turned on, CloudWatch writes log files to the Amazon S3 bucket that you specified when you configured CloudTrail\. For more information, see [Logging Amazon CloudWatch API Calls with AWS CloudTrail](logging_cw_api_calls.md)\.
+ **AWS Identity and Access Management \(IAM\)** is a web service that helps you securely control access to AWS resources for your users\. Use IAM to control who can use your AWS resources \(authentication\) and what resources they can use in which ways \(authorization\)\. For more information, see [Identity and Access Management for Amazon CloudWatch](auth-and-access-control-cw.md)\.
