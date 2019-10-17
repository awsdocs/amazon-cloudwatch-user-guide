# Setting Up Your Application<a name="appinsights-setting-up"></a>

This section provides steps for setting up, configuring, and managing your Application Insights for \.NET and SQL Server application using the console, the AWS CLI, and AWS Tools for Windows PowerShell\.

**Topics**
+ [Add and Configure an Application with the CloudWatch Console](#appinsights-add-configure)
+ [Disable an Application with the Console](#appinsights-disable-app)
+ [Disable Monitoring for an Application Component with the Console](#appinsights-disable-monitoring)
+ [Delete an Application with the Console](#appinsights-delete-app)
+ [Add and Manage an Application with Application Insights Using the Command Line](#appinsights-config-app-command)
+ [Manage and Update Monitoring Using the Command Line](#appinsights-monitoring)

## Add and Configure an Application with the CloudWatch Console<a name="appinsights-add-configure"></a>

**Add and configure an application from the CloudWatch console**  
To get started with CloudWatch Application Insights for \.NET and SQL Server from the CloudWatch console, follow these steps\.

1. **Start\.** Open the [CloudWatch console landing page](http://console.aws.amazon.com/cloudwatch)\. From the left navigation pane, choose **Settings**\. From the **Settings** page, choose **Application Insights for \.NET and SQL Server** > **View applications to get started**\. 

1. **Add an Application\.** To set up monitoring for your \.NET and SQL Server application, on the Application Insights for \.NET and SQL Server page, select **Add an application**\. This page shows the list of applications that are monitored with Application Insights for \.NET and SQL Server, along with their monitoring status\. After you select **Add an application**, you will be taken to the **Add an application** page\.

1. **Select Resource Group\. **On the **Add an application** page, to add an application to CloudWatch Application Insights for \.NET and SQL Server, choose an AWS Resource Group from the dropdown list that contains your application resources\. These resources include front\-end servers, load balancers, auto scaling groups, and database servers\. 

   CloudWatch Application Insights for \.NET and SQL Server supports both tag\-based and CloudFormation\-based Resource Groups \(with the exception of Auto Scaling groups\)\. For more information, see [Working with Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html)\.

   If you have not created a Resource Group for your \.NET application, you can create one\. For more information, see the [https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)\. 

1. **Add Monitoring Details\.** After you add an application, you are taken to the **Monitoring Details** page, which lists the application components, resources in those components, and their monitoring status\. Components are auto\-grouped, standalone, or custom groupings of similar resources that make up an application\. By default, CloudWatch Application Insights for \.NET and SQL Server groups instances that are in Auto Scaling groups, and instances that are behind your Elastic Load Balancers\. On this page, you can configure custom components and can manage monitoring for each application component\. For supported components, see [Supported Application Components](appinsights-what-is.md#appinsights-components)\.

1. **Configure components\.** After selecting a Resource Group, you are prompted to configure [components](appinsights-what-is.md#components)\. We recommend grouping similar resources, such as \.NET web server instances, into custom components for easier onboarding and better monitoring and insights\. By default, CloudWatch Application Insights for \.NET and SQL Server groups instances that are in Auto Scaling groups, and instances that are behind your Elastic Load Balancers\. For supported components, see [Supported Application Components](appinsights-what-is.md#appinsights-components)\.

   Under **Application components**, for each component for which you want to set up monitors, select the component and select **Manage Monitoring**\.

1. **Enable Monitors\.** To set up monitoring for an application component, select the components that you want to monitor and then choose **Manage Monitoring**\. Select the **Enable Monitoring** check box\. When you select the check box, the dropdown populates with the relevant application tiers\. Choose the application tier for the selected component\. The tiers indicate the part of the application stack running on the selected resources\.

   Based on your tier selection, CloudWatch Application Insights for \.NET and SQL Server makes recommendations for logs to monitor for the selected component\. This recommendation can be customized according to your needs\.

   For application\-specific logs, including for Microsoft SQL Server Error logs and IIS logs, verify the default log path \(if any\) or enter the correct log location in your EC2 instance\. 

   You can also choose to add Windows Event Logs, including Windows Logs and Applications and Services Logs\. To do this, enter the types of events you want to store and analyze\. Then, specify all of the event levels \(critical, error, warning, informational, or verbose\) that you want to store in your CloudWatch account\. 

   You can add a log group for storing and grouping each of these logs on your CloudWatch account, which also facilitates searches\. 

   CloudWatch Application Insights for \.NET and SQL Server also sets up relevant metrics for your application resources\. They are monitored for approximately two weeks to identify the appropriate metrics thresholds\. If you have created the metrics in the past, CloudWatch Application Insights for \.NET and SQL Server pulls historical data for the last two weeks to identify the thresholds and to set the alarms accordingly\. For newly created metrics, it may take up to three days before alarms are created\. 

1. **Save monitors\.** When you are finished selecting and customizing logs and metrics, select **Save** to set up monitors for the selected component\. When you select **Save**, Application Insights sets up the CloudWatch Agent configuration files for all of the instances in your application based on the recommended metrics and your selection of logs\. It can take up to an hour for this process to complete\. 

   CloudWatch Application Insights for \.NET and SQL Server also sets up CloudWatch alarms for selected metrics in the component\. The alarms are dynamically updated by monitoring historical metric patterns from the past two weeks\. 

    When you select **Cancel**, Application Insights only deletes your current selections\. 

   When you create a new application with CloudWatch Application Insights for \.NET and SQL Server, the service‐linked role is created for you\. To delete the service‐linked role, you must first delete all of your applications on CloudWatch Applications Insights for \.NET and SQL Server and then manually delete the role\. For more information, see [Using Service\-Linked Roles for CloudWatch Application Insights for \.NET and SQL Server](CHAP_using-service-linked-roles-appinsights.md)\.

   CloudWatch Application Insights for \.NET and SQL Server is now set to monitor metrics and logs for your application\. It may take up to two weeks for the system to generate meaningful insights\.

   \*If your Resource Group is already configured and you want to save your configuration but don’t want CloudWatch Application Insights for \.NET and SQL Server to monitor your application, you can disable CloudWatch Application Insights for \.NET and SQL Server\. You can also delete your configuration\.

1. **Add AWS Systems Manager OpsCenter Integration\.** To view and get notified when problems are detected for selected applications, select the **Integrate with AWS OpsCenter** check box on the **Monitoring Details** page\. To track the operations that are taken to resolve operational work items \(OpsItems\) that are related to your AWS resources, provide the SNS topic ARN\. 

1. **View monitoring \(optional\)\.** After your application has been set up for monitoring, you can view and troubleshoot detected problems and insights in the default overview page of the CloudWatch console\. You can view detected problems, alarms, and dashboards by selecting **View Insights** from the Application Insights landing page, or on the CloudWatch landing page\. 

## Disable an Application with the Console<a name="appinsights-disable-app"></a>

To disable an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to disable\. Under **Actions**, choose **Disable**\. When you disable an application, monitoring is disabled, but Application Insights stores the saved monitors for application components\. 

## Disable Monitoring for an Application Component with the Console<a name="appinsights-disable-monitoring"></a>

To disable monitoring for an application component, from the Application Details page select the component for which you want to disable monitoring\. Choose **Manage Monitors**, and then uncheck the **Enable Monitoring** check box\. 

## Delete an Application with the Console<a name="appinsights-delete-app"></a>

To delete an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to delete\. Under **Actions**, choose **Delete**\. This deletes monitoring and deletes all of the saved monitors for application components\. The application resources are not deleted\. 

## Add and Manage an Application with Application Insights Using the Command Line<a name="appinsights-config-app-command"></a>

You can add, get information about, manage, and configure your Application Insights application using the command line\. 

**Topics**
+ [Add an Application](#appinsights-add-app)
+ [Describe an Application](#appinsights-describe-app)
+ [List Components in an Application](#appinsights-list-components)
+ [Describe a Component](#appinsights-describe-components)
+ [Group Similar Resources into a Custom Component](#appinsights-group-resources-components)
+ [Ungroup a Custom Component](#appinsights-ungroup-resources-components)
+ [Update an Application](#appinsights-update-app)
+ [Update a Custom Component](#appinsights-update-component)

### Add an Application<a name="appinsights-add-app"></a>

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

### Describe an Application<a name="appinsights-describe-app"></a>

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

### List Components in an Application<a name="appinsights-list-components"></a>

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

### Describe a Component<a name="appinsights-describe-components"></a>

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

### Group Similar Resources into a Custom Component<a name="appinsights-group-resources-components"></a>

We recommend grouping similar resources, such as \.NET web server instances, into custom components for easier onboarding and better monitoring and insights\. Currently, Application Insights for \.NET and SQL Server supports custom groups for EC2 instances\.

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

### Ungroup a Custom Component<a name="appinsights-ungroup-resources-components"></a>

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

### Update an Application<a name="appinsights-update-app"></a>

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

### Update a Custom Component<a name="appinsights-update-component"></a>

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

## Manage and Update Monitoring Using the Command Line<a name="appinsights-monitoring"></a>

You can manage and update monitoring for your Application Insights application using the command line\.

**Topics**
+ [List Problems with Your Application](#appinsights-list-problems-monitoring)
+ [Describe an Application Problem](#appinsights-describe-app-problem)
+ [Describe the Anomalies or Errors Associated with a Problem](#appinsights-describe-anomalies)
+ [Describe an Anomaly or Error with the Application](#appinsights-describe-anomalies)
+ [Describe the Monitoring Configurations of a Component](#appinsights-describe-monitoring)
+ [Describe the Recommended Monitoring Configuration of a Component](#appinsights-describe-rec-monitoring)
+ [Update the Monitoring Configurations for a Component](#update-monitoring)
+ [Remove a Specified Resource Group from Application Insights Monitoring](#update-monitoring)

### List Problems with Your Application<a name="appinsights-list-problems-monitoring"></a>

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

### Describe an Application Problem<a name="appinsights-describe-app-problem"></a>

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

### Describe the Anomalies or Errors Associated with a Problem<a name="appinsights-describe-anomalies"></a>

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

### Describe an Anomaly or Error with the Application<a name="appinsights-describe-anomalies"></a>

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

### Describe the Monitoring Configurations of a Component<a name="appinsights-describe-monitoring"></a>

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

For more information about component configuration and for example JSON files, see [Component Configuration](component-config.md)\.

### Describe the Recommended Monitoring Configuration of a Component<a name="appinsights-describe-rec-monitoring"></a>

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

For more information about component configuration and for example JSON files, see [Component Configuration](component-config.md)\.

### Update the Monitoring Configurations for a Component<a name="update-monitoring"></a>

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

For more information about component configuration and for example JSON files, see [Component Configuration](component-config.md)\.

### Remove a Specified Resource Group from Application Insights Monitoring<a name="update-monitoring"></a>

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