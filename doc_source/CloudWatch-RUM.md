# Use CloudWatch RUM<a name="CloudWatch-RUM"></a>

With CloudWatch RUM, you can perform real user monitoring to collect and view client\-side data about your web application performance from actual user sessions in near real time\. The data that you can visualize and analyze includes page load times, client\-side errors, and user behavior\. When you view this data, you can see it all aggregated together and also see breakdowns by the browsers and devices that your customers use\.

You can use the collected data to quickly identify and debug client\-side performance issues\. CloudWatch RUM helps you visualize anomalies in your application performance and find relevant debugging data such as error messages, stack traces, and user sessions\. You can also use RUM to understand the range of end user impact including the number of users, geolocations, and browsers used\.

End user data that you collect for CloudWatch RUM is retained for 30 days and then automatically deleted\. If you want to keep the RUM events for a longer time, you can choose to have the app monitor send copies of the events to CloudWatch Logs in your account\. Then, you can adjust the retention period for that log group\.

To use RUM, you create an *app monitor* and provide some information\. RUM generates a JavaScript snippet for you to paste into your application\. The snippet pulls in the RUM web client code\. The RUM web client captures data from a percentage of your application's user sessions, which is displayed in a pre\-built dashboard\. You can specify what percentage of user sessions to gather data from\.

The RUM web client is open source\. For more information, see [CloudWatch RUM web client](https://github.com/aws-observability/aws-rum-web)\.

**Performance considerations**

This section discusses the performance considerations of using CloudWatch RUM\.
+ **Load performance impact**— The CloudWatch RUM web client can be installed in your web application as a JavaScript module, or loaded into your web application asynchronously from a content delivery network \(CDN\)\. It does not block the application’s load process\. CloudWatch RUM is designed for there to be no perceptible impact to the application’s load time\.
+ **Runtime impact**— The RUM web client performs processing to record and dispatch RUM data to the CloudWatch RUM service\. Because events are infrequent and the amount of processing is small, CloudWatch RUM is designed for there to be no detectable impact to the application’s performance\.
+ **Network impact**— The RUM web client periodically sends data to the CloudWatch RUM service\. Data is dispatched at regular intervals while the application is running, and also immediately before the browser unloads the application\. Data sent immediately before the browser unloads the application are sent as beacons, which, are designed to have no detectable impact on the application’s unload time\.

**RUM Pricing**

With CloudWatch RUM, you incur charges for every RUM event that CloudWatch RUM receives\. Each data item collected using the RUM web client is considered a RUM event\. Examples of RUM events include a page view, a JavaScript error, and an HTTP error\. You have options for which types of events are collected by each app monitor\. You can activate or deactivate options to collect performance telemetry events, JavaScript errors, HTTP errors, and X\-Ray traces\. For more information about choosing these options, see [Step 2: Create an app monitor](CloudWatch-RUM-get-started-create-app-monitor.md) and [Information collected by the CloudWatch RUM web client](CloudWatch-RUM-datacollected.md)\. For more information about pricing, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\. 

**Region availability**

CloudWatch RUM is currently available in the following Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(Oregon\)
+ Europe \(Frankfurt\)
+ Europe \(Stockholm\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Asia Pacific \(Tokyo\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)

**Topics**
+ [IAM policies to use CloudWatch RUM](CloudWatch-RUM-permissions.md)
+ [Set up an application to use CloudWatch RUM](CloudWatch-RUM-get-started.md)
+ [Configuring the CloudWatch RUM web client](CloudWatch-RUM-configure-client.md)
+ [Regionalization](CloudWatch-RUM-Regionalization.md)
+ [Use page groups](CloudWatch-RUM-page-groups.md)
+ [Specify custom metadata](CloudWatch-RUM-custom-metadata.md)
+ [Send custom events](CloudWatch-RUM-custom-events.md)
+ [Viewing the CloudWatch RUM dashboard](CloudWatch-RUM-view-data.md)
+ [CloudWatch metrics that you can collect with CloudWatch RUM](CloudWatch-RUM-metrics.md)
+ [Data protection and data privacy with CloudWatch RUM](CloudWatch-RUM-privacy.md)
+ [Information collected by the CloudWatch RUM web client](CloudWatch-RUM-datacollected.md)
+ [Manage your applications that use CloudWatch RUM](CloudWatch-RUM-manage.md)
+ [CloudWatch RUM quotas](CloudWatch-RUM-quotas.md)
+ [Troubleshooting CloudWatch RUM](CloudWatch-RUM-troubleshooting.md)