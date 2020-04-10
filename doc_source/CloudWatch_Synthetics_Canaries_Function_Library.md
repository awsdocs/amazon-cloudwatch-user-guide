# Library Functions Available For Canary Scripts<a name="CloudWatch_Synthetics_Canaries_Function_Library"></a>

CloudWatch Synthetics includes several built\-in functions which you can call when writing Node\.js scripts to use as canaries\.

Some functions are applicable to both UI and API canaries, while others are applicable only to UI canaries\. A UI canary is a canary that uses the **getPage\(\)** function and uses Puppeteer as a web driver to navigate and interact with web pages\.

**Topics**
+ [Library Functions that Apply to All Canaries](#CloudWatch_Synthetics_Library_allcanaries)
+ [Library Functions that Apply to Only UI Canaries](#CloudWatch_Synthetics_Library_UIcanaries)

## Library Functions that Apply to All Canaries<a name="CloudWatch_Synthetics_Library_allcanaries"></a>

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

You can use the SyntheticsLogger assuming you want to run the Synethetics Library at the same log level as your Synthetics canary logging\.

Using the SyntheticsLogger is not required to create a log file that is uploaded your S3 results location\. You could instead create a different log file in the `/tmp` folder\. Any files created under the `/tmp` folder are uploaded to the results location in S3 as artifacts\. 

To use the Synthetics Library logger:

```
const log = require('SyntheticsLogger');
```

Useful function definitions:

**log\.debug\(*message*, *ex*\);**

Parameters: *message* is the message to log, and *ex* is the exception, if any, to log

Example:

```
log.debug("Starting step - login.");
```

**log\.error\(*message*, *ex*\);**

Parameters: *message* is the message to log, and *ex* is the exception, if any, to log

Example:

```
try {
  await login();
catch (ex) {
  log.error("Error encountered in step - login.", ex);
}
```

**log\.info\(*message*, *ex*\);**

Parameters: *message* is the message to log, and *ex* is the exception, if any, to log

Example:

```
log.info("Successfully completed step - login.");
```

**log\.log\(*message*, *ex*\);**

This is an alias for `log.info`\. 

Parameters: *message* is the message to log, and *ex* is the exception, if any, to log

Example:

```
 log.log("Successfully completed step - login.");
```

**log\.warn\(*message*, *ex*\);**

Parameters: *message* is the message to log, and *ex* is the exception, if any, to log

Example:

```
log.warn("Exception encountered trying to publish CloudWatch Metric.", ex);
```

## Library Functions that Apply to Only UI Canaries<a name="CloudWatch_Synthetics_Library_UIcanaries"></a>

The following CloudWatch Synthetics library functions are useful only for UI canaries\.

**Topics**
+ [async addUserAgent\(page, userAgentString\);](#CloudWatch_Synthetics_Library_addUserAgent)
+ [async executeStep\(stepName = null, functionToExecute\);](#CloudWatch_Synthetics_Library_executeStep)
+ [getPage\(\);](#CloudWatch_Synthetics_Library_getPage)
+ [getRequestResponseLogHelper\(\);](#CloudWatch_Synthetics_Library_getRequestResponseLogHelper)
+ [RequestResponseLogHelper class](#CloudWatch_Synthetics_Library_RequestResponseLogHelper)
+ [setRequestResponseLogHelper\(\);](#CloudWatch_Synthetics_Library_setRequestResponseLogHelper)
+ [async takeScreenshot\(stepName, suffix\);](#CloudWatch_Synthetics_Library_takeScreenshot)

### async addUserAgent\(page, userAgentString\);<a name="CloudWatch_Synthetics_Library_addUserAgent"></a>

This function appends *userAgentString* to the specified page's user\-agent header\.

Example:

```
await synthetics.addUserAgent(page, "MyApp-1.0");
```

Results in the page’s user\-agent header being set to `browsers-user-agent-header-valueMyApp-1.0`

### async executeStep\(stepName = null, functionToExecute\);<a name="CloudWatch_Synthetics_Library_executeStep"></a>

Executes the provided step, wrapping it with start/pass/fail logging, start/pass/fail screenshots, and pass/fail and duration metrics\. In further detail, it does the following:
+ Logs that the step started\.
+ Takes a screenshot called **<stepName>\-starting**\.
+ Starts a timer\.
+ Executes the provided function\.
+ If the function returns normally, it counts as passing\. If the function throws, it counts as failing\.
+ End the timer\.
+ Logs whether the step passed or failed
+ Takes a screenshot called **<stepName>\-succeeded** or **<stepName>\-failed**\.
+ Emits the `stepName` **SuccessPercent** metric, 100 for pass or 0 for failure\.
+ Emits the `stepName` **Duration** metric, with a value based on the step start and end times\.
+ Finally, returns what the `functionToExecute` returned or re\-throws what `functionToExecute` threw\.

Example:

```
await synthetics.executeStep('navigateToUrl', async function (timeoutInMillis = 30000) {
           await page.goto(url, {waitUntil: ['load', 'networkidle0'], timeout: timeoutInMillis});});
```

Response:

Returns what `functionToExecute` returns\.

### getPage\(\);<a name="CloudWatch_Synthetics_Library_getPage"></a>

Returns the current open page as a Puppeteer object\. For more information, see [Puppeteer API v1\.14\.0](https://github.com/puppeteer/puppeteer/blob/v1.14.0/docs/api.md)\.

Example:

```
let page = synthetics.getPage();
```

Response:

The page \(Puppeteer object\) that is currently open in the current browser session\.

### getRequestResponseLogHelper\(\);<a name="CloudWatch_Synthetics_Library_getRequestResponseLogHelper"></a>

Use this function as a builder pattern for tweaking the request and response logging flags\.

Example:

```
synthetics.setRequestResponseLogHelper(getRequestResponseLogHelper().withLogRequestHeaders(false));;
```

Response:

```
{RequestResponseLogHelper}
```

### RequestResponseLogHelper class<a name="CloudWatch_Synthetics_Library_RequestResponseLogHelper"></a>

Handles the fine grained configuration and creation of string representations of request and response payloads\. More details:

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

### setRequestResponseLogHelper\(\);<a name="CloudWatch_Synthetics_Library_setRequestResponseLogHelper"></a>

Use this function as a builder pattern for setting the request and response logging flags\.

Example:

```
synthetics.setRequestResponseLogHelper().withLogRequestHeaders(true).withLogResponseHeaders(true);
```

Response:

```
{RequestResponseLogHelper}
```

### async takeScreenshot\(stepName, suffix\);<a name="CloudWatch_Synthetics_Library_takeScreenshot"></a>

Takes a screenshot \(\.PNG\) of the current page with optional stepName and optional suffix\.

Examples:

```
await synthetics.takeScreenshot(); // 00_screenshot.png
```

```
await synthetics.takeScreenshot("Cart"); // 01_Cart.png
```

```
await synthetics.takeScreenshot("Cart", "starting"); // 02_Cart-starting.png
```