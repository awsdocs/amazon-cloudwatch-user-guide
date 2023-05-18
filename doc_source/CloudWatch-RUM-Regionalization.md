# Regionalization<a name="CloudWatch-RUM-Regionalization"></a>

This section illustrates strategies for using CloudWatch RUM with applications in different Regions\.

## My web application is deployed in multiple AWS Regions<a name="CloudWatch-RUM-Regionalization-multiple"></a>

If your web application is deployed in multipled AWS Regions, you have three options:
+ Deploy one app monitor in one Region, in one account, serving all Regions\.
+ Deploy separate app monitors for each Region, in unique accounts\.
+ Deploy separate app monitors for each Region, all in one account\.

The advantage of using one app monitor is that all data will be centralized into one visualization, and all logs are written to the same log group in CloudWatch Logs\. With a single app monitor there is a small amount of extra latency for requests, and a single point of failure\.

Using multiple app monitors removes the single point of failure, but prevents all data from being combined into one visualization\.

### CloudWatch RUM hasn't launched in some Regions that my application is deployed in<a name="CloudWatch-RUM-Regionalization-notavailable"></a>

CloudWatch RUM is launched into many Regions and has wide geographical coverage\. By setting up CloudWatch RUM in the Regions where it is available, you can get the benefits\. End users can be anywhere and still have their sessions included if you have set up an app monitor in the Region that they are connecting to\.

However, CloudWatch RUM is not yet launched in AWS GovCloud \(US\-East\), AWS GovCloud \(US\-West\), or any Regions in China\. You are not able to send data to CloudWatch RUM from these Regions\.