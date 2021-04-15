# Viewing canary statistics and details<a name="CloudWatch_Synthetics_Canaries_Details"></a>

You can view details about your canaries and see statistics about their runs\.

To be able to see all the details about your canary run results, you must be logged on to an account that has sufficient permissions\. For more information, see [Required roles and permissions for CloudWatch canaries](CloudWatch_Synthetics_Canaries_Roles.md)\.

**To view canary statistics and details**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Canaries**\.

   In the details about the canaries that you have created:
   + **Status** visually shows how many of your canaries have passed their most recent runs\.
   + In the graph under **Canary runs**, each point represents one minute of your canaries’ runs\. You can pause on a point to see details\.
   + Near the bottom of the page is a table displaying all canaries\. A column on the right shows the alarms created for each canary\. Only alarms that conform to the naming standard for canary alarms are displayed\. This standard is `Synthetics-Alarm-canaryName-index `\. Canary alarms that you create in the **Synthetics** section of the CloudWatch console automatically use this naming convention\. If you create canary alarms in the **Alarms** section of the CloudWatch console or by using AWS CloudFormation, and you don't use this naming convention, the alarms work but they do not appear in this list\.

1. To see more details about a single canary, choose a point in the **Status** graph or choose the name of the canary in the **Canaries** table\.

   In the details about that canary:
   + The **Availability** tab displays information about the recent runs of this canary\.

     Under **Canary runs**, you can choose one of the lines to see details about that run\.

     Under the graph, you can choose **Links checked**, **Screenshot**, **HAR file**, or **Logs** to see these types of details\. If the canary has active tracing enabled, you can also choose **Traces** to see tracing information from the canary's runs\.

     The logs for canary runs are stored in S3 buckets and in CloudWatch Logs\.

     Screenshots show how your customers view your webpages\. You can use the HAR files \(HTTP Archive files\) to view detailed performance data about the webpages\. You can analyze the list of web requests and catch performance issues such as time to load for an item\. Log files show the record of interactions between the canary run and the webpage and can be used to identify details of errors\.

     If the canary uses the `syn-nodejs-2.0-beta` runtime or later, you can sort the HAR files by status code, request size, or duration\.

     If the canary uses the `syn-nodejs-2.0-beta` runtime or later and the canary executes steps in its script, you can choose a **Steps** tab\. This tab displays a list of the canary's steps, each step's status, failure reason, URL after step execution, screenshots, and duration of step execution\. For API canaries with HTTP steps, you can view steps and corresponding HTTP requests if you are using runtime `syn-nodejs-2.2` or later\.

     Choose the **HTTP Requests** tab to view the log of each HTTP request made by the canary\. You can view request/response headers, response body, status code, error and performance timings \(total duration, TCP connection time, TLS handshake time, first byte time, and content transfer time\)\. All HTTP requests which use the HTTP/HTTPS module under the hood are captured here\.

     By default in API canaries, the request header, response header, request body, and response body are not included in the report for security reasons\. If you choose to include them, the data is stored only in your S3 bucket\. For information about how to include this data in the report, see [executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_executeHttpStep)\.

     Response body content types of text, HTML and JSON are supported\. Content types like text/HTML, text/plain, application/JSON and application/x\-amz\-json\-1\.0 are supported\. Compressed responses are not supported\. 
   + The **Monitoring** tab displays graphs of the CloudWatch metrics published by this canary\. For more information about these metrics, see [CloudWatch metrics published by canaries](CloudWatch_Synthetics_Canaries_metrics.md)\.

     Below the CloudWatch graphics published by the canary are graphs of Lambda metrics related to the canary's Lambda code\.
   + The **Configuration** tab displays configuration and schedule information about the canary\.
   + The **Tags** tab displays the tags associated with the canary\.