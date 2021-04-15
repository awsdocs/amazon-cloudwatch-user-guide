# Using synthetic monitoring<a name="CloudWatch_Synthetics_Canaries"></a>

You can use Amazon CloudWatch Synthetics to create *canaries*, configurable scripts that run on a schedule, to monitor your endpoints and APIs\. Canaries follow the same routes and perform the same actions as a customer, which makes it possible for you to continually verify your customer experience even when you don't have any customer traffic on your applications\. By using canaries, you can discover issues before your customers do\.

Canaries are scripts written in Node\.js or Python\. They create Lambda functions in your account that use Node\.js or Python as a framework\. Canaries work over both HTTP and HTTPS protocols\.

Canaries offer programmatic access to a headless Google Chrome Browser via Puppeteer or Selenium Webdriver\. For more information about Puppeteer, see [Puppeteer](https://developers.google.com/web/tools/puppeteer)\. For more information about Selenium, see [www\.selenium\.dev/](https://www.selenium.dev)\.

Canaries check the availability and latency of your endpoints and can store load time data and screenshots of the UI\. They monitor your REST APIs, URLs, and website content, and they can check for unauthorized changes from phishing, code injection and cross\-site scripting\.

For a video demonstration of canaries, see the [Amazon CloudWatch Synthetics Demo](https://www.youtube.com/watch?v=hF3NM9j-u7I) video\.



You can run a canary once or on a regular schedule\. Scheduled canaries can run 24 hours a day, as often as once per minute\.

For information about security issues to consider before you create and run canaries, see [Security Considerations for Synthetics Canaries](servicelens_canaries_security.md)\. 

By default, canaries create several CloudWatch metrics in the `CloudWatchSynthetics` namespace\. These metrics have `CanaryName` as a dimension\. Canaries that use the `executeStep()` or `executeHttpStep()` function from the function library also have `StepName` as a dimension\. For more information about the canary function library, see [Library functions available for canary scripts](CloudWatch_Synthetics_Canaries_Function_Library.md)\.

CloudWatch Synthetics integrates well with CloudWatch ServiceLens, which uses CloudWatch with AWS X\-Ray to provide an end\-to\-end view of your services to help you more efficiently pinpoint performance bottlenecks and identify impacted users\. Canaries that you create with CloudWatch Synthetics appear on the ServiceLens service map\. For more information about ServiceLens, see [Using ServiceLens to Monitor the Health of Your Applications](ServiceLens.md)\.

CloudWatch Synthetics is currently available in all commerical AWS Regions and the GovCloud Regions\.

**Topics**
+ [Required roles and permissions for CloudWatch canaries](CloudWatch_Synthetics_Canaries_Roles.md)
+ [Creating a canary](CloudWatch_Synthetics_Canaries_Create.md)
+ [Troubleshooting a failed canary](CloudWatch_Synthetics_Canaries_Troubleshoot.md)
+ [Sample code for canary scripts](CloudWatch_Synthetics_Canaries_Samples.md)
+ [Canaries and X\-Ray tracing](CloudWatch_Synthetics_Canaries_tracing.md)
+ [Running a canary on a VPC](CloudWatch_Synthetics_Canaries_VPC.md)
+ [Viewing canary statistics and details](CloudWatch_Synthetics_Canaries_Details.md)
+ [CloudWatch metrics published by canaries](CloudWatch_Synthetics_Canaries_metrics.md)
+ [Editing or Deleting a Canary](synthetics_canaries_deletion.md)
+ [Monitoring canary events with Amazon EventBridge](monitoring-events-eventbridge.md)