# Set up, configure, and manage your application for monitoring<a name="appinsights-setting-up"></a>

This section provides steps for setting up, configuring, and managing your CloudWatch Application Insights application using the console, the AWS CLI, and AWS Tools for Windows PowerShell\.

**Topics**
+ [Console steps](#appinsights-setting-up-console)
+ [Command line steps](#appinsights-setting-up-command)
+ [Set up notifications](#appinsights-cloudwatch-events)

## Set up, configure, and manage your application for monitoring from the CloudWatch console<a name="appinsights-setting-up-console"></a>

This section provides steps for setting up, configuring, and managing your application for monitoring from the CloudWatch console\.

**Topics**
+ [Add and configure an application](#appinsights-add-configure)
+ [Disable an application](#appinsights-disable-app)
+ [Disable monitoring for an application component](#appinsights-disable-monitoring)
+ [Delete an application](#appinsights-delete-app)

### Add and configure an application<a name="appinsights-add-configure"></a>

**Add and configure an application from the CloudWatch console**  
To get started with CloudWatch Application Insights from the CloudWatch console, follow these steps\.

1. **Start\.** Open the [CloudWatch console landing page](http://console.aws.amazon.com/cloudwatch)\. From the left navigation pane, choose **Settings**\. From the **Settings** page, choose **Application Insights** > **View applications to get started**\. 

1. **Add an Application\.** To set up monitoring for your \.NET and SQL Server application, on the CloudWatch Application Insights page, select **Add an application**\. This page shows the list of applications that are monitored with CloudWatch Application Insights, along with their monitoring status\. After you select **Add an application**, you will be taken to the **Add an application** page\.

1. **Select Resource Group\. **On the **Add an application** page, to add an application to CloudWatch Application Insights, choose an AWS Resource Group from the dropdown list that contains your application resources\. These resources include front\-end servers, load balancers, auto scaling groups, and database servers\. 

   An [ARN](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) will be generated for the application in the following format:

   ```
   arn:partition:applicationinsights:region:account-id:application/resource-group/resource-group-name
   ```

   For example:

   ```
   arn:aws:applicationinsights:us-east-1:123456789012:application/resource-group/my-resource-group
   ```

   CloudWatch Application Insights supports both tag\-based and CloudFormation\-based Resource Groups \(with the exception of Auto Scaling groups\)\. For more information, see [Working with Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html)\.

   If you have not created a Resource Group for your \.NET application, you can create one\. For more information, see the [https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)\. 

1. **Add Monitoring Details\.** After you add an application, you are taken to the **Monitoring Details** page, which lists the application components, resources in those components, and their monitoring status\. Components are auto\-grouped, standalone, or custom groupings of similar resources that make up an application\. By default, CloudWatch Application Insights groups instances that are in Auto Scaling groups, and instances that are behind your Elastic Load Balancers\. On this page, you can configure custom components and can manage monitoring for each application component\. For supported components, see [Supported application components](appinsights-what-is.md#appinsights-components)\.

1. **Configure components\.** After selecting a Resource Group, you are prompted to configure [components](appinsights-what-is.md#components)\. We recommend grouping similar resources, such as \.NET web server instances, into custom components for easier onboarding and better monitoring and insights\. By default, CloudWatch Application Insights groups instances that are in Auto Scaling groups, and instances that are behind your Elastic Load Balancers\. For supported components, see [Supported application components](appinsights-what-is.md#appinsights-components)\.

   Under **Application components**, for each component for which you want to set up monitors, select the component and select **Manage Monitoring**\.

1. **Enable Monitors\.** To set up monitoring for an application component, select the components that you want to monitor and then choose **Manage Monitoring**\. Select the **Enable Monitoring** check box\. When you select the check box, the dropdown populates with the relevant application tiers\. Choose the application tier for the selected component\. The tiers indicate the part of the application stack running on the selected resources\. If you select a custom tier, Application Insights recommends monitors based on the operating system\. You can customize the list of metrics and logs, and add custom application logs and log patterns to detect\. 

   Based on your tier selection, CloudWatch Application Insights makes recommendations for logs to monitor for the selected component\. This recommendation can be customized according to your needs\.

   For application\-specific logs, including for Microsoft SQL Server Error logs and IIS logs, verify the default log path \(if any\) or enter the correct log location in your EC2 instance\. 

   You can also choose to add Windows Event Logs, including Windows Logs and Applications and Services Logs\. To do this, enter the types of events you want to store and analyze\. Then, specify all of the event levels \(critical, error, warning, informational, or verbose\) that you want to store in your CloudWatch account\. 

   You can add a log group for storing and grouping each of these logs on your CloudWatch account, which also facilitates searches\. 

   CloudWatch Application Insights also sets up relevant metrics for your application resources\. They are monitored for approximately two weeks to identify the appropriate metrics thresholds\. If you have created the metrics in the past, CloudWatch Application Insights pulls historical data for the last two weeks to identify the thresholds and to set the alarms accordingly\. For newly created metrics, it may take up to three days before alarms are created\. You can also monitor your application resources using the CloudWatch alarms you created in your account\. 

1. **Save monitors\.** When you are finished selecting and customizing logs and metrics, select **Save** to set up monitors for the selected component\. When you select **Save**, Application Insights sets up the CloudWatch Agent configuration files for all of the instances in your application based on the recommended metrics and your selection of logs\. It can take up to an hour for this process to complete\. 

   CloudWatch Application Insights also sets up CloudWatch alarms for selected metrics in the component\. The alarms are dynamically updated by monitoring historical metric patterns from the past two weeks\. 

    When you select **Cancel**, Application Insights only deletes your current selections\. 

   When you create a new application with CloudWatch Application Insights, the service‐linked role is created for you\. To delete the service‐linked role, you must first delete all of your applications on CloudWatch Application Insights and then manually delete the role\. For more information, see [Using Service\-Linked Roles for CloudWatch Application Insights for \.NET and SQL Server](CHAP_using-service-linked-roles-appinsights.md)\.

   CloudWatch Application Insights is now set to monitor metrics and logs for your application\. It may take up to two weeks for the system to generate meaningful insights\.

   \*If your Resource Group is already configured and you want to save your configuration but don’t want CloudWatch Application Insights to monitor your application, you can disable CloudWatch Application Insights\. You can also delete your configuration\.

1. **Add AWS Systems Manager OpsCenter Integration\.** To view and get notified when problems are detected for selected applications, select the **Integrate with AWS OpsCenter** check box on the **Monitoring Details** page\. To track the operations that are taken to resolve operational work items \(OpsItems\) that are related to your AWS resources, provide the SNS topic ARN\. 

1. **View monitoring \(optional\)\.** After your application has been set up for monitoring, you can view and troubleshoot detected problems and insights in the default overview page of the CloudWatch console\. You can view detected problems, alarms, and dashboards by selecting **View Insights** from the Application Insights landing page, or on the CloudWatch landing page\. 

### Disable an application<a name="appinsights-disable-app"></a>

To disable an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to disable\. Under **Actions**, choose **Disable**\. When you disable an application, monitoring is disabled, but Application Insights stores the saved monitors for application components\. 

### Disable monitoring for an application component<a name="appinsights-disable-monitoring"></a>

To disable monitoring for an application component, from the Application Details page select the component for which you want to disable monitoring\. Choose **Manage Monitors**, and then clear the **Enable Monitoring** check box\. 

### Delete an application<a name="appinsights-delete-app"></a>

To delete an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to delete\. Under **Actions**, choose **Delete**\. This deletes monitoring and deletes all of the saved monitors for application components\. The application resources are not deleted\. 

## Set up, configure, and manage your application for monitoring using the command line<a name="appinsights-setting-up-command"></a>

This section provides steps for setting up, configuring, and managing your application for monitoring using the AWS CLI and AWS Tools for Windows PowerShell\.

**Topics**
+ [Add and manage an application](#appinsights-config-app-command)
+ [Manage and update monitoring](#appinsights-monitoring)
+ [Configure monitoring for SQL Always On Availability Groups](#configure-sql)
+ [Configure monitoring for MySQL RDS](#configure-mysql-rds)
+ [Configure monitoring for MySQL EC2](#configure-mysql-ec2)
+ [Configure monitoring for PostgreSQL RDS](#configure-postgresql-rds)
+ [Configure monitoring for PostgreSQL EC2](#configure-postgresql-ec2)

### Add and manage an application<a name="appinsights-config-app-command"></a>

You can add, get information about, manage, and configure your Application Insights application using the command line\. 

**Topics**
+ [Add an application](#appinsights-add-app)
+ [Describe an application](#appinsights-describe-app)
+ [List components in an application](#appinsights-list-components)
+ [Describe a component](#appinsights-describe-components)
+ [Group similar resources into a custom component](#appinsights-group-resources-components)
+ [Ungroup a custom component](#appinsights-ungroup-resources-components)
+ [Update an application](#appinsights-update-app)
+ [Update a custom component](#appinsights-update-component)

#### Add an application<a name="appinsights-add-app"></a>

**Add an application using the AWS CLI**  
To use the AWS CLI to add an application for your resource group called `my-resource-group`, with OpsCenter enabled to deliver the created opsItem to the SNS topic ARN ` arn:aws:sns:us-east-1:123456789012:MyTopic`, use the following command\.

```
aws application-insights create-application --resource-group-name my-resource-group --ops-center-enabled --ops-item-sns-topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic
```

**Add an application using AWS Tools for Windows PowerShell**  
To use AWS Tools for Windows PowerShell to add an application for your resource group called `my-resource-group` with OpsCenter enabled to deliver the created opsItem to the SNS topic ARN `arn:aws:sns:us-east-1:123456789012:MyTopic`, use the following command\.

```
New-CWAIApplication -ResourceGroupName my-resource-group -OpsCenterEnabled true -OpsItemSNSTopicArn arn:aws:sns:us-east-1:123456789012:MyTopic
```

#### Describe an application<a name="appinsights-describe-app"></a>

**Describe an application using the AWS CLI**  
To use the AWS CLI to describe an application created on a resource group called `my-resource-group`, use the following command\.

```
aws application-insights describe-application --resource-group-name my-resource-group
```

**Describe an application using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to describe an application created on a resource group called `my-resource-group`, use the following command\.

```
Get-CWAIApplication -ResourceGroupName my-resource-group
```

#### List components in an application<a name="appinsights-list-components"></a>

**List components in an application using the AWS CLI**  
To use the AWS CLI to list the components created on a resource group called `my-resource-group`, use the following command\.

```
aws application-insights list-components --resource-group-name my-resource-group
```

**List components in an application using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to list the components created on a resource group called `my-resource-group`, use the following command\.

```
Get-CWAIComponentList -ResourceGroupName my-resource-group
```

#### Describe a component<a name="appinsights-describe-components"></a>

**Describe a component using the AWS CLI**  
You can use the following AWS CLI command to describe a component called `my-component` that belongs to an application created on a resource group called `my-resource-group`\.

```
aws application-insights describe-component --resource-group-name my-resource-group --component-name my-component
```

**Describe a component using AWS Tools for Windows PowerShell**  
You can use the following AWS Tools for Windows PowerShell command to describe a component called `my-component` that belongs to an application created on a resource group called `my-resource-group`\.

```
Get-CWAIComponent -ComponentName my-component -ResourceGroupName my-resource-group
```

#### Group similar resources into a custom component<a name="appinsights-group-resources-components"></a>

We recommend grouping similar resources, such as \.NET web server instances, into custom components for easier onboarding and better monitoring and insights\. Currently, CloudWatch Application Insights supports custom groups for EC2 instances\.

**To group resources into a custom component using the AWS CLI**  
To use the AWS CLI to group three instances \(`arn:aws:ec2:us-east-1:123456789012:instance/i-11111`, `arn:aws:ec2:us-east-1:123456789012:instance/i-22222`, and `arn:aws:ec2:us-east-1:123456789012:instance/i-33333`\) together into a custom component called `my-component` for an application created for the resource group called `my-resource-group`, use the following command\. 

```
aws application-insights create-component --resource-group-name my-resource-group --component-name my-component --resource-list arn:aws:ec2:us-east-1:123456789012:instance/i-11111 arn:aws:ec2:us-east-1:123456789012:instance/i-22222 arn:aws:ec2:us-east-1:123456789012:instance/i-33333
```

**To group resources into a custom component using AWS Tools for Windows PowerShell**  
To use AWS Tools for Windows PowerShell to group three instances \(`arn:aws:ec2:us-east-1:123456789012:instance/i-11111`, `arn:aws:ec2:us-east-1:123456789012:instance/i-22222`, and `arn:aws:ec2:us-east-1:123456789012:instance/i-33333`\) together into a custom component called `my-component`, for an application created for the resource group called `my-resource-group`, use the following command\.

```
New-CWAIComponent -ResourceGroupName my-resource-group -ComponentName my-component -ResourceList arn:aws:ec2:us-east-1:123456789012:instance/i-11111,arn:aws:ec2:us-east-1:123456789012:instance/i-22222,arn:aws:ec2:us-east-1:123456789012:instance/i-33333 
```

#### Ungroup a custom component<a name="appinsights-ungroup-resources-components"></a>

**To ungroup a custom component using the AWS CLI**  
To use the AWS CLI to ungroup a custom component named `my-component` in an application created on the resource group, `my-resource-group`, use the following command\. 

```
aws application-insights delete-component --resource-group-name my-resource-group --component-name my-new-component
```

**To ungroup a custom component using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to ungroup a custom component named `my-component` in an application created on the resource group, `my-resource-group`, use the following command\.

```
Remove-CWAIComponent -ComponentName my-component -ResourceGroupName my-resource-group
```

#### Update an application<a name="appinsights-update-app"></a>

**Update an application using the AWS CLI**  
You can use the AWS CLI to update an application to generate AWS Systems Manager OpsCenter OpsItems for problems detected with the application, and to associate the created OpsItems to the SNS topic `arn:aws:sns:us-east-1:123456789012:MyTopic`, using the following command\.

```
aws application-insights update-application --resource-group-name my-resource-group --ops-center-enabled --ops-item-sns-topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic
```

**Update an application using AWS Tools for Windows PowerShell**  
You can use the AWS Tools for Windows PowerShell to update an application to generate AWS Systems Manager OpsCenter OpsItems for problems detected with the application, and to associate the created OpsItems to the SNS topic `arn:aws:sns:us-east-1:123456789012:MyTopic`, using the following command\.

```
Update-CWAIApplication -ResourceGroupName my-resource-group -OpsCenterEnabled true -OpsItemSNSTopicArn arn:aws:sns:us-east-1:123456789012:MyTopic
```

#### Update a custom component<a name="appinsights-update-component"></a>

**Update a custom component using the AWS CLI**  
You can use the AWS CLI to update a custom component called `my-component` with a new component name, `my-new-component`, and an updated group of instances, by using the following command\.

```
aws application-insights update-component --resource-group-name my-resource-group --component-name my-component --new-component-name my-new-component --resource-list arn:aws:ec2:us-east-1:123456789012:instance/i-44444 arn:aws:ec2:us-east-1:123456789012:instance/i-55555
```

**Update a custom component using AWS Tools for Windows PowerShell**  
You can use the AWS Tools for Windows PowerShell to update a custom component called `my-component` with a new component name, `my-new-component`, and an updated group of instances, by using the following command\.

```
Update-CWAIComponent -ComponentName my-component -NewComponentName my-new-component -ResourceGroupName my-resource-group -ResourceList arn:aws:ec2:us-east-1:123456789012:instance/i-44444,arn:aws:ec2:us-east-1:123456789012:instance/i-55555
```

### Manage and update monitoring<a name="appinsights-monitoring"></a>

You can manage and update monitoring for your Application Insights application using the command line\.

**Topics**
+ [List problems with your application](#appinsights-list-problems-monitoring)
+ [Describe an application problem](#appinsights-describe-app-problem)
+ [Describe the anomalies or errors associated with a problem](#appinsights-describe-anomalies)
+ [Describe an anomaly or error with the application](#appinsights-describe-anomalies)
+ [Describe the monitoring configurations of a component](#appinsights-describe-monitoring)
+ [Describe the recommended monitoring configuration of a component](#appinsights-describe-rec-monitoring)
+ [Update the monitoring configurations for a component](#update-monitoring)
+ [Remove a specified resource group from Application Insights monitoring](#update-monitoring)

#### List problems with your application<a name="appinsights-list-problems-monitoring"></a>

**List problems with your application using the AWS CLI**  
To use the AWS CLI to list problems with your application detected between 1,000 and 10,000 milliseconds since Unix Epoch for an application created on a resource group called `my-resource-group`, use the following command\.

```
 aws application-insights list-problems --resource-group-name my-resource-group --start-time 1000 --end-time 10000
```

**List problems with your application using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to list problems with your application detected between 1,000 and 10,000 milliseconds since Unix Epoch for an application created on a resource group called `my-resource-group`, use the following command\.

```
$startDate = "8/6/2019 3:33:00"
$endDate = "8/6/2019 3:34:00"
Get-CWAIProblemList -ResourceGroupName my-resource-group -StartTime $startDate -EndTime $endDate
```

#### Describe an application problem<a name="appinsights-describe-app-problem"></a>

**Describe an application problem using the AWS CLI**  
To use the AWS CLI to describe a problem with problem id `p-1234567890`, use the following command\.

```
aws application-insights describe-problem —problem-id p-1234567890
```

**Describe an application problem using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to describe a problem with problem id `p-1234567890`, use the following command\.

```
Get-CWAIProblem -ProblemId p-1234567890
```

#### Describe the anomalies or errors associated with a problem<a name="appinsights-describe-anomalies"></a>

**Describe the anomalies or errors associated with a problem using the AWS CLI**  
To use the AWS CLI to describe the anomalies or errors associated with a problem with problem id `p-1234567890`, use the following command\.

```
aws application-insights describe-problem-observations --problem-id -1234567890
```

**Describe the anomalies or errors associated with a problem using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to describe the anomalies or errors associated with a problem with problem id `p-1234567890`, use the following command\.

```
Get-CWAIProblemObservation -ProblemId p-1234567890
```

#### Describe an anomaly or error with the application<a name="appinsights-describe-anomalies"></a>

**Describe an anomaly or error with the application using the AWS CLI**  
To use the AWS CLI to describe an anomaly or error with the application with the observation id `o-1234567890`, use the following command\.

```
aws application-insights describe-observation —observation-id o-1234567890
```

**Describe an anomaly or error with the application using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to describe an anomaly or error with the application with the observation id `o-1234567890`, use the following command\.

```
Get-CWAIObservation -ObservationId o-1234567890
```

#### Describe the monitoring configurations of a component<a name="appinsights-describe-monitoring"></a>

**Describe the monitoring configurations of a component using the AWS CLI**  
To use the AWS CLI to describe the monitoring configuration of a component called `my-component` in an application created on the resource group `my-resource-group`, use the following command\.

```
aws application-insights describe-component-configuration —resource-group-name my-resource-group —component-name my-component
```

**Describe the monitoring configurations of a component using AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to describe the monitoring configuration of a component called `my-component`, in an application created on the resource group `my-resource-group`, use the following command\.

```
Get-CWAIComponentConfiguration -ComponentName my-component -ResourceGroupName my-resource-group
```

For more information about component configuration and for example JSON files, see [Work with component configurations](component-config.md)\.

#### Describe the recommended monitoring configuration of a component<a name="appinsights-describe-rec-monitoring"></a>

**Describe the recommended monitoring configuration of a component using the AWS CLI**  
When the component is part of a \.NET Worker application, you can use the AWS CLI to describe the recommended monitoring configuration of a component called `my-component` in an application created on the resource group `my-resource-group`, by using the following command\.

```
aws application-insights describe-component-configuration-recommendation --resource-group-name my-resource-group --component-name my-component --tier DOT_NET_WORKER
```

**Describe the recommended monitoring configuration of a component using AWS Tools for Windows PowerShell**  
When the component is part of a \.NET Worker application, you can use the AWS Tools for Windows PowerShell to describe the recommended monitoring configuration of a component called `my-component` in an application created on the resource group `my-resource-group`, by using the following command\.

```
Get-CWAIComponentConfigurationRecommendation -ComponentName my-component -ResourceGroupName my-resource-group -Tier DOT_NET_WORKER
```

For more information about component configuration and for example JSON files, see [Work with component configurations](component-config.md)\.

#### Update the monitoring configurations for a component<a name="update-monitoring"></a>

**Update the monitoring configurations for a component using the AWS CLI**  
To use the AWS CLI to update the component called `my-component` in an application created on the resource group called `my-resource-group`, use the following command\. The command includes these actions:

1. Enable monitoring for the component\.

1. Set the tier of the component to `.NET Worker`\.

1. Update the JSON configuration of the component to read from the local file `configuration.txt`\.

```
aws application-insights update-component-configuration --resource-group-name my-resource-group --component-name my-component --tier DOT_NET_WORKER --monitor --component-configuration "file://configuration.txt"
```

**Update the monitoring configurations for a component using the AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to update the component called `my-component` in an application created on the resource group called `my-resource-group`, use the following command\. The command includes these actions:

1. Enable monitoring for the component\.

1. Set the tier of the component to `.NET Worker`\.

1. Update the JSON configuration of the component to read from the local file `configuration.txt`\.

```
[string]$config = Get-Content -Path configuration.txt
Update-CWAIComponentConfiguration -ComponentName my-component -ResourceGroupName my-resource-group -Tier DOT_NET_WORKER -Monitor 1 -ComponentConfiguration $config
```

For more information about component configuration and for example JSON files, see [Work with component configurations](component-config.md)\.

#### Remove a specified resource group from Application Insights monitoring<a name="update-monitoring"></a>

**Remove a specified resource group from Application Insights monitoring using the AWS CLI**  
To use the AWS CLI to remove an application created on the resource group called `my-resource-group` from monitoring, use the following command\.

```
aws application-insights delete-application --resource-group-name my-resource-group
```

**Remove a specified resource group from Application Insights monitoring using the AWS Tools for Windows PowerShell**  
To use the AWS Tools for Windows PowerShell to remove an application created on the resource group called `my-resource-group` from monitoring, use the following command\.

```
Remove-CWAIApplication -ResourceGroupName my-resource-group
```

### Configure monitoring for SQL Always On Availability Groups<a name="configure-sql"></a>

1. Create an application for the resource group with the SQL HA EC2 instances\.

   ```
   aws application-insights create-application ‐-region <REGION> ‐-resource-group-name  <RESOURCE_GROUP_NAME>
   ```

1. Define the EC2 instances that represent the SQL HA cluster by creating a new application component\.

   ```
   aws application-insights create-component ‐-resource-group-name  "<RESOURCE_GROUP_NAME>" ‐-component-name SQL_HA_CLUSTER ‐-resource-list  "arn:aws:ec2:<REGION>:<ACCOUNT_ID>:instance/<CLUSTER_INSTANCE_1_ID>" "arn:aws:ec2:<REGION>:<ACCOUNT_ID>:instance/<CLUSTER_INSTANCE_2_ID>
   ```

1. Configure the SQL HA component\.

   ```
   aws application-insights  update-component-configuration ‐-resource-group-name "<RESOURCE_GROUP_NAME>" ‐-region <REGION> ‐-component-name "SQL_HA_CLUSTER" ‐-monitor ‐-tier SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP ‐-monitor  ‐-component-configuration '{
     "instances" : {
       "alarmMetrics" : [ {
         "alarmMetricName" : "CPUUtilization",
         "monitor" : true
       }, {
         "alarmMetricName" : "StatusCheckFailed",
         "monitor" : true
       }, {
         "alarmMetricName" : "Processor % Processor Time",
         "monitor" : true
       }, {
         "alarmMetricName" : "Memory % Committed Bytes In Use",
         "monitor" : true
       }, {
         "alarmMetricName" : "Memory Available Mbytes",
         "monitor" : true
       }, {
         "alarmMetricName" : "Paging File % Usage",
         "monitor" : true
       }, {
         "alarmMetricName" : "System Processor Queue Length",
         "monitor" : true
       }, {
         "alarmMetricName" : "Network Interface Bytes Total/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "PhysicalDisk % Disk Time",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Buffer Manager Buffer cache hit ratio",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Buffer Manager Page life expectancy",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:General Statistics Processes blocked",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:General Statistics User Connections",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Locks Number of Deadlocks/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:SQL Statistics Batch Requests/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica File Bytes Received/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Log Bytes Received/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Log remaining for undo",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Log Send Queue",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Mirrored Write Transaction/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Recovery Queue",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Redo Bytes Remaining",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Redone Bytes/sec",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Total Log requiring undo",
         "monitor" : true
       }, {
         "alarmMetricName" : "SQLServer:Database Replica Transaction Delay",
         "monitor" : true
       } ],
       "windowsEvents" : [ {
         "logGroupName" : "WINDOWS_EVENTS-Application-<RESOURCE_GROUP_NAME>",
         "eventName" : "Application",
         "eventLevels" : [ "WARNING", "ERROR", "CRITICAL", "INFORMATION" ],
         "monitor" : true
       }, {
         "logGroupName" : "WINDOWS_EVENTS-System-<RESOURCE_GROUP_NAME>",
         "eventName" : "System",
         "eventLevels" : [ "WARNING", "ERROR", "CRITICAL" ],
         "monitor" : true
       }, {
         "logGroupName" : "WINDOWS_EVENTS-Security-<RESOURCE_GROUP_NAME>",
         "eventName" : "Security",
         "eventLevels" : [ "WARNING", "ERROR", "CRITICAL" ],
         "monitor" : true
       } ],
       "logs" : [ {
         "logGroupName" : "SQL_SERVER_ALWAYSON_AVAILABILITY_GROUP-<RESOURCE_GROUP_NAME>",
         "logPath" : "C:\\Program Files\\Microsoft SQL Server\\MSSQL**.MSSQLSERVER\\MSSQL\\Log\\ERRORLOG",
         "logType" : "SQL_SERVER",
         "monitor" : true,
         "encoding" : "utf-8"
       } ]
     }
   }'
   ```

**Note**  
Application Insights must ingest Application Event logs \(information level\) to detect cluster activities such as failover\.

### Configure monitoring for MySQL RDS<a name="configure-mysql-rds"></a>

1. Create an application for the resource group with the RDS MySQL database instance\.

   ```
   aws application-insights create-application ‐-region <REGION> ‐-resource-group-name  <RESOURCE_GROUP_NAME>
   ```

1. The error log is enabled by default\. The slow query log can be enabled using data parameter groups\. For more information, see [Accessing the MySQL Slow Query and General Logs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html#USER_LogAccess.MySQL.Generallog)\.
   + `set slow_query_log = 1`
   + `set log_output = FILE`

1. Export the logs to be monitored to CloudWatch logs\. For more information, see [Publishing MySQL Logs to CloudWatch Logs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html#USER_LogAccess.MySQLDB.PublishtoCloudWatchLogs)\.

1. Configure the MySQL RDS component\.

   ```
   aws application-insights  update-component-configuration ‐-resource-group-name "<RESOURCE_GROUP_NAME>" ‐-region <REGION> ‐-component-name "<DB_COMPONENT_NAME>" ‐-monitor ‐-tier DEFAULT ‐-monitor  ‐-component-configuration "{\"alarmMetrics\":[{\"alarmMetricName\":\"CPUUtilization\",\"monitor\":true}],\"logs\":[{\"logType\":\"MYSQL\",\"monitor\":true},{\"logType\": \"MYSQL_SLOW_QUERY\",\"monitor\":false}]}"
   ```

### Configure monitoring for MySQL EC2<a name="configure-mysql-ec2"></a>

1. Create an application for the resource group with the SQL HA EC2 instances\.

   ```
   aws application-insights create-application ‐-region <REGION> ‐-resource-group-name  <RESOURCE_GROUP_NAME>
   ```

1. The error log is enabled by default\. The slow query log can be enabled using data parameter groups\. For more information, see [Accessing the MySQL Slow Query and General Logs](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html#USER_LogAccess.MySQL.Generallog)\.
   + `set slow_query_log = 1`
   + `set log_output = FILE`

1. Configure the MySQL EC2 component\.

   ```
   aws application-insights  update-component-configuration ‐-resource-group-name "<RESOURCE_GROUP_NAME>" ‐-region <REGION> ‐-component-name "<DB_COMPONENT_NAME>" ‐-monitor ‐-tier MYSQL ‐-monitor  ‐-component-configuration "{\"alarmMetrics\":[{\"alarmMetricName\":\"CPUUtilization\",\"monitor\":true}],\"logs\":[{\"logGroupName\":\"<UNIQUE_LOG_GROUP_NAME>\",\"logPath\":\"C:\\\\ProgramData\\\\MySQL\\\\MySQL Server **\\\\Data\\\\<FILE_NAME>.err\",\"logType\":\"MYSQL\",\"monitor\":true,\"encoding\":\"utf-8\"}]}"
   ```

### Configure monitoring for PostgreSQL RDS<a name="configure-postgresql-rds"></a>

1. Create an application for the resource group with the PostgreSQL RDS database instance\.

   ```
   aws application-insights create-application ‐-region <REGION> ‐-resource-group-name  <RESOURCE_GROUP_NAME>
   ```

1. Publishing PostgreSQL logs to CloudWatch is not enabled by default\. To enable monitoring, open the RDS console and select the database to monitor\. Choose **Modify** in the upper right corner, and select the checkbox labeled **PostgreSQL** log\. Choose **Continue** to save this setting\.

1. Your PostgreSQL logs are exported to CloudWatch\.

1. Configure the PostgreSQL RDS component\.

   ```
   aws application-insights update-component-configuration --region <REGION> --resource-group-name <RESOURCE_GROUP_NAME> --component-name <DB_COMPONENT_NAME> --monitor --tier DEFAULT --component-configuration 
   "{
      \"alarmMetrics\":[
         {
            \"alarmMetricName\": \"CPUUtilization\",
            \"monitor\": true
         }
      ],
      \"logs\":[
         {
            \"logType\": \"POSTGRESQL\",
            \"monitor\": true
         }
      ]
   }"
   ```

### Configure monitoring for PostgreSQL EC2<a name="configure-postgresql-ec2"></a>

1. Create an application for the resource group with the PostgreSQL EC2 instance\.

   ```
   aws application-insights create-application ‐-region <REGION> ‐-resource-group-name  <RESOURCE_GROUP_NAME>
   ```

1. Configure the PostgreSQL EC2 component\.

   ```
   aws application-insights update-component-configuration ‐-region <REGION> ‐-resource-group-name <RESOURCE_GROUP_NAME> ‐-component-name <DB_COMPONENT_NAME> ‐-monitor ‐-tier POSTGRESQL ‐-component-configuration 
   "{
      \"alarmMetrics\":[
         {
            \"alarmMetricName\":\"CPUUtilization\",
            \"monitor\":true
         }
      ],
      \"logs\":[
         {
            \"logGroupName\":\"<UNIQUE_LOG_GROUP_NAME>\",
            \"logPath\":\"/var/lib/pgsql/data/log/\",
            \"logType\":\"POSTGRESQL\",
            \"monitor\":true,
            \"encoding\":\"utf-8\"
         }
      ]
   }"
   ```

## Set up notifications for detected problems<a name="appinsights-cloudwatch-events"></a>

For each application that is added to CloudWatch Application Insights, a CloudWatch Event is published for the following events:
+ **Problem Creation\.** Emitted when CloudWatch Application Insights detects a new problem\.
  + Detail Type: ** "Application Insights Problem Detected"**
  + Detail:
    + `problemId`: The detected problem ID\.
    + `region`: The AWS Region where the problem was created\.
    + `resourceGroupName`: The Resource Group for the registered application for which the problem was detected\.
    + `status`: The status of the problem\.
    + `severity`: The severity of the problem\.
    + `problemUrl`: The console URL for the problem\.
+ **Problem Update\.** Emitted when the problem is updated with a new observation or when an existing observation is updated and the problem is subsequently updated; updates include a resolution or closure of the problem\.
  + Detail Type: ** "Application Insights Problem Updated"**
  + Detail:
    + `problemId`: The created problem ID\.
    + `region`: The AWS Region where the problem was created\.
    + `resourceGroupName`: The Resource Group for the registered application for which the problem was detected\.
    + `status`: The status of the problem\.
    + `severity`: The severity of the problem\.
    + `problemUrl`: The console URL for the problem\.

**How to receive notification for problem events generated by an application**

From the CloudWatch console, select **Rules** under **Events** in the left navigation pane\. From the **Rules **page, select **Create rule**\. Choose **Amazon CloudWatch Application Insights** from the **Service Name** dropdown list and choose the **Event Type**\. Then, choose **Add target** and select the target and parameters, for example, an **SNS topic** or **Lambda function**\. 

**Actions through AWS Systems Manager\.** CloudWatch Application Insights provides built\-in integration with Systems Manager OpsCenter\. If you choose to use this integration for your application, an OpsItem is created on the OpsCenter console for every problem detected with the application\. From the OpsCenter console, you can view summarized information about the problem detected by CloudWatch Application Insights and pick a Systems Manager Automation runbook to take remedial actions or further identify Windows processes that are causing resource issues in your application\. 