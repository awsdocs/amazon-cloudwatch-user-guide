# Using Synthetic Monitoring<a name="CloudWatch_Synthetics_Canaries"></a>


****  

|  | 
| --- |
| CloudWatch Synthetics is in open preview\. The preview is currently available in the following Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html) The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [cw\-synthetics@amazon\.com](mailto:cw-synthetics@amazon.com) | 

Amazon CloudWatch Synthetics enables you to create canaries to monitor your endpoints and APIs\. Canaries are configurable scripts that follow the same routes and perform the same actions as a customer\. This enables the outside\-in view of your customers’ experiences, and your service’s availability from their point of view\.

You can run a canary once, or on a regular schedule\. Scheduled canaries can run 24 hours a day, as often as once per minute\. The canaries check the availability and latency of your endpoints, and can store load time data and screenshots of the UI\.

For information about security issues to consider when creating and running canaries, see [Security Considerations for Synthetics Canaries](servicelens_canaries_security.md)\. 

CloudWatch Synthetics integrates well with CloudWatch ServiceLens\. ServiceLens integrates CloudWatch with AWS X\-Ray to provide an end\-to\-end view of your services to help you more efficiently pinpoint performance bottlenecks and identify impacted users\. Canaries that you create with CloudWatch Synthetics appear on the ServiceLens service map\. For more information about ServiceLens, see [Using ServiceLens to Monitor the Health of Your Applications](ServiceLens.md)\.

**Topics**
+ [Necessary Roles and Permissions for CloudWatch Canaries](CloudWatch_Synthetics_Canaries_Roles.md)
+ [Creating a Canary](CloudWatch_Synthetics_Canaries_Create.md)
+ [Viewing Canary Statistics and Details](CloudWatch_Synthetics_Canaries_Details.md)
+ [Deleting a Canary](synthetics_canaries_deletion.md)