# Library Functions Available for Canary Scripts<a name="CloudWatch_Synthetics_Canaries_Function_Library"></a>

CloudWatch Synthetics includes several built\-in classes and functions that you can call when writing Node\.js scripts to use as canaries\.

Some apply to both UI and API canaries\. Others apply to UI canaries only\. A UI canary is a canary that uses the `getPage()` function and uses Puppeteer as a web driver to navigate and interact with webpages\.

**Topics**
+ [Library Classes and Functions that Apply to All Canaries](#CloudWatch_Synthetics_Library_allcanaries)
+ [Library Classes and Functions that Apply to UI Canaries Only](#CloudWatch_Synthetics_Library_UIcanaries)

## Library Classes and Functions that Apply to All Canaries<a name="CloudWatch_Synthetics_Library_allcanaries"></a>

The following CloudWatch Synthetics library functions are useful for all canaries\.

**Topics**
+ [getLogLevel\(\);](#CloudWatch_Synthetics_Library_getLogLevel)
+ [setLogLevel\(\);](#CloudWatch_Synthetics_Library_setLogLevel)
+ [Synthetics Logger](#CloudWatch_Synthetics_Library_SyntheticsLogger)

### getLogLevel\(\);<a name="CloudWatch_Synthetics_Library_getLogLevel"></a>

Retrieves the current log level for the Synthetics library\. Possible values are the following:
+ `0` – Debug
+ `1` – Info
+ `2` – Warn
+ `3` – Error

Example:

```
let logLevel = synthetics.getLogLevel();
```

### setLogLevel\(\);<a name="CloudWatch_Synthetics_Library_setLogLevel"></a>

Sets the log level for the Synthetics library\. Possible values are the following:
+ `0` – Debug
+ `1` – Info
+ `2` – Warn
+ `3` – Error

Example:

```
synthetics.setLogLevel(0);
```

### Synthetics Logger<a name="CloudWatch_Synthetics_Library_SyntheticsLogger"></a>

SyntheticsLogger writes logs out to both the console and to a local log file at the same log level\. This log file is written to both locations only if the log level is at or below the desired logging level of the log function that was called\.

The logging statements in the local log file are prepended with "DEBUG: ", "INFO: ", and so on to match the log level of the function that was called\.

You can use the SyntheticsLogger, assuming you want to run the Synthetics Library at the same log level as your Synthetics canary logging\.

Using the SyntheticsLogger is not required to create a log file that is uploaded to your S3 results location\. You could instead create a different log file in the `/tmp` folder\. Any files created under the `/tmp` folder are uploaded to the results location in S3 as artifacts\. 

To use the Synthetics Library logger:

```
const log = require('SyntheticsLogger');
```

Useful function definitions:

**log\.debug\(*message*, *ex*\);**

Parameters: *message* is the message to log\. *ex* is the exception, if any, to log\.

Example:

```
log.debug("Starting step - login.");
```

**log\.error\(*message*, *ex*\);**

Parameters: *message* is the message to log\. *ex* is the exception, if any, to log\.

Example:

```
try {
  await login();
catch (ex) {
  log.error("Error encountered in step - login.", ex);
}
```

**log\.info\(*message*, *ex*\);**

Parameters: *message* is the message to log\. *ex* is the exception, if any, to log\.

Example:

```
log.info("Successfully completed step - login.");
```

**log\.log\(*message*, *ex*\);**

This is an alias for `log.info`\. 

Parameters: *message* is the message to log\. *ex* is the exception, if any, to log\.

Example:

```
 log.log("Successfully completed step - login.");
```

**log\.warn\(*message*, *ex*\);**

Parameters: *message* is the message to log\. *ex* is the exception, if any, to log\.

Example:

```
log.warn("Exception encountered trying to publish CloudWatch Metric.", ex);
```

## Library Classes and Functions that Apply to UI Canaries Only<a name="CloudWatch_Synthetics_Library_UIcanaries"></a>

The following CloudWatch Synthetics library functions are useful only for UI canaries\.

**Topics**
+ [Synthetics class](#CloudWatch_Synthetics_Library_Synthetics_Class)
+ [BrokenLinkCheckerReport class](#CloudWatch_Synthetics_Library_BrokenLinkCheckerReport)
+ [SyntheticsLink class](#CloudWatch_Synthetics_Library_SyntheticsLink)

### Synthetics class<a name="CloudWatch_Synthetics_Library_Synthetics_Class"></a>

The following functions are in the Synthetics class\.

#### async addUserAgent\(page, userAgentString\);<a name="CloudWatch_Synthetics_Library_addUserAgent"></a>

This function appends *userAgentString* to the specified page's user\-agent header\.

Example:

```
await synthetics.addUserAgent(page, "MyApp-1.0");
```

Results in the page’s user\-agent header being set to `browsers-user-agent-header-valueMyApp-1.0`

#### async executeStep\(stepName = null, functionToExecute\);<a name="CloudWatch_Synthetics_Library_executeStep"></a>

Executes the provided step, wrapping it with start/pass/fail logging, start/pass/fail screenshots, and pass/fail and duration metrics\. It also does the following:
+ Logs that the step started\.
+ Takes a screenshot named `<stepName>-starting`\.
+ Starts a timer\.
+ Executes the provided function\.
+ If the function returns normally, it counts as passing\. If the function throws, it counts as failing\.
+ Ends the timer\.
+ Logs whether the step passed or failed
+ Takes a screenshot named `<stepName>-succeeded` or `<stepName>-failed`\.
+ Emits the `stepName` `SuccessPercent` metric, 100 for pass or 0 for failure\.
+ Emits the `stepName` `Duration` metric, with a value based on the step start and end times\.
+ Finally, returns what the `functionToExecute` returned or re\-throws what `functionToExecute` threw\.

If the canary uses the `syn-nodejs-2.0` runtime or later, this function also adds a step execution summary to the canary's report\. The summary includes details about each step, such as start time, end time, status \(PASSED/FAILED\), failure reason \(if failed\), and screenshots captured during the execution of each step\.

Example:

```
await synthetics.executeStep('navigateToUrl', async function (timeoutInMillis = 30000) {
           await page.goto(url, {waitUntil: ['load', 'networkidle0'], timeout: timeoutInMillis});});
```

Response:

Returns what `functionToExecute` returns\.

#### getPage\(\);<a name="CloudWatch_Synthetics_Library_getPage"></a>

Returns the current open page as a Puppeteer object\. For more information, see [Puppeteer API v1\.14\.0](https://github.com/puppeteer/puppeteer/blob/v1.14.0/docs/api.md)\.

Example:

```
let page = synthetics.getPage();
```

Response:

The page \(Puppeteer object\) that is currently open in the current browser session\.

#### getRequestResponseLogHelper\(\);<a name="CloudWatch_Synthetics_Library_getRequestResponseLogHelper"></a>

Use this function as a builder pattern for tweaking the request and response logging flags\.

Example:

```
synthetics.setRequestResponseLogHelper(getRequestResponseLogHelper().withLogRequestHeaders(false));;
```

Response:

```
{RequestResponseLogHelper}
```

#### RequestResponseLogHelper class<a name="CloudWatch_Synthetics_Library_RequestResponseLogHelper"></a>

Handles the fine\-grained configuration and creation of string representations of request and response payloads\. 

```
class RequestResponseLogHelper {
 
    constructor () {
        this.request = {url: true, resourceType: false, method: false, headers: false, postData: false};
        this.response = {status: true, statusText: true, url: true, remoteAddress: false, headers: false};
    }
 
    withLogRequestUrl(logRequestUrl);
    
    withLogRequestResourceType(logRequestResourceType);
    
    withLogRequestMethod(logRequestMethod);
    
    withLogRequestHeaders(logRequestHeaders);
    
    withLogRequestPostData(logRequestPostData);

        
    withLogResponseStatus(logResponseStatus);
    
    withLogResponseStatusText(logResponseStatusText);
   
    withLogResponseUrl(logResponseUrl);
 
    withLogResponseRemoteAddress(logResponseRemoteAddress);
    
    withLogResponseHeaders(logResponseHeaders);
```

Example:

```
synthetics.setRequestResponseLogHelper(getRequestResponseLogHelper()
.withLogRequestPostData(true)
.withLogRequestHeaders(true)
.withLogResponseHeaders(true));
```

Response:

```
{RequestResponseLogHelper}
```

#### setRequestResponseLogHelper\(\);<a name="CloudWatch_Synthetics_Library_setRequestResponseLogHelper"></a>

Use this function as a builder pattern for setting the request and response logging flags\.

Example:

```
synthetics.setRequestResponseLogHelper().withLogRequestHeaders(true).withLogResponseHeaders(true);
```

Response:

```
{RequestResponseLogHelper}
```

#### async takeScreenshot\(stepName, suffix\);<a name="CloudWatch_Synthetics_Library_takeScreenshot"></a>

This function is supported only on canaries that use the `syn-nodejs-2.0-beta` version of the runtime or later\.

Takes a screenshot \(\.PNG\) of the current page with optional `stepName` and optional suffix\. The `stepName` is the name of the step for which the screenshot is capture\. The `suffix` is an optional string to be used in naming the screenshot\.

Returns a `ScreenshotResult` object\. The screenshots are uploaded to the S3 bucket specified for the canary\.

Example:

```
await synthetics.takeScreenshot("navigateToUrl", "loaded")
```

This example captures and uploads a screenshot named `01-navigateToUrl-loaded.png` to the canary's S3 bucket\.

### BrokenLinkCheckerReport class<a name="CloudWatch_Synthetics_Library_BrokenLinkCheckerReport"></a>

This class provides methods to add a synthetics link\. It's supported only on canaries that use the `syn-nodejs-2.0-beta` version of the runtime or later\. 

To use `BrokenLinkCheckerReport`, include the following lines in the script:

```
const BrokenLinkCheckerReport = require('BrokenLinkCheckerReport');
            
const brokenLinkCheckerReport = new BrokenLinkCheckerReport();
```

Useful function definitions:

**addLink\(*syntheticsLink*, isBroken\)**

`syntheticsLink` is a `SyntheticsLink` object representing a link\. This function adds the link according to the status code\. By default, it considers a link to be broken if the status code is not available or the status code is 400 or higher\. You can override this default behavior by passing in the optional parameter `isBrokenLink` with a value of `true` or `false`\.

This function does not have a return value\.

**getLinks\(\)**

This function returns an array of `SyntheticsLink` objects that are included in the broken link checker report\.

**getTotalBrokenLinks\(\)**

This function returns a number representing the total number of broken links\.

**getTotalLinksChecked\(\)**

This function returns a number representing the total number of links included in the report\.

**How to use BrokenLinkCheckerReport**

The following canary script code snippet demonstrates an example of navigating to a link and adding it to the broken link checker report\.

1. Import `SyntheticsLink`, `BrokenLinkCheckerReport`, and `Synthetics`\.

   ```
   const BrokenLinkCheckerReport = require('BrokenLinkCheckerReport');
   const SyntheticsLink = require('SyntheticsLink');
   
   // Synthetics dependency
   const synthetics = require('Synthetics');
   ```

1. To add a link to the report, create an instance of `BrokenLinkCheckerReport`\.

   ```
   let brokenLinkCheckerReport = new BrokenLinkCheckerReport();
   ```

1. Navigate to the URL and add it to the broken link checker report\.

   ```
   let url = "https://amazon.com";
   
   let syntheticsLink = new SyntheticsLink(url);
   
   // Navigate to the url.
   let page = await synthetics.getPage();
   
   // Create a new instance of Synthetics Link
   let link = new SyntheticsLink(url)
   
   try {
       const response = await page.goto(url, {waitUntil: 'domcontentloaded', timeout: 30000});
   } catch (ex) {
       // Add failure reason if navigation fails.
       link.withFailureReason(ex);
   }
   
   if (response) {
       // Capture screenshot of destination page
       let screenshotResult = await synthetics.takeScreenshot('amazon-home', 'loaded');
      
       // Add screenshot result to synthetics link
       link.addScreenshotResult(screenshotResult);
   
       // Add status code and status description to the link
       link.withStatusCode(response.status()).withStatusText(response.statusText())
   }
   
   // Add link to broken link checker report.
   brokenLinkCheckerReport.addLink(link);
   ```

1. Add the report to Synthetics\. This creates a JSON file named `BrokenLinkCheckerReport.json` in your S3 bucket for each canary run\. You can see a links report in the console for each canary run along with screenshots, logs, and HAR files\.

   ```
   await synthetics.addReport(brokenLinkCheckerReport);
   ```

### SyntheticsLink class<a name="CloudWatch_Synthetics_Library_SyntheticsLink"></a>

This class provides methods to wrap information\. It's supported only on canaries that use the `syn-nodejs-2.0-beta` version of the runtime or later\. 

To use `SyntheticsLink`, include the following lines in the script:

```
const SyntheticsLink = require('SyntheticsLink');

const syntheticsLink = new SyntheticsLink(“https://www.amazon.com");
```

This function returns `syntheticsLinkObject`

Useful function definitions:

**withUrl\(*url*\)**

`url` is a URL string\. This function returns `syntheticsLinkObject`

**withText\(*text*\)**

`text` is a string representing anchor text\. This function returns `syntheticsLinkObject`\. It adds anchor text corresponding to the link\.

**withParentUrl\(*parentUrl*\)**

`parentUrl` is a string representing the parent \(source page\) URL\. This function returns `syntheticsLinkObject`

**withStatusCode\(*statusCode*\)**

`statusCode` is a string representing the status code\. This function returns `syntheticsLinkObject`

**withFailureReason\(*failureReason*\)**

`failureReason` is a string representing the failure reason\. This function returns `syntheticsLinkObject`

**addScreenshotResult\(*screenshotResult*\)**

`screenshotResult` is an object\. It is an instance of `ScreenshotResult` that was returned by the Synthetics function `takeScreenshot`\. The object includes the following:
+ `fileName`— A string representing the `screenshotFileName`
+ `pageUrl` \(optional\)
+ `error` \(optional\)