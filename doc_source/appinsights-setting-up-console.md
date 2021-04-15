# Set up, configure, and manage your application for monitoring from the CloudWatch console<a name="appinsights-setting-up-console"></a>

This section provides steps for setting up, configuring, and managing your application for monitoring from the CloudWatch console\.

**Topics**
+ [Add and configure an application](#appinsights-add-configure)
+ [Disable an application](#appinsights-disable-app)
+ [Disable monitoring for an application component](#appinsights-disable-monitoring)
+ [Delete an application](#appinsights-delete-app)

## Add and configure an application<a name="appinsights-add-configure"></a>

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

   When you create a new application with CloudWatch Application Insights, the service‐linked role is created for you\. To delete the service‐linked role, you must first delete all of your applications on CloudWatch Application Insights and then manually delete the role\. For more information, see [Using service\-linked roles for CloudWatch Application Insights](CHAP_using-service-linked-roles-appinsights.md)\.

   CloudWatch Application Insights is now set to monitor metrics and logs for your application\. It may take up to two weeks for the system to generate meaningful insights\.

   \*If your Resource Group is already configured and you want to save your configuration but don’t want CloudWatch Application Insights to monitor your application, you can disable CloudWatch Application Insights\. You can also delete your configuration\.

1. **Add AWS SSM OpsCenter Integration\.** To view and get notified when problems are detected for selected applications, select the **Integrate with AWS OpsCenter** check box on the **Monitoring Details** page\. To track the operations that are taken to resolve operational work items \(OpsItems\) that are related to your AWS resources, provide the SNS topic ARN\. 

1. **View monitoring \(optional\)\.** After your application has been set up for monitoring, you can view and troubleshoot detected problems and insights in the default overview page of the CloudWatch console\. You can view detected problems, alarms, and dashboards by selecting **View Insights** from the Application Insights landing page, or on the CloudWatch landing page\. 

## Disable an application<a name="appinsights-disable-app"></a>

To disable an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to disable\. Under **Actions**, choose **Disable**\. When you disable an application, monitoring is disabled, but Application Insights stores the saved monitors for application components\. 

## Disable monitoring for an application component<a name="appinsights-disable-monitoring"></a>

To disable monitoring for an application component, from the Application Details page select the component for which you want to disable monitoring\. Choose **Manage Monitors**, and then clear the **Enable Monitoring** check box\. 

## Delete an application<a name="appinsights-delete-app"></a>

To delete an application, from the CloudWatch dashboard, under **Settings**, select the application that you want to delete\. Under **Actions**, choose **Delete**\. This deletes monitoring and deletes all of the saved monitors for application components\. The application resources are not deleted\. 