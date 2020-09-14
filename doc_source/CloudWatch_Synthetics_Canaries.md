# Using Synthetic Monitoring<a name="CloudWatch_Synthetics_Canaries"></a>

You can use Amazon CloudWatch Synthetics to create *canaries*, configurable scripts that run on a schedule, to monitor your endpoints and APIs\. Canaries follow the same routes and perform the same actions as a customer, which makes it possible for you to continually verify your customer experience even when you don't have any customer traffic on your applications\. By using canaries, you can discover issues before your customers do\.

Canaries are Node\.js scripts\. They create Lambda functions in your account that use Node\.js as a framework\. Canaries can use the Puppeteer Node\.js library to perform functions on your applications\. Canaries work over HTTP and HTTPS protocols\. 

Canaries check the availability and latency of your endpoints and can store load time data and screenshots of the UI\. They monitor your REST APIs, URLs, and website content, and they can check for unauthorized changes from phishing, code injection and cross\-site scripting\.

For a video demonstration of canaries, see the [Amazon CloudWatch Synthetics Demo](https://www.youtube.com/watch?v=hF3NM9j-u7I) video\.

You can run a canary once or on a regular schedule\. Scheduled canaries can run 24 hours a day, as often as once per minute\.

For information about security issues to consider before you create and run canaries, see [Security Considerations for Synthetics Canaries](servicelens_canaries_security.md)\. 

By default, canaries create CloudWatch metrics in the `CloudWatchSynthetics` namespace\. They create metrics with the names `SuccessPercent`, `Failed`, and `Duration`\. These metrics have `CanaryName` as a dimension\. Canaries that use the `executeStep()` function from the function library also have `StepName` as a dimension\. For more information about the canary function library, see [Library Functions Available for Canary Scripts](CloudWatch_Synthetics_Canaries_Function_Library.md)\.

CloudWatch Synthetics integrates well with CloudWatch ServiceLens, which uses CloudWatch with AWS X\-Ray to provide an end\-to\-end view of your services to help you more efficiently pinpoint performance bottlenecks and identify impacted users\. Canaries that you create with CloudWatch Synthetics appear on the ServiceLens service map\. For more information about ServiceLens, see [Using ServiceLens to Monitor the Health of Your Applications](ServiceLens.md)\.

CloudWatch Synthetics is currently available in the following AWS Regions:
+ US East \(N\. Virginia\)
+ US East \(Ohio\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Africa \(Cape Town\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Canada \(Central\)
+ China \(Beijing\)
+ China \(Ningxia\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Milan\)
+ Europe \(Paris\)
+ Europe \(Stockholm\)
+ Middle East \(Bahrain\)
+ South America \(São Paulo\)
+ AWS GovCloud \(US\-East\)
+ AWS GovCloud \(US\-West\)

**Topics**
+ [Required Roles and Permissions for CloudWatch Canaries](CloudWatch_Synthetics_Canaries_Roles.md)
+ [Creating a Canary](CloudWatch_Synthetics_Canaries_Create.md)
+ [Viewing Canary Statistics and Details](CloudWatch_Synthetics_Canaries_Details.md)
+ [Editing or Deleting a Canary](synthetics_canaries_deletion.md)