# What is Amazon CloudWatch Application Insights?<a name="appinsights-what-is"></a>

CloudWatch Application Insights helps you monitor your applications that use Amazon EC2 instances along with other [application resources](#appinsights-components)\. It identifies and sets up key metrics, logs, and alarms across your application resources and technology stack \(for example, your Microsoft SQL Server database, web \(IIS\) and application servers, OS, load balancers, and queues\)\. It continuously monitors metrics and logs to detect and correlate anomalies and errors\. When errors and anomalies are detected, Application Insights generates [CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html) that you can use to set up notifications or take actions\. To assist with troubleshooting, it creates automated dashboards for detected problems, which include correlated metric anomalies and log errors, along with additional insights to point you to a potential root cause\. The automated dashboards help you to take remedial actions to keep your applications healthy and to prevent impact to the end\-users of your application\. It also creates OpsItems so that you can resolve problems using [AWS SSM OpsCenter](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html)\.

You can configure important counters, such as Mirrored Write Transaction/sec, Recovery Queue Length, and Transaction Delay, as well as Windows Event Logs on CloudWatch\. When a failover event or problem occurs with your SQL HA workload, such as a restricted access to query a target database, CloudWatch Application Insights provides automated insights \.

CloudWatch Application Insights integrates with [AWS Launch Wizard](https://docs.aws.amazon.com/launchwizard/latest/userguide/what-is-launch-wizard.html) to provide a one\-click monitoring setup experience for deploying SQL Server HA workloads on AWS\. When you select the option to set up monitoring and insights with Application Insights on the [Launch Wizard console](https://console.aws.amazon.com/launchwizard), CloudWatch Application Insights automatically sets up relevant metrics, logs, and alarms on CloudWatch, and starts monitoring newly deployed workloads\. You can view automated insights and detected problems, along with the health of your SQL Server HA workloads, on the CloudWatch console\.

**Topics**
+ [Features](#appinsights-features)
+ [Concepts](#appinsights-concepts)
+ [Pricing](#appinsights-pricing)
+ [Related services](#appinsights-related-services)
+ [Supported application components](#appinsights-components)
+ [Supported technology stacks](#appinsights-stack)

## Features<a name="appinsights-features"></a>

Application Insights provides the following features\.

**Automatic set up of monitors for application resources**  
CloudWatch Application Insights reduces the time it takes to set up monitoring for your applications\. It does this by scanning your application resources, providing a customizable list of recommended metrics and logs, and setting them up on CloudWatch to provide necessary visibility into your application resources, such as Amazon EC2 and Elastic Load Balancers \(ELB\)\. It also sets up dynamic alarms on monitored metrics\. The alarms are automatically updated based on anomalies detected in the previous two weeks\. 

**Problem detection and notification**  
CloudWatch Application Insights detects signs of potential problems with your application, such as metric anomalies and log errors\. It correlates these observations to surface potential problems with your application\. It then generates CloudWatch Events, [which can be configured to receive notifications or take actions](appinsights-cloudwatch-events.md)\. This eliminates the need for you to create individual alarms on metrics or log errors\. 

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

## Related services<a name="appinsights-related-services"></a>

The following services are used along with CloudWatch Application Insights:

**Related AWS services**
+ **Amazon CloudWatch** provides system‐wide visibility into resource utilization, application performance, and operational health\. It collects and tracks metrics, sends alarm notifications, automatically updates resources that you are monitoring based on the rules that you define, and allows you to monitor your own custom metrics\. CloudWatch Application Insights is initiated through CloudWatch—specifically, within the CloudWatch default operational dashboards\. For more information, see the [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\.
+ **CloudWatch Container Insights** collects, aggregates, and summarizes metrics and logs from your containerized applications and microservices\. You can use Container Insights to monitor Amazon ECS, Amazon Elastic Kubernetes Service, and Kubernetes platforms on Amazon EC2\. When Application Insights is enabled on the Container Insights or Application Insights consoles, Application Insights displays detected problems on your Container Insights dashboard\. For more information, see [Using Container Insights](ContainerInsights.md) \.
+ **Amazon DynamoDB** is a fully managed NoSQL database service that lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling\. DynamoDB also offers encryption at rest, which eliminates the operational burden and complexity involved in protecting sensitive data\.
+ **Amazon EC2 **provides scalable computing capacity in the AWS Cloud\. You can use Amazon EC2 to launch as many or as few virtual servers as you need, to configure security and networking, and to manage storage\. You can scale up or down to handle changes in requirements or spikes in popularity, which reduces your need to forecast traffic\. For more information, see the [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) or [https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html)\.
+ **Amazon Elastic Block Store \(Amazon EBS\)** provides block\-level storage volumes for use with Amazon EC2 instances\. Amazon EBS volumes behave like raw, unformatted block devices\. You can mount these volumes as devices on your instances\. Amazon EBS volumes that are attached to an instance are exposed as storage volumes that persist independently from the life of the instance\. You can create a file system on top of these volumes, or use them in any way you would use a block device \(such as a hard drive\)\. You can dynamically change the configuration of a volume attached to an instance\. For more information, see the [Amazon EBS User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)\.
+ **Amazon EC2 Auto Scaling **helps ensure that you have the correct number of EC2 instances available to handle the load for your application\. For more information, see the [https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)\.
+ **Elastic Load Balancing **distributes incoming applications or network traffic across multiple targets, such as EC2 instances, containers, and IP addresses, in multiple Availability Zones\. For more information, see the [https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)\.
+ **IAM **is a web service that helps you to securely control access to AWS resources for your users\. Use IAM to control who can use your AWS resources \(authentication\), and to control the resources they can use and how they can use them \(authorization\)\. For more information, see [Authentication and Access Control for Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/auth-and-access-control-cw.html)\.
+ **AWS Lambda** lets you build serverless applications composed of functions that are triggered by events and automatically deploy them using CodePipeline and AWS CodeBuild\. For more information, see [AWS Lambda Applications](https://docs.aws.amazon.com/lambda/latest/dg/deploying-lambda-apps.html)\. 
+ **AWS Launch Wizard for SQL Server** reduces the time it takes to deploy SQL Server high availability solution to the cloud\. You input your application requirements, including performance, number of nodes, and connectivity on the service console, and AWS Launch Wizard identifies the right AWS resources to deploy and run your SQL Server Always On application\. 
+ **AWS Resource Groups **help you to organize the resources that make up your application\. With Resource Groups, you can manage and automate tasks on a large number of resources at one time\. Only one Resource Group can be registered for a single application\. For more information, see the [https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)\.
+ **Amazon SQS **offers a secure, durable, and available hosted queue that allows you to integrate and decouple distributed software systems and components\. For more information, see the [https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)\.
+ **AWS Step Functions** is a serverless function composer that allows you to sequence a variety of AWS services and resources, including AWS Lambda functions, into structured, visual workflows\. For more information, see the [AWS Step Functions User Guide](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)\.
+ **AWS SSM OpsCenter **aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation documents \(runbooks\) that you can use to quickly resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically\-generated summary reports about OpsItems by status and source\. For more information, see the [https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html)\.
+ **Amazon API Gateway** is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale\. API developers can create APIs that access AWS or other web services, as well as data stored in the AWS Cloud\. For more information, see the [Amazon API Gateway User Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)\.
**Note**  
Application Insights supports only REST API protocols \(v1 of the API Gateway service\)\.
+ **Amazon Elastic Container Service \(Amazon ECS\)** is a fully managed container orchestration service\. You can use Amazon ECS to run your most sensitive and mission\-critical applications\. For more information, see the [Amazon Elastic Container Service Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)\.
+ **Amazon Elastic Kubernetes Service \(Amazon EKS\)** is a managed service that you can use to run Kubernetes on AWS without having to install, operate, and maintain your own Kubernetes control plane or nodes\. Kubernetes is an open\-source system for automating the deployment, scaling, and management of containerized applications\. For more information, see the [Amazon EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)\.
+ **Kubernetes on Amazon EC2**\. Kubernetes is open\-source software that helps you deploy and manage containerized applications at scale\. Kubernetes manages clusters of Amazon EC2 compute instances and runs containers on those instances with processes for deployment, maintenance, and scaling\. With Kubernetes you can run any type of containerized application with the same toolset on\-premises and in the cloud\. For more information, see [Running Kubernetes on AWS EC2](https://v1-18.docs.kubernetes.io/docs/setup/production-environment/turnkey/aws/)\.
+ **Amazon FSx** helps you to launch and run popular file systems that are fully managed by AWS\. With Amazon FSx, you can leverage the feature sets and performance of common open source and commercially\-licensed file systems to avoid time\-consuming administrative tasks\. For more information, see the [Amazon FSx Documentation](https://docs.aws.amazon.com/fsx/)\.
+ **Amazon Simple Notification Service \(SNS\)** is a fully\-managed messaging service for both application\-to\-application and application\-to\-person communication\. You can configure Amazon SNS for monitoring by Application Insights\. When Amazon SNS is configured as a resource for monitoring, Application Insights tracks SNS metrics to help determine why SNS messages may encounter issues or fail\.

**Related third\-party services**
+ For some workloads and applications monitored in Application Insights, **Prometheus JMX exporter** is installed using AWS Systems Manager Distributor so that CloudWatch Application Insights can retrieve Java\-specific metrics\. When you choose to monitor a Java application, Application Insights automatically installs the Prometheus JMX exporter for you\. 

## Supported application components<a name="appinsights-components"></a>

CloudWatch Application Insights scans your resource group to identify application components\. Components can be standalone, auto\-grouped \(such as instances in an Auto Scaling group or behind a load balancer\), or custom \(by grouping together individual EC2 instances\)\. 

The following components are supported by CloudWatch Application Insights:

**AWS components**
+ Amazon EC2
+ Amazon EBS
+ Amazon RDS
+ Elastic Load Balancing: Application Load Balancer and Classic Load Balancer \(all target instances of these load balancers are identified and configured\)\.
+ Amazon EC2 Auto Scaling groups: AWS Auto Scaling \(Auto Scaling groups are dynamically configured for all target instances; if your application scales up, CloudWatch Application Insights automatically configure the new instances\)\. Auto Scaling groups are not supported for CloudFormation\-based Resource Groups\. 
+ AWS Lambda
+ Amazon Simple Queue Service \(Amazon SQS\)
+ Amazon DynamoDB table
+ Amazon S3 bucket metrics
+ AWS Step Functions
+ Amazon API Gateway REST API stages
+ Amazon Elastic Container Service \(Amazon ECS\): cluster, service, and task
+ Amazon Elastic Kubernetes Service \(Amazon EKS\): cluster
+ Kubernetes on Amazon EC2: Kubernetes cluster running on EC2
+ Amazon SNS topic

Any other component type resources are not currently tracked by CloudWatch Application Insights\. If a component type that is supported does not appear in your Application Insights application, the component may already be registered and managed by another application you own that is monitored by Application Insights\. 

## Supported technology stacks<a name="appinsights-stack"></a>

You can use CloudWatch Application Insights to monitor your applications running on Windows Server and Linux operating systems by selecting the application tier dropdown menu option for one of the following technologies: 
+ Front‐end: Microsoft Internet Information Services \(IIS\) Web Server
+ Worker‐tier: 
  + \.NET Framework 
  + \.NET Core
+ Applications: Java
+ Active Directory
+ SharePoint
+ Databases: 
  + Microsoft SQL Server running on Amazon RDS or Amazon EC2 \(including SQL Server High Availability configurations\. See, [Component configuration examples](component-configuration-examples.md)\)\.
  + MySQL running on Amazon RDS, Amazon Aurora, or Amazon EC2
  + PostgreSQL running on Amazon RDS or Amazon EC2
  + Amazon DynamoDB table
  + Oracle running on Amazon RDS or Amazon EC2
  + SAP HANA database on a single Amazon EC2 instance and multiple EC2 instances
  + Cross\-AZ SAP HANA database high availability setup\.

If none of the technology stacks listed above apply to your application resources, you can monitor your application stack by choosing **Custom** from the application tier dropdown menu on the **Manage monitoring** page\.