# Document History<a name="DocumentHistory"></a>

The following table describes important changes in each release of the *Amazon CloudWatch User Guide*, beginning in June 2018\. For notification about updates to this documentation, you can subscribe to an RSS feed\. 

| Change | Description | Date | 
| --- |--- |--- |
| [CloudWatch agent section re\-organized](#DocumentHistory) | The CloudWatch agent documentation has been rewritten to improve clarity, especially for customers using the command line to install and configure the agent\. For more information, see [ Collecting Metrics and Logs from Amazon EC2 Instances and On\-Premises Servers with the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *Amazon CloudWatch User Guide*\. | March 28, 2019 | 
| [SEARCH function added to metric math expressions](#DocumentHistory) | You can now use a SEARCH function in metric math expressions\. This enables you to create dashboards that update automatically as new resources are created that match the search query\. For more information, see [ Using Search Expressions in Graphs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-search-expressions.html) in the *Amazon CloudWatch User Guide*\. | March 21, 2019 | 
| [AWS SDK Metrics for Enterprise Support](#DocumentHistory) | SDK Metrics helps you assess the health of your AWS services and diagnose latency caused by reaching your account usage limits or by a service outage\. For more information, see [ Monitor Applications Using AWS SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-SDK-Metrics.html) in the *Amazon CloudWatch User Guide*\. | December 11, 2018 | 
| [Alarms on math expressions](#DocumentHistory) | CloudWatch supports creating alarms based on metric math expressions\. For more information, see [ Alarms on Math Expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html#alarms-on-metric-math-expressions) in the *Amazon CloudWatch User Guide*\. | November 20, 2018 | 
| [New CloudWatch console home page](#DocumentHistory) | Amazon has created a new home page in the CloudWatch console, which automatically displays key metrics and alarms for all the AWS services you are using\. For more information, see [ Getting Started with Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/GettingStarted.html) in the *Amazon CloudWatch User Guide*\. | November 19, 2018 | 
| [AWS CloudFormation templates for the CloudWatch Agent](#DocumentHistory) | Amazon has uploaded AWS CloudFormation templates that you can use to install and update the CloudWatch agent\. For more information, see [ Install the CloudWatch Agent on New Instances Using AWS CloudFormation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent-New-Instances-CloudFormation.html) in the *Amazon CloudWatch User Guide*\. | November 9, 2018 | 
| [Enhancements to the CloudWatch Agent](#DocumentHistory) | The CloudWatch agent has been updated to work with both the StatsD and collectd protocols\. It also has improved cross\-account support\. For more information, see [Retrieve Custom Metrics with StatsD](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-custom-metrics-statsd.html), [Retrieve Custom Metrics with collectd](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-custom-metrics-collectd.html), and [ Sending Metrics and Logs to a Different AWS Account](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-common-scenarios.html#CloudWatch-Agent-send-to-different-AWS-account) in the *Amazon CloudWatch User Guide*\. | September 28, 2018 | 
| [Support for Amazon VPC endpoints](#DocumentHistory) | You can now establish a private connection between your VPC and CloudWatch\. For more information, see [Using CloudWatch with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-and-interface-VPC.html) in the *Amazon CloudWatch User Guide*\. | June 28, 2018 | 

The following table describes important changes to the *Amazon CloudWatch User Guide* before June 2018\.


| Change | Description | Release Date | 
| --- | --- | --- | 
|  Metric math  |  You can now perform math expressions on CloudWatch metrics, producing new time series that you can add to graphs on your dashboard\. For more information, see [Using Metric Math](using-metric-math.md)\.  | 4 April 2018 | 
|  "M out of N" alarms  |  You can now configure an alarm to trigger based on "M out of N" datapoints in any alarm evaluation interval\. For more information, see [Evaluating an Alarm](AlarmThatSendsEmail.md#alarm-evaluation)\.  | 8 December 2017 | 
|  CloudWatch agent  |  A new unified CloudWatch agent was released\. You can use the unified multi\-platform agent to collect custom both system metrics and log files from Amazon EC2 instances and on\-premises servers\. The new agent supports both Windows and Linux and enables customization of metrics collected, including sub\-resource metrics such as per\-CPU core\. For more information, see [Collecting Metrics and Logs from Amazon EC2 Instances and On\-Premises Servers with the CloudWatch Agent](Install-CloudWatch-Agent.md)\.  | 7 September 2017 | 
|  NAT gateway metrics  |  Added metrics for Amazon VPC NAT gateway\.  | 7 September 2017 | 
|  High\-resolution metrics  |  You can now optionally set up custom metrics as high\-resolution metrics, with a granularity of as low as one second\. For more information, see [High\-Resolution Metrics](publishingMetrics.md#high-resolution-metrics)\.  | 26 July 2017 | 
|  Dashboard APIs  |  You can now create, modify, and delete dashboards using APIs and the AWS CLI\. For more information, see [Create a CloudWatch Dashboard](create_dashboard.md)\.  | 6 July 2017 | 
|  AWS Direct Connect metrics  |  Added metrics for AWS Direct Connect\.  | 29 June 2017 | 
|  Amazon VPC VPN metrics  |  Added metrics for Amazon VPC VPN\.  | 15 May 2017 | 
|  AppStream 2\.0 metrics  |  Added metrics for AppStream 2\.0\.  | 8 March 2017 | 
|  CloudWatch console color picker  |  You can now choose the color for each metric on your dashboard widgets\. For more information, see [Edit a Graph on a CloudWatch Dashboard](edit_graph_dashboard.md)\.  | 27 February 2017 | 
|  Alarms on dashboards  |  Alarms can now be added to dashboards\. For more information, see [Add or Remove an Alarm from a CloudWatch Dashboard](add_remove_alarm_dashboard.md)\.  | 15 February 2017 | 
|  Added metrics for Amazon Polly  |  Added metrics for Amazon Polly\.  | 1 December 2016 | 
|  Added metrics for Amazon Kinesis Data Analytics  |  Added metrics for Amazon Kinesis Data Analytics\.  | 1 December 2016 | 
|  Added support for percentile statistics  |  You can specify any percentile, using up to two decimal places \(for example, p95\.45\)\. For more information, see [Percentiles](cloudwatch_concepts.md#Percentiles)\.  | 17 November 2016 | 
|  Added metrics for Amazon Simple Email Service  |  Added metrics for Amazon Simple Email Service\.  | 2 November 2016 | 
|  Updated metrics retention  |  Amazon CloudWatch now retains metrics data for 15 months instead of 14 days\.  | 1 November 2016 | 
|  Updated metrics console interface  |  The CloudWatch console is updated with improvements to existing functionality and new functionality\.  | 1 November 2016 | 
|  Added metrics for Amazon Elastic Transcoder  |  Added metrics for Amazon Elastic Transcoder\.  | 20 September 2016 | 
|  Added metrics for Amazon API Gateway  |  Added metrics for Amazon API Gateway\.  | 9 September 2016 | 
|  Added metrics for AWS Key Management Service  |  Added metrics for AWS Key Management Service\.  | 9 September 2016 | 
|  Added metrics for the new Application Load Balancers supported by Elastic Load Balancing  |  Added metrics for Application Load Balancers\.  | 11 August 2016 | 
|  Added new NetworkPacketsIn and NetworkPacketsOut metrics for Amazon EC2  |  Added new NetworkPacketsIn and NetworkPacketsOut metrics for Amazon EC2\.  | 23 March 2016 | 
|  Added new metrics for Amazon EC2 Spot fleet  |  Added new metrics for Amazon EC2 Spot fleet\.  | 21 March 2016 | 
|  Added new CloudWatch Logs metrics  |  Added new CloudWatch Logs metrics\.  | 10 March 2016 | 
|  Added Amazon Elasticsearch Service and AWS WAF metrics and dimensions  |  Added Amazon Elasticsearch Service and AWS WAF metrics and dimensions\.   | 14 October 2015 | 
|  Added support for CloudWatch dashboards  |  Dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those that are spread out across different regions\. For more information, see [Using Amazon CloudWatch Dashboards](CloudWatch_Dashboards.md)\.   | 8 October 2015 | 
|  Added AWS Lambda metrics and dimensions  |  Added AWS Lambda metrics and dimensions\.   | 4 September 2015 | 
|  Added Amazon Elastic Container Service metrics and dimensions  |  Added Amazon Elastic Container Service metrics and dimensions\.   | 17 August 2015 | 
|  Added Amazon Simple Storage Service metrics and dimensions  |  Added Amazon Simple Storage Service metrics and dimensions\.   | 26 July 2015 | 
|  New feature: Reboot alarm action  |  Added the reboot alarm action and new IAM role for use with alarm actions\. For more information, see [Create Alarms to Stop, Terminate, Reboot, or Recover an Instance](UsingAlarmActions.md)\.   | 23 July 2015 | 
|  Added Amazon WorkSpaces metrics and dimensions  |  Added Amazon WorkSpaces metrics and dimensions\.   | 30 April 2015 | 
|  Added Amazon Machine Learning metrics and dimensions  |  Added Amazon Machine Learning metrics and dimensions\.   | 9 April 2015 | 
|  New feature: Amazon EC2 instance recovery alarm actions  |  Updated alarm actions to include new EC2 instance recovery action\. For more information, see [Create Alarms to Stop, Terminate, Reboot, or Recover an Instance](UsingAlarmActions.md)\.   | 12 March 2015 | 
|  Added Amazon CloudFront and Amazon CloudSearch metrics and dimensions  |  Added Amazon CloudFront and Amazon CloudSearch metrics and dimensions\.  | 6 March 2015 | 
|  Added Amazon Simple Workflow Service metrics and dimensions  |  Added Amazon Simple Workflow Service metrics and dimensions\.   | 9 May 2014 | 
|  Updated guide to add support for AWS CloudTrail  |  Added a new topic to explain how you can use AWS CloudTrail to log activity in Amazon CloudWatch\. For more information, see [Logging Amazon CloudWatch API Calls with AWS CloudTrail](logging_cw_api_calls.md)\.  | 30 April 2014 | 
|  Updated guide to use the new AWS Command Line Interface \(AWS CLI\)  |  The AWS CLI is a cross\-service CLI with a simplified installation, unified configuration, and consistent command line syntax\. The AWS CLI is supported on Linux/Unix, Windows, and Mac\. The CLI examples in this guide have been updated to use the new AWS CLI\. For information about how to install and configure the new AWS CLI, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.  | 21 February 2014 | 
|  Added Amazon Redshift and AWS OpsWorks metrics and dimensions  |  Added Amazon Redshift and AWS OpsWorks metrics and dimensions\.   | 16 July 2013 | 
|  Added Amazon Route 53 metrics and dimensions  |  Added Amazon Route 53 metrics and dimensions\.   | 26 June 2013 | 
|  New feature: Amazon CloudWatch Alarm Actions  |  Added a new section to document Amazon CloudWatch alarm actions, which you can use to stop or terminate an Amazon Elastic Compute Cloud instance\. For more information, see [Create Alarms to Stop, Terminate, Reboot, or Recover an Instance](UsingAlarmActions.md)\.   | 8 January 2013 | 
|  Updated EBS metrics  |  Updated the EBS metrics to include two new metrics for Provisioned IOPS volumes\.   | 20 November 2012 | 
|  New billing alerts  |  You can now monitor your AWS charges using Amazon CloudWatch metrics and create alarms to notify you when you have exceeded the specified threshold\. For more information, see [Create a Billing Alarm to Monitor Your Estimated AWS Charges](monitor_estimated_charges_with_cloudwatch.md)\.   | 10 May 2012 | 
|  New metrics  |  You can now access six new Elastic Load Balancing metrics that provide counts of various HTTP response codes\.   | 19 October 2011 | 
|  New feature  |  You can now access metrics from Amazon EMR\.   | 30 June 2011 | 
|  New feature  |  You can now access metrics from Amazon Simple Notification Service and Amazon Simple Queue Service\.   | 14 July 2011 | 
|  New Feature  |  Added information about using the `PutMetricData` API to publish custom metrics\. For more information, see [Publishing Custom Metrics](publishingMetrics.md)\.  | 10 May 2011 | 
|  Updated metrics retention  |  Amazon CloudWatch now retains the history of an alarm for two weeks rather than six weeks\. With this change, the retention period for alarms matches the retention period for metrics data\.  | 07 April 2011 | 
|  New feature  |  Added ability to send Amazon Simple Notification Service or Auto Scaling notifications when a metric has crossed a threshold\. For more information, see [Alarms](cloudwatch_concepts.md#CloudWatchAlarms)\.  | 02 December 2010 | 
|  New feature  |  A number of CloudWatch actions now include the MaxRecords and NextToken parameters, which enable you to control pages of results to display\.  | 02 December 2010 | 
|  New feature  |  This service now integrates with AWS Identity and Access Management \(IAM\)\.  | 02 December 2010 | 