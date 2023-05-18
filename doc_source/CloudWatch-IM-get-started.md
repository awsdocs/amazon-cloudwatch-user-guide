# Getting started with Amazon CloudWatch Internet Monitor using the console<a name="CloudWatch-IM-get-started"></a>

To get started with Amazon CloudWatch Internet Monitor, you must create a *monitor* in Internet Monitor for your application by adding AWS resources that it uses and then setting a few configuration options\. The resources that you add, Amazon Virtual Private Clouds \(VPCs\), CloudFront distributions, or WorkSpaces directories, provide the information for Internet Monitor to map internet traffic information for your application\. Then you can visualize and explore performance and availability about client usage, using your monitor\.

Typically, it's simplest to create one monitor in Internet Monitor for one application\. Within the same monitor, you can search and sort through measurements and metrics in Internet Monitor log files by different locations and ASNs \(such as, internet service providers\) and so on\. It's not necessary to create separate monitors for applications in different areas, for example\. 

 The steps here walk you through setting up your monitor by using the console\. To see examples of using the AWS Command Line Interface with the Internet Monitor API actions, to create a monitor, view events, and so on, see [Examples of using the CLI with Amazon CloudWatch Internet Monitor](CloudWatch-IM-get-started-CLI.md)\.

**To create a monitor**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, under **Application monitoring**, choose **Internet Monitor**\.

1. Choose **Add monitor**\.

1. For **Monitor name**, enter the name you want to use for this monitor in Internet Monitor\.

1. Choose **Add resources**, and then select the resources to set the monitoring boundaries for Internet Monitor to use for this monitor\.
**Note**  
VPCs that you add must be connected to the internet with an Internet Gateway configured\.  
If you add VPCs and CloudFront distributions, you can't also add WorkSpaces directories at the same time\.

1. Choose a percentage of your internet traffic to monitor\. 

1. Optionally, under **City\-networks maximum limit**, select a limit for the number of city\-networks \(locations and ASNs, or internet service providers\) that Internet Monitor will monitor traffic for\. You can change this at any time by editing your monitor\. See [Choosing a city\-network maximum limit](IMCityNetworksMaximum.md)\. 

   Note: The city\-networks maximum limit caps the number of city\-networks that Internet Monitor monitors for your application, regardless of the percentage of traffic that you choose to monitor\.

1. Optionally, specify an Amazon S3 bucket name and custom prefix to publish internet measurements to Amazon S3 for all monitored city\-networks\. 

   Internet Monitor publishes the top 500 \(by traffic volume\) internet measurements for your application to CloudWatch Logs every five minutes\. If you choose to publish measurements to S3, measurements are still published to CloudWatch Logs\. For more information, see [Publishing internet measurements to Amazon S3 in Amazon CloudWatch Internet Monitor](CloudWatch-IM-get-started.Publish-to-S3.md)\.

1. Choose **Next**\.

1. Optionally, add a tag for your monitor\.

1. Choose **Next**\.

1. Review your monitor, including the resources that you've chosen to add\.

1. Choose **Create monitor**\.

After you create a monitor, you can edit the monitor at any time to change the maximum city\-networks limit or add or remove resources, you can delete the monitor, or make other changes\. To do these tasks in the Internet Monitor console, select a monitor, and then choose an option in the **Action** menu\. Note that you can’t change the name of the monitor\.

**To view the Internet Monitor dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, then **Internet Monitor**\.

   The **Monitors** tab displays a list of the monitors that you have created\. 

To see more information about a monitor, choose a monitor\.

**Topics**
+ [Choosing a city\-network maximum limit](IMCityNetworksMaximum.md)
+ [Publishing internet measurements to Amazon S3 in Amazon CloudWatch Internet Monitor](CloudWatch-IM-get-started.Publish-to-S3.md)
+ [Using an Internet Monitor monitor](IMWhyCreateMonitor.md)
+ [Edit or delete an Internet Monitor monitor](CloudWatch-IM-get-started.edit-monitor.md)