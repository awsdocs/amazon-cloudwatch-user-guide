# What Is Amazon CloudWatch Application Insights?<a name="appinsights-what-is"></a>

CloudWatch Application Insights helps you monitor your applications that use Amazon EC2 instances along with other [AWS application resources](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-what-is.html#appinsights-components)\. It identifies and sets up key metrics, logs, and alarms across your application resources and technology stack \(for example, your Microsoft SQL Server database, web \(IIS\) and application servers, OS, load balancers, and queues\)\. It continuously monitors metrics and logs to detect and correlate anomalies and errors\. When errors and anomalies are detected, Application Insights generates [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) that you can use to set up notifications or take actions\. To aid with troubleshooting, it creates automated dashboards for detected problems, which include correlated metric anomalies and log errors, along with additional insights to point you to a potential root cause\. The automated dashboards help you to take remedial actions to keep your applications healthy and to prevent impact to the end users of your application\. It also creates OpsItems so that you can resolve problems using [AWS Systems Manager OpsCenter](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html)\.

You can configure important counters, such as Mirrored Write Transaction/sec, Recovery Queue Length, and Transaction Delay, and Windows Event Logs on CloudWatch\. When a failover event or problem occurs with your SQL HA workload, such as a restricted access to query a target database, CloudWatch Application Insights can provide automated insights \.

CloudWatch Application Insights integrates with [AWS Launch Wizard](https://docs.aws.amazon.com/launchwizard/latest/userguide/what-is-launch-wizard.html) to provide a one\-click monitoring setup experience for deploying SQL Server HA workloads on AWS\. When you select the option to set up monitoring and insights with Application Insights on the Launch Wizard console, CloudWatch Application Insights automatically sets up relevant metrics, logs, and alarms on CloudWatch, and starts monitoring newly deployed workloads\. You can view automated insights and detected problems, along with the health of your SQL Server HA workloads, on the CloudWatch console\.

**Topics**
+ [Features](#appinsights-features)
+ [Concepts](#appinsights-concepts)
+ [Pricing](#appinsights-pricing)
+ [Related AWS services](#appinsights-related-services)
+ [Supported application components](#appinsights-components)
+ [Supported technology stack](#appinsights-stack)

## Features<a name="appinsights-features"></a>

Application Insights provides the following features\.

**Automatic set up of monitors for application resources**  
CloudWatch Application Insights reduces the time it takes to set up monitoring for your applications\. It does this by scanning your application resources, providing a customizable list of recommended metrics and logs, and setting them up on CloudWatch to provide necessary visibility into your application resources, such as Amazon EC2 and Elastic Load Balancers \(ELB\)\. It also sets up dynamic alarms on monitored metrics\. The alarms are automatically updated based on anomalies detected in the previous two weeks\. 

**Problem detection and notification**  
CloudWatch Application Insights detects signs of potential problems with your application, such as metric anomalies and log errors\. It correlates these observations to surface potential problems with your application\. It then generates CloudWatch Events, [which can be configured to receive notifications or take actions](appinsights-setting-up.md#appinsights-cloudwatch-events)\. This eliminates the need for you to create individual alarms on metrics or log errors\. 

**Troubleshooting**  
CloudWatch Application Insights creates CloudWatch automatic dashboards for problems that are detected\. The dashboards show details about the problem, including the associated metric anomalies and log errors to help you with troubleshooting\. They also provide additional insights that point to potential root causes of the anomalies and errors\. 

## Concepts<a name="appinsights-concepts"></a>

The following concepts are important for understanding how Application Insights monitors your application\.

**Component**  
An auto\-grouped, standalone, or custom grouping of similar resources that make up an application\. We recommend grouping similar resources into custom components for better monitoring\.

**Observation**  
An individual event \(metric anomaly, log error, or exception\) that is detected with an application or application resource\.

**Problem**  
Problems are detected by correlating, classifying, and grouping related observations\. 

For definitions of other key concepts for CloudWatch Application Insights, see [ Amazon CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)\.

## Pricing<a name="appinsights-pricing"></a>

CloudWatch Application Insights sets up recommended metrics and logs for selected application resources using CloudWatch Metrics, Logs, and CloudWatch Events for notifications on detected problems\. These features are charged to your AWS account according to [CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing)\. For the detected problems, it creates [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) and [automatic dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\. You are not charged for setup assistance, monitoring data analysis, or problem detection\. 

## Related AWS services<a name="appinsights-related-services"></a>

The following services are used along with CloudWatch Application Insights:
+ **Amazon CloudWatch **provides system‐wide visibility into resource utilization, application performance, and operational health\. It collects and tracks metrics, sends alarm notifications, automatically updates resources that you are monitoring based on the rules that you define, and allows you to monitor your own custom metrics\. CloudWatch Application Insights is initiated through CloudWatch—specifically, within the CloudWatch default operational dashboards\. For more information, see the [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\.
+ **AWS Resource Groups **help you to organize the resources that make up your application\. With Resource Groups, you can manage and automate tasks on a large number of resources at one time\. Only one Resource Group can be registered for a single application\. For more information, see the [https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)\.
+ **IAM **is a web service that helps you to securely control access to AWS resources for your users\. Use IAM to control who can use your AWS resources \(authentication\), and to control the resources they can use and how they can use them \(authorization\)\. For more information, see [Authentication and Access Control for Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/auth-and-access-control-cw.html)\.
+ **Amazon EC2 **provides scalable computing capacity in the AWS Cloud\. You can use Amazon EC2 to launch as many or as few virtual servers as you need, to configure security and networking, and to manage storage\. You can scale up or down to handle changes in requirements or spikes in popularity, which reduces your need to forecast traffic\. For more information, see the [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) or [https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html)\.
+ **Elastic Load Balancing **distributes incoming applications or network traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in multiple Availability Zones\. For more information, see the [https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)\.
+ **Amazon EC2 Auto Scaling **helps ensure that you have the correct number of EC2 instances available to handle the load for your application\. For more information, see the [https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)\.
+ **Amazon SQS **offers a secure, durable, and available hosted queue that allows you to integrate and decouple distributed software systems and components\. For more information, see the [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)\.
+ **AWS Systems Manager OpsCenter **aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation documents \(runbooks\) that you can use to quickly resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically\-generated summary reports about OpsItems by status and source\. For more information, see the [https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html)\.
+ **AWS Lambda** lets you build serverless applications composed of functions that are triggered by events and automatically deploy them using CodePipeline and AWS CodeBuild\. For more information, see [AWS Lambda Applications](https://docs.aws.amazon.com/lambda/latest/dg/deploying-lambda-apps.html)\. 
+ **Amazon DynamoDB** is a fully managed NoSQL database service that lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling\. DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data\.
+ **AWS Launch Wizard for SQL Server** reduces the time it takes to deploy SQL Server high availability solution to the cloud\. You input your application requirements, including performance, number of nodes, and connectivity on the service console, and AWS Launch Wizard identifies the right AWS resources to deploy and run your SQL Server Always On application\. 

## Supported application components<a name="appinsights-components"></a>

CloudWatch Application Insights scans your Resource Group—in this case, a group of resources used by a \.NET application—to identify application components\. Components can be standalone, auto\-grouped \(such as instances in an Auto Scaling group or behind a load balancer\), or custom \(by grouping together individual EC2 instances\)\. The following components are supported by CloudWatch Application Insights:
+ Amazon EC2
+ Amazon RDS
+ Elastic Load Balancing: Application Load Balancer and Classic Load Balancer \(all target instances of these load balancers are identified and configured\)\.
+ Amazon EC2 Auto Scaling groups: AWS Auto Scaling \(Auto Scaling groups are dynamically configured for all target instances; if your application scales up, CloudWatch Application Insights automatically configure the new instances\)\. Auto Scaling groups are not supported for CloudFormation\-based Resource Groups\. 
+ AWS Lambda
+ Amazon Simple Queue Service \(Amazon SQS\)
+ Amazon DynamoDB table
+ Amazon S3 bucket metrics
+ AWS Step Functions

Any other component type resources are ignored by CloudWatch Application Insights \. If a component type that is supported does not appear in your Application Insights application, the component may already be registered and managed by another application you own that is monitored by Application Insights\. 

## Supported technology stack<a name="appinsights-stack"></a>

You can use CloudWatch Application Insights to monitor your applications running on Windows Server and Linux operating systems by selecting **Custom** from the application tier dropdown menu in the console\. In addition, CloudWatch Application Insights, sets up monitors and generates more insights for applications built with the following technologies: 
+ Front‐end: Microsoft Internet Information Services \(IIS\) Web Server
+ Worker‐tier: \.NET Framework
+ Databases: 
  + Microsoft SQL Server running on Amazon RDS or Amazon EC2 \(including SQL Server High Availability configurations\. See, [Component configuration examples](component-configuration-examples.md)\)\.
  + MySQL running on Amazon RDS, Amazon Aurora, or Amazon EC2
  + PostgreSQL running on Amazon RDS or Amazon EC2
  + Amazon DynamoDB table

If none of the technology stacks listed above apply to your application resources, you can monitor your application stack by choosing **Custom** from the application tier dropdown menu on the **Manage monitoring** page\.