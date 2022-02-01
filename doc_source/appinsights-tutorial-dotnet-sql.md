# Tutorial: Set up monitoring for \.NET applications using SQL Server<a name="appinsights-tutorial-dotnet-sql"></a>

This tutorial demonstrates how to configure CloudWatch Application Insights to monitor an example solution and then simulate problem scenarios to test the solution\. In this example, a load balanced web application using SQL Server on the backend is deployed\. The web application and SQL Server are hosted on separate EC2 instances\.

**Topics**
+ [Use case scenario](#appinsights-tutorial-dotnet-sql-scenario)
+ [Prerequisites](#appinsights-tutorial-dotnet-sql-prereqs)
+ [Deploy resources for example scenario](#appinsights-tutorial-dotnet-sql-deploy-resources)
+ [Set up monitoring with Amazon CloudWatch Application Insights](#appinsights-tutorial-dotnet-sql-monitoring)
+ [Simulate problem scenarios and view insights](#appinsights-tutorial-dotnet-sql-problem-scenarios)

## Use case scenario<a name="appinsights-tutorial-dotnet-sql-scenario"></a>

In this scenario, a \.NET application using SQL Server on the backend runs on an Amazon EC2 instance\. The deployment is configured with two load\-balanced EC2 instances hosting the Barley Adventure Works application\. Both instances access SQL Server, which is hosted on a separate EC2 instance\. Monitors are set up with CloudWatch Application Insights to quickly identify, isolate, and resolve application issues\.

![\[Solution component diagram\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/app-insights-solution-components-v2.png)

## Prerequisites<a name="appinsights-tutorial-dotnet-sql-prereqs"></a>

To complete the steps in this tutorial, you must have an AWS account\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Deploy resources for example scenario<a name="appinsights-tutorial-dotnet-sql-deploy-resources"></a>

A CloudFormation template is provided to automate the deployment scenario for testing\. The template deploys the following instances:
+ An EC2 instance that hosts the Microsoft SQL Server database\.
+ Two load\-balanced EC2 instances\. Each load\-balanced instance hosts the Barley Adventure Works web application\.

The template deployment takes approximately 10 minutes to complete\.

**Steps to deploy the CloudFormation stack**

1. Choose **Create Stack**>**With new resources \(standard\)** from the AWS CloudFormation landing page at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/) to launch a CloudFormation stack in your account in the US East \(N\. Virginia\) Region\. This template is available only in the `us-east-1` Region\.

1. Select **Template is ready** on the **Create Stack** page\.

1. Under **Specify template**, select **Amazon S3 URL** and enter the following S3 URL path: `https://application-insights-demo-resources.s3.amazonaws.com/SampleApp.yml` \. Choose **Next**\.
**Note**  
This CloudFormation template can also be found in the aws\-samples GitHub repo at the following location: [https://github\.com/aws\-samples/application\-insights\-sample\-application/blob/master/SampleApp\.yml](https://github.com/aws-samples/application-insights-sample-application/blob/master/SampleApp.yml)\.

1. On the **Specify stack details** page, enter a name for the stack, such as `ApplicationInsightsTest`\.

1. Review the default parameters under **Parameters** and modify the values to your preferences\. Enter a password for SQLServer\. Verify that you follow the SQL Server password complexity requirements or the application will not work as expected\. Enter an existing EC2 key pair or create a new one in the EC2 console\. Choose **Next**\.

1. On the **Configure stack options** page, under **Tags**, optionally add tags to help you identify your stack\. Select **Next**\.

1. Review and confirm the settings on the **Review** page\. Select the box acknowledging that the template may create AWS Identity and Access Management \(IAM\) resources\.

1. Choose **Create stack** to deploy the stack\. Reminder that you can only deploy this template from the `us-east-1` Region\.

1. Monitor the status of the stack deployment from the **Events** tab of the Cloud Formation stack page\. When the stack is successfully deployed, continue to the next section\. 

## Set up monitoring with Amazon CloudWatch Application Insights<a name="appinsights-tutorial-dotnet-sql-monitoring"></a>

This section demonstrates how to create a resource group from the resources deployed by the CloudFormation template, and how to add the resource group to CloudWatch Application Insights for monitoring\. 

**Create resource group**

1. Navigate to the [AWS Resource Groups console](https://console.aws.amazon.com/resource-groups) and choose **Create resource group**\. 

1. On the **Create query\-based group** page, under **Group type**, select **CloudFormation stack based**\.

1. Under **Grouping criteria**, select the CloudFormation stack you created in the previous section \(`ApplicationInsightsTest`\) from the dropdown list\. Keep resource types as **All supported resource types**\.

1. Under **Group details**, enter a **Group name**, such as `application-insights-resource-group`, and an optional description of the resource group\. Then, choose **Create group**\.

**Set up resource group monitoring on CloudWatch Application Insights**

1. Navigate to the [Amazon CloudWatch console](https://console.aws.amazon.com/cloudwatch) and choose **Application Insights** under **Insights** on the left navigation pane\.

1. Choose **View applications** next to **Application Insights**\.

1. On the **Applications monitored** page, choose **Add an application**\.

1. Under **Resource Group selection**, select the resource group you created in the previous procedure \(`application-insights-resource-group`\) and choose **Add application**\.

1. On the **Overview** page, refresh your browser to display the application components in your resource group\.

1. Select the **Application Load Balancer group** and choose **Manage monitoring**\.

1. On the **Manage monitoring** page, select **Enable monitoring**\.

1. Choose **Save**\.

1. To enable monitoring for the SQL Server instance, select the SQL Server EC2 instance in the **Application components** section and repeat the previous steps for enabling monitoring from the **Manage monitoring** page\.

   The following metrics are monitored for the SQL Server instance:
   + CPUUtilization
   + StatusCheckFailed
   + Memory % Committed Bytes in Use
   + Memory Available Mbytes
   + Network Interface Bytes Total/sec
   + Paging File % Usage
   + Physical Disk % Disk Time
   + Processor % Processor Time
   + SQLServer:Buffer Manager cache hit ratio
   + SQLServer:Buffer Manager life expectancy
   + SQLServer:General Statistics Processes blocked
   + SQLServer:General Statistics User Connections
   + SQLServer:Locks Number of Deadlocks/sec
   + SQLServer:SQL Statistics Batch Requests/sec
   + System Processor Queue Length

   The following metrics are monitored for the volumes attached to the SQL Server instance:
   + VolumeReadBytes
   + VolumeWriteBytes
   + VolumeReadOps
   + VolumeWriteOps
   + VolumeTotalReadTime
   + VolumeTotalWriteTime
   + VolumeIdleTime
   + VolumeQueueLength
   + VolumeThroughputPercentage
   + VolumeConsumedReadWriteOps
   + BurstBalance

1. When monitoring is enabled for both the load balancer and the SQL Server instance, the resource group to which they belong displays a status of **Enabled** on the resource group **Overview ** page\.

## Simulate problem scenarios and view insights<a name="appinsights-tutorial-dotnet-sql-problem-scenarios"></a>

This section describes how to create a SQL login failure, a SQL memory pressure event, and an HTTP 500 error so that you can view the error details on the CloudWatch Application Insights dashboard\.

**Topics**
+ [Simulate SQL login failure](#appinsights-tutorial-dotnet-sql-login-failure)
+ [Simulate high memory pressure](#appinsights-tutorial-dotnet-sql-memory-pressure)
+ [Simulate an HTTP 500 error](#appinsights-tutorial-dotnet-sql-http-500-errors)

### Simulate SQL login failure<a name="appinsights-tutorial-dotnet-sql-login-failure"></a>

To simulate a SQL login failure and view the problem from the CloudWatch dashboard, perform the following steps\.

1. Log in to the EC2 instance provisioned for your SQL Server instance \(M4 instance type\) using the key pair you selected when you created the CloudFormation stack\.

1. From the **Start** menu, launch SQL Management Studio\.

1. Enter a username and an incorrect password and choose **Connect**\. A message appears indicating that the login has failed\. Repeat this step a few more times\.

1. From the CloudWatch Application Insights **Problems detected** page \(at the bottom of the [CloudWatch console landing page](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1)\), the error should appear under the problem summary as **SQL: Login Failure**\. To see more details about the problem, select the problem link\.

### Simulate high memory pressure<a name="appinsights-tutorial-dotnet-sql-memory-pressure"></a>

To simulate a high memory pressure event, which can lead to application performance degradation and timeout errors on the web servers and load balancers, perform the following steps\.

1. Launch SQL Management Studio from the SQL server instance and login using the Windows Administrator account\.

1. Right\-click on the database server, choose **Properties**, and select **Memory**\.

1. Under the **Server memory options**, reduce the **Maximum server memory** to 256 KB\.

1. In a new query window, run the following SQL query:

   ```
   SELECT COUNT(*) 
   FROM [AdventureWorks2016].[Sales].[Customer]
   ```

   A message appears indicating that there is insufficient memory to run the query\.

1. From the CloudWatch Application Insights **Problems detected** page \(at the bottom of the [CloudWatch console landing page](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1)\), the error should appear under the problem summary as **SQL: Memory Pressure**\. To see more details about the problem, select the problem link\.

### Simulate an HTTP 500 error<a name="appinsights-tutorial-dotnet-sql-http-500-errors"></a>

HTTP requests for an unhandled HTTP request to a web application results in an HTTP 500 error\. To simulate an HTTP 500 error, perform the following steps\.

1. From the AWS Management Console, navigate to the AWS CloudFormation console\.

1. Choose the **Outputs** tab of the `AppInsightsTest` CloudFormation stack that you previously launched\.

1. Open the URL displayed under **Value** for the AdventureWorks application in a web browser\.

1. Navigate to the customer details page of the Barley Adventure Works web application by suffixing the previously mentioned URL with `sampleapp/SalesOrderDetails/edit/5`\.

1. Refresh the compiled URL request several times\. Your URL should look something like this: `http://<YourURL>.us-east-1.elb.amazonaws.com/sampleapp/SalesOrderDetails/edit/5` \.

   An error message appears indicating that the file or directory cannot be found\.

1. From the CloudWatch Application Insights **Problems detected** page \(at the bottom of the [CloudWatch console landing page](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1)\), the error should appear under the problem summary as **ALB: Backend 5XX errors**\. To see more details about the problem, select the problem link\.