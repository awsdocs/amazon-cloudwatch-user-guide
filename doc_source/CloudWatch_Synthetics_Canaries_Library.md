# Synthetics runtime versions<a name="CloudWatch_Synthetics_Canaries_Library"></a>

When you create or update a canary, you choose a Synthetics runtime version for the canary\. A Synthetics runtime is a combination of the Synthetics code that calls your script handler, and the Lambda layers of bundled dependencies\.

CloudWatch Synthetics currently supports runtimes that use Node\.js for scripts and the Puppeteer framework, and runtimes that use Python for scripting and Selenium Webdriver for the framework\.

We recommend that you always use the most recent runtime version for your canaries, to be able to use the latest features and updates made to the Synthetics library\.

**Topics**
+ [CloudWatch Synthetics runtime support policy](#CloudWatch_Synthetics_Canaries_runtime_support)
+ [Runtime versions using Node\.js and Puppeteer](CloudWatch_Synthetics_Library_nodejs_puppeteer.md)
+ [Runtime versions using Python and Selenium Webdriver](CloudWatch_Synthetics_Library_python_selenium.md)

## CloudWatch Synthetics runtime support policy<a name="CloudWatch_Synthetics_Canaries_runtime_support"></a>

Synthetics runtime versions are subject to maintenance and security updates\. When any component of a runtime version is no longer supported, that Synthetics runtime version is deprecated\.

You can't create canaries using deprecated runtime versions\. Canaries that use deprecated runtimes continue to run\. You can stop, start, and delete these canaries\. You can update an existing canary that uses a deprecated runtime version by updating the canary to use a supported runtime version\.

CloudWatch Synthetics notifies you by email if you have canaries that use runtimes that are scheduled to be deprecated in the next 60 days\. We recommend that you migrate your canaries to a supported runtime version to benefit from the new functionality, security, and performance enhancements that are included in more recent releases\. 

**How do I update a canary to a new runtime version?**

You can update a canary’s runtime version by using the CloudWatch console, AWS CloudFormation, the AWS CLI or the AWS SDK\. When you use the CloudWatch console, you can update up to five canaries at once by selecting them in the canary list page and then choosing **Actions**, **Update Runtime**\.

You can verify the upgrade by first cloning the canary using the CloudWatch console and updating its runtime version\. This creates another canary which is a clone of your original canary\. Once you have verified your canary with the new runtime version, you can update the runtime version of your original canary and delete the clone canary\.

 You can also update multiple canaries using an upgrade script\. For more information, see [Canary runtime upgrade script](CloudWatch_Synthetics_Canaries_upgrade_script.md)\.

If you upgrade a canary and it fails, see [Troubleshooting a failed canary](CloudWatch_Synthetics_Canaries_Troubleshoot.md)\.

**Runtime deprecation dates**


| Runtime Version | Deprecation date | 
| --- | --- | 
|  `syn-nodejs-2.2`  |  May 28, 2021  | 
|  `syn-nodejs-2.1`  |  May 28, 2021  | 
|  `syn-nodejs-2.0`  |  May 28, 2021  | 
|  `syn-nodejs-2.0-beta`  |  February 8, 2021  | 
|  `syn-1.0`  |  May 28, 2021  | 