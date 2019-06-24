# Setting Up Your Application<a name="appinsights-setting-up"></a>

To get started with CloudWatch Application Insights for \.NET and SQL Server, start on the [CloudWatch console landing page](http://console.aws.amazon.com/cloudwatch)\. From the left navigation pane, choose **Settings**\. From the **Settings** page, choose **Application Insights for \.NET and SQL Server** > **View applications to get started**\. Follow these steps to configure your \.NET and SQL Server application\.

1. **Add an Application\.** To set up monitoring for your \.NET and SQL Server application, on the Application Insights for \.NET and SQL Server page, select **Add an application**\. This page shows the list of applications that are monitored with Application Insights for \.NET and SQL Server, along with their monitoring status\. After you select **Add an application**, you will be taken to the **Add an application** page\.

1. **Select Resource Group\. **On the **Add an application** page, to add an application to CloudWatch Application Insights for \.NET and SQL Server, choose an AWS Resource Group from the dropdown list that contains your application resources, including front\-end servers, load balancers, auto scaling groups, and database servers\. 

   CloudWatch Application Insights for \.NET and SQL Server supports both tag\-based and CloudFormation\-based Resource Groups \(with the exception of Auto Scaling groups\)\. For more information, see [Working with Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html)\.

   If you have not created a Resource Group for your \.NET application, you can create one\. For more information, see the [https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html)\. 

1. **Add Monitoring Details\.** After you add an application, you are taken to the **Monitoring Details** page, which lists the application components, resources in those components, and their monitoring status\. Components are auto\-grouped, standalone, or custom groupings of similar resources that make up an application\. By default, CloudWatch Application Insights for \.NET and SQL Server groups instances that are in Auto Scaling groups, and instances that are behind your Elastic Load Balancers\. On this page, you can configure custom components and manage monitoring for each application component\. For supported components, see [Supported Application Components](appinsights-what-is.md#appinsights-components)\.

1. **Configure components\.** After selecting a Resource Group, you are prompted to configure [components](appinsights-what-is.md#components)\. We recommend grouping similar resources, such as \.NET web server instances, into custom components for easier onboarding and better monitoring and insights\. By default, CloudWatch Application Insights for \.NET and SQL Server groups instances that are in Auto Scaling groups, and instances that are behind your Elastic Load Balancers\. For supported components, see [Supported Application Components](appinsights-what-is.md#appinsights-components)\.

   Under **Application components**, for each component for which you want to set up monitors, select the component and select **Manage Monitoring**\.

1. **Enable Monitors\.** To set up monitoring for an application component, select the components that you want to monitor and then choose **Manage Monitoring**\. Select the **Enable Monitoring** check box\. When you select the check box, the dropdown populates with the relevant application tiers\. Choose the application tier for the selected component\. The tiers indicate the part of the application stack running on the selected resources\.

   Based on your tier selection, CloudWatch Application Insights for \.NET and SQL Server makes recommendations for logs to monitor for the selected component\. This recommendation can be customized according to your needs\.

   For each log, specify the file path for the log on your EC2 instance\. The corresponding log group name has a default value but can be customized\. For SQL Server error log and IIS log, a default file path location is assigned\. If you do not want to store your logs in the default location, you should update this default file path\. For log path and log name guidance, see [Paths](https://docs.microsoft.com/en-us/windows/desktop/FileIO/naming-a-file#paths)\.

   CloudWatch Application Insights for \.NET and SQL Server also sets up relevant metrics for your application resources\. They are monitored for approximately two weeks to identify the appropriate metrics thresholds\. If you have created the metrics in the past, CloudWatch Application Insights for \.NET and SQL Server pulls historical data for the last two weeks to identify the thresholds and to set the alarms accordingly\. For newly created metrics, it may take up to three days before alarms are created\. 

1. **Save monitors\.** When you are finished selecting and customizing logs and metrics, select **Save** to set up monitors for the selected component\. When you select **Save**, Application Insights sets up the CloudWatch Agent configuration files for all of the instances in your application based on the recommended metrics and your selection of logs\. It can take up to an hour for this process to complete\. 

   CloudWatch Application Insights for \.NET and SQL Server also sets up CloudWatch alarms for selected metrics in the component\. The alarms are dynamically updated by monitoring historical metric patterns from the past two weeks\. 

    When you select **Cancel**, Application Insights only deletes your current selections\. 
**Note**  
When you create a new application with CloudWatch Application Insights for \.NET and SQL Server, the service‐linked role is created for you\. To delete the service‐linked role, you must first delete all of your applications on CloudWatch Applications Insights for \.NET and SQL Server and then manually delete the role\. For more information, see [Using Service\-Linked Roles for CloudWatch Application Insights for \.NET and SQL Server](CHAP_using-service-linked-roles-appinsights.md)\.

   CloudWatch Application Insights for \.NET and SQL Server is now set to monitor metrics and logs for your application\. It may take up to two weeks for the system to generate meaningful insights\.

1. **View monitoring \(optional\)\.** After your application has been set up for monitoring, you can view and troubleshoot detected problems and insights in the default overview page of the CloudWatch console\. You can view detected problems, alarms, and dashboards by selecting **View Insights** from the Application Insights landing page, or on the CloudWatch landing page\. 

**Disabling an Application**  
To disable an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to disable\. Under **Actions**, choose **Disable**\. When you disable an application, monitoring is disabled, but Application Insights stores the saved monitors for application components\. 

**Disabling Monitoring for an Application Component**  
To disable monitoring for an application component, from the Application Details page select the component for which you want to disable monitoring\. Choose **Manage Monitors**, and then uncheck the **Enable Monitoring** check box\. 

**Deleting an Application**  
To delete an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to delete\. Under **Actions**, choose **Delete**\. This deletes monitoring and deletes all of the saved monitors for application components\. The application resources are not deleted\. 

If your Resource Group is already configured and you want to save your configuration but don’t want CloudWatch Application Insights for \.NET and SQL Server to monitor your application, you can disable CloudWatch Application Insights for \.NET and SQL Server\. You can also delete your configuration\.