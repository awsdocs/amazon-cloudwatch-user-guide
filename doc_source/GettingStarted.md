# Getting Started with Amazon CloudWatch<a name="GettingStarted"></a>

Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

The CloudWatch overview home page appears\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/monitoring-overviewpage-console.PNG)

The overview displays the following items, refreshed automatically\.
+ The upper left shows a list of AWS services you use in your account, along with the state of alarms in those services\. The upper right shows two or four alarms in your account, depending on how many AWS services you use\. The alarms shown are those in the ALARM state or those that most recently changed state\.

  These upper areas enable you to assess the health of your AWS services, by seeing the alarm states in every service and the alarms that most recently changed state\. This helps you monitor and quickly diagnose issues\.
+ Below these areas is the custom dashboard that you have created and named **CloudWatch\-Default**, if any\. This is a convenient way for you to add metrics about your own custom services or applications to the overview page, or to bring forward additional key metrics from AWS services that you most want to monitor\.
+ If you use six or more AWS services, below the default dashboard is a link to the automatic cross\-service dashboard\. The cross\-service dashboard automatically displays key metrics from every AWS service you use, without requiring you to choose what metrics to monitor or create custom dashboards\. You can also use it to drill down to any AWS service and see even more key metrics for that service\.

  If you use fewer than six AWS services, the cross\-service dashboard is shown automatically on this page\.

From this overview, you can focus your view to a specific resource group or a specific AWS service\. This enables you to narrow your view to a subset of resources in which you are interested\. Using resource groups enables you to use tags to organize projects, focus on a subset of your architecture, or just distinguish between your production and development environments\. For more information, see [What Is AWS Resource Groups?](welcome.html)\.

**Topics**
+ [See Key Metrics From All AWS Services](CloudWatch_Automatic_Dashboards_Cross_Service.md)
+ [Focus on Metrics and Alarms in a Single AWS Service](CloudWatch_Automatic_Dashboards_Focus_Service.md)
+ [Focus on Metrics and Alarms in a Resource Group](CloudWatch_Automatic_Dashboards_Resource_Group.md)