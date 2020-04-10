# Viewing Canary Statistics and Details<a name="CloudWatch_Synthetics_Canaries_Details"></a>


****  

|  | 
| --- |
| CloudWatch Synthetics is in open preview\. The preview is currently available in the following Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries_Details.html) The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [cw\-synthetics@amazon\.com](mailto:cw-synthetics@amazon.com) | 

You can view details about your canaries and see statistics about their runs\.

To be able to see all the details about your canary test run results, you must be logged on to an account that has sufficient permissions\. For more information, see [Necessary Roles and Permissions for CloudWatch Canaries](CloudWatch_Synthetics_Canaries_Roles.md)\.

**To view canary statistics and details**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

   CloudWatch displays a screen with details about the canaries that you have created\.
   + **Status** visually shows how many of your canaries have passed their most recent test runs\.
   + In the graph under **Canary test runs**, each point represents one minute of your canaries’ test runs\. You can pause on a point to see details\.

1. To see more details about a single canary, choose a point in the **Status** graph or choose the name of the canary in the **Canaries** table\.

   CloudWatch displays a screen with details about that canary\.
   + Under **Canary test runs**, you can choose one of the lines to see details about that particular test run\.
   + Under the graph, you can choose **Screenshot**, **HAR file**, or **Logs** to see these types of details\.

     The logs for canary test runs are stored in Amazon S3 buckets, not in CloudWatch Logs\.