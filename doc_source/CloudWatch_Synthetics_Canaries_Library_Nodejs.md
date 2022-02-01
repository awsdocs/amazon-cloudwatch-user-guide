# Library functions available for Node\.js canary scripts<a name="CloudWatch_Synthetics_Canaries_Library_Nodejs"></a>

This section lists the library functions available for Node\.js canary scripts\.

**Topics**
+ [Node\.js library classes and functions that apply to all canaries](#CloudWatch_Synthetics_Library_allcanaries)
+ [Node\.js library classes and functions that apply to UI canaries only](#CloudWatch_Synthetics_Library_UIcanaries)
+ [Node\.js library classes and functions that apply to API canaries only](#CloudWatch_Synthetics_Library_APIcanaries)

## Node\.js library classes and functions that apply to all canaries<a name="CloudWatch_Synthetics_Library_allcanaries"></a>

The following CloudWatch Synthetics library functions for Node\.js are useful for all canaries\.

**Topics**
+ [Synthetics class](#CloudWatch_Synthetics_Library_Synthetics_Class_all)
+ [SyntheticsConfiguration class](#CloudWatch_Synthetics_Library_SyntheticsConfiguration)
+ [Synthetics logger](#CloudWatch_Synthetics_Library_SyntheticsLogger)
+ [SyntheticsLogHelper class](#CloudWatch_Synthetics_Library_SyntheticsLogHelper)

### Synthetics class<a name="CloudWatch_Synthetics_Library_Synthetics_Class_all"></a>

The following functions for all canaries are in the Synthetics class\.

#### addExecutionError\(errorMessage, ex\);<a name="CloudWatch_Synthetics_Library_addExecutionError"></a>

`errorMessage` describes the error and `ex` is the exception that is encountered

You can use `addExecutionError` to set execution errors for your canary\. It fails the canary without interrupting the script execution\. It also doesn't impact your `successPercent` metrics\.

You should track errors as execution errors only if they are not important to indicate the success or failure of your canary script\.

An example of the use of `addExecutionError` is the following\. You are monitoring the availability of your endpoint and taking screenshots after the page has loaded\. Because the failure of taking a screenshot doesn't determine availability of the endpoint, you can catch any errors encountered while taking screenshots and add them as execution errors\. Your availability metrics will still indicate that the endpoint is up and running, but your canary status will be marked as failed\. The following sample code block catches such an error and adds it as an execution error\.

```
try {
        await synthetics.takeScreenshot(stepName, "loaded");
    } catch(ex) {
        synthetics.addExecutionError('Unable to take screenshot ', ex);
    }
```

#### getCanaryName\(\);<a name="CloudWatch_Synthetics_Library_getCanaryName"></a>

Returns the name of the canary\.

#### getRuntimeVersion\(\);<a name="CloudWatch_Synthetics_Library_getRuntimeVersion"></a>

This function is available in runtime version `syn-nodejs-puppeteer-3.0` and later\. It returns the Synthetics runtime version of the canary\. For example, the return value could be `syn-nodejs-puppeteer-3.0`\.

#### getLogLevel\(\);<a name="CloudWatch_Synthetics_Library_getLogLevel"></a>

Retrieves the current log level for the Synthetics library\. Possible values are the following:
+ `0` – Debug
+ `1` – Info
+ `2` – Warn
+ `3` – Error

Example:

```
let logLevel = synthetics.getLogLevel();
```

#### setLogLevel\(\);<a name="CloudWatch_Synthetics_Library_setLogLevel"></a>

Sets the log level for the Synthetics library\. Possible values are the following:
+ `0` – Debug
+ `1` – Info
+ `2` – Warn
+ `3` – Error

Example:

```
synthetics.setLogLevel(0);
```

### SyntheticsConfiguration class<a name="CloudWatch_Synthetics_Library_SyntheticsConfiguration"></a>

This class is available only in the `syn-nodejs-2.1` runtime version or later\.

You can use the SyntheticsConfiguration class to configure the behavior of Synthetics library functions\. For example, you can use this class to configure the `executeStep()` function to not capture screenshots\.

You can set CloudWatch Synthetics configurations at the global level, which are applied to all steps of canaries\. You can also override these configurations at the step level by passing configuration key/value pairs\.

You can pass in options at the step level\. For examples, see [async executeStep\(stepName, functionToExecute, \[stepConfig\]\);](#CloudWatch_Synthetics_Library_executeStep) and [executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)](#CloudWatch_Synthetics_Library_executeHttpStep)

Function definitions:

#### setConfig\(options\)<a name="CloudWatch_Synthetics_Library_setConfig"></a>

`options` is an object, which is a set of configurable options for your canary\. The following sections explain the possible fields in `options`\.

##### setConfig\(options\) for all canaries<a name="CloudWatch_Synthetics_Library_setConfigall"></a>

For canaries using `syn-nodejs-puppeteer-3.2` or later, the **\(options\)** for **setConfig** can include the following parameters:
+ `includeRequestHeaders` \(boolean\)— Whether to include request headers in the report\. The default is `false`\.
+ `includeResponseHeaders` \(boolean\)— Whether to include response headers in the report\. The default is `false`\.
+ `restrictedHeaders` \(array\)— A list of header values to ignore, if headers are included\. This applies to both request and response headers\. For example, you can hide your credentials by passing **includeRequestHeaders** as `true` and **restrictedHeaders** as `['Authorization']`\. 
+ `includeRequestBody` \(boolean\)— Whether to include the request body in the report\. The default is `false`\.
+ `includeResponseBody` \(boolean\)— Whether to include the response body in the report\. The default is `false`\.

**setConfig\(options\) regarding CloudWatch metrics**

For canaries using `syn-nodejs-puppeteer-3.1` or later, the **\(options\)** for **setConfig** can include the following Boolean parameters that determine which metrics are published by the canary\. The default for each of these options is `true`\. The options that start with `aggregated` determine whether the metric is emitted without the `CanaryName` dimension\. You can use these metrics to see the aggregated results for all of your canaries\. The other options determine whether the metric is emitted with the `CanaryName` dimension\. You can use these metrics to see results for each individual canary\.

For a list of CloudWatch metrics emitted by canaries, see [CloudWatch metrics published by canaries](CloudWatch_Synthetics_Canaries_metrics.md)\.
+ `failedCanaryMetric` \(boolean\)— Whether to emit the `Failed` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `failedRequestsMetric` \(boolean\)— Whether to emit the `Failed requests` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `_2xxMetric` \(boolean\)— Whether to emit the `2xx` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `_4xxMetric` \(boolean\)— Whether to emit the `4xx` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `_5xxMetric` \(boolean\)— Whether to emit the `5xx` metric \(with the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `stepDurationMetric` \(boolean\)— Whether to emit the `Step duration` metric \(with the `CanaryName` `StepName` dimensions\) for this canary\. The default is `true`\.
+ `stepSuccessMetric` \(boolean\)— Whether to emit the `Step success` metric \(with the `CanaryName` `StepName` dimensions\) for this canary\. The default is `true`\.
+ `aggregatedFailedCanaryMetric` \(boolean\)— Whether to emit the `Failed` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregatedFailedRequestsMetric` \(boolean\)— Whether to emit the `Failed Requests` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated2xxMetric` \(boolean\)— Whether to emit the `2xx` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated4xxMetric` \(boolean\)— Whether to emit the `4xx` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `aggregated5xxMetric` \(boolean\)— Whether to emit the `5xx` metric \(without the `CanaryName` dimension\) for this canary\. The default is `true`\.
+ `visualMonitoringSuccessPercentMetric` \(boolean\)— Whether to emit the `visualMonitoringSuccessPercent` metric for this canary\. The default is `true`\.
+ `visualMonitoringTotalComparisonsMetric` \(boolean\)— Whether to emit the `visualMonitoringTotalComparisons` metric for this canary\. The default is `false`\.
+ `stepsReport` \(boolean\)— Whether to report a step execution summary\. The default is `true`\.
+ `includeUrlPassword` \(boolean\)— Whether to include a password that appears in the URL\. By default, passwords that appear in URLs are redacted from logs and reports, to prevent disclosing sensitive data\. The default is `false`\.
+ `restrictedUrlParameters` \(array\)— A list of URL path or query parameters to redact\. This applies to URLs appearing in logs, reports, and errors\. The parameter is case\-insensitive\. You can pass an asterisk \(\*\) as a value to redact all URL path and query parameter values\. The default is an empty array\.
+ `logRequest` \(boolean\)— Whether to log every request in canary logs\. For UI canaries, this logs each request sent by the browser\. The default is `true`\.
+ `logResponse` \(boolean\)— Whether to log every response in canary logs\. For UI canaries, this logs every response received by the browser\. The default is `true`\.
+ `logRequestBody` \(boolean\)— Whether to log request bodies along with the requests in canary logs\. This configuration applies only if `logRequest` is `true`\. The default is `false`\.
+ `logResponseBody` \(boolean\)— Whether to log response bodies along with the responses in canary logs\. This configuration applies only if `logResponse` is `true`\. The default is `false`\.
+ `logRequestHeaders` \(boolean\)— Whether to log request headers along with the requests in canary logs\. This configuration applies only if `logRequest` is `true`\. The default is `false`\.

  Note that `includeRequestHeaders` enables headers in artifacts\.
+ `logResponseHeaders` \(boolean\)— Whether to log response headers along with the responses in canary logs\. This configuration applies only if `logResponse` is `true`\. The default is `false`\.

  Note that `includeResponseHeaders` enables headers in artifacts\.

**Note**  
The `Duration` and `SuccessPercent` metrics are always emitted for each canary, both with and without the `CanaryName` metric\. 

##### Methods to enable or disable metrics<a name="CloudWatch_Synthetics_Library_setConfig_metrics"></a>

**disableAggregatedRequestMetrics\(\)**

Disables the canary from emitting all request metrics that are emitted with no `CanaryName` dimension\.

**disableRequestMetrics\(\)**

Disables all request metrics, including both per\-canary metrics and metrics aggregated across all canaries\.

**enableAggregatedRequestMetrics\(\)**

Enables the canary to emit all request metrics that are emitted with no `CanaryName` dimension\.

**enableRequestMetrics\(\)**

Enables all request metrics, including both per\-canary metrics and metrics aggregated across all canaries\.

**get2xxMetric\(\)**

Returns whether the canary emits a `2xx` metric with the `CanaryName` dimension\.

**get4xxMetric\(\)**

Returns whether the canary emits a `4xx` metric with the `CanaryName` dimension\.

**get5xxMetric\(\)**

Returns whether the canary emits a `5xx` metric with the `CanaryName` dimension\.

**getAggregated2xxMetric\(\)**

Returns whether the canary emits a `2xx` metric with no dimension\.

**getAggregated4xxMetric\(\)**

Returns whether the canary emits a `4xx` metric with no dimension\.

**getAggregatedFailedCanaryMetric\(\)**

Returns whether the canary emits a `Failed` metric with no dimension\.

**getAggregatedFailedRequestsMetric\(\)**

Returns whether the canary emits a `Failed requests` metric with no dimension\.

**getAggregated5xxMetric\(\)**

Returns whether the canary emits a `5xx` metric with no dimension\.

**getFailedCanaryMetric\(\)**

Returns whether the canary emits a `Failed` metric with the `CanaryName` dimension\.

**getFailedRequestsMetric\(\)**

Returns whether the canary emits a `Failed requests` metric with the `CanaryName` dimension\.

**getStepDurationMetric\(\)**

Returns whether the canary emits a `Duration` metric with the `CanaryName` dimension for this canary\.

**getStepSuccessMetric\(\)**

Returns whether the canary emits a `StepSuccess` metric with the `CanaryName` dimension for this canary\.

**with2xxMetric\(\_2xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `2xx` metric with the `CanaryName` dimension for this canary\.

**with4xxMetric\(\_4xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `4xx` metric with the `CanaryName` dimension for this canary\.

**with5xxMetric\(\_5xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `5xx` metric with the `CanaryName` dimension for this canary\.

**withAggregated2xxMetric\(aggregated2xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `2xx` metric with no dimension for this canary\.

**withAggregated4xxMetric\(aggregated4xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `4xx` metric with no dimension for this canary\.

**withAggregated5xxMetric\(aggregated5xxMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `5xx` metric with no dimension for this canary\.

**withAggregatedFailedCanaryMetric\(aggregatedFailedCanaryMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed` metric with no dimension for this canary\.

**withAggregatedFailedRequestsMetric\(aggregatedFailedRequestsMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed requests` metric with no dimension for this canary\.

**withFailedCanaryMetric\(failedCanaryMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed` metric with the `CanaryName` dimension for this canary\.

**withFailedRequestsMetric\(failedRequestsMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `Failed requests` metric with the `CanaryName` dimension for this canary\.

**withStepDurationMetric\(stepSuccessMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `Duration` metric with the `CanaryName` dimension for this canary\.

**withStepSuccessMetric\(stepSuccessMetric\)**

Accepts a Boolean argument, which specifies whether to emit a `StepSuccess` metric with the `CanaryName` dimension for this canary\.

##### Methods to enable or disable other features<a name="CloudWatch_Synthetics_Library_setConfig_methods"></a>

**withHarFile\(\)**

Accepts a Boolean argument, which specifies whether to create a HAR file for this canary\.

**withStepsReport\(\)**

Accepts a Boolean argument, which specifies whether to report a step execution summary for this canary\.

**withIncludeUrlPassword\(\)**

Accepts a Boolean argument, which specifies whether to include passwords that appear in URLs in logs and reports\.

**withRestrictedUrlParameters\(\)**

Accepts an array of URL path or query parameters to redact\. This applies to URLs appearing in logs, reports, and errors\. You can pass an asterisk \(\*\) as a value to redact all URL path and query parameter values

**withLogRequest\(\)**

Accepts a Boolean argument, which specifies whether to log every request in the canary's logs\.

**withLogResponse\(\)**

Accepts a Boolean argument, which specifies whether to log every response in the canary's logs\.

**withLogRequestBody\(\)**

Accepts a Boolean argument, which specifies whether to log every request body in the canary's logs\.

**withLogResponseBody\(\)**

Accepts a Boolean argument, which specifies whether to log every response body in the canary's logs\.

**withLogRequestHeaders\(\)**

Accepts a Boolean argument, which specifies whether to log every request header in the canary's logs\.

**withLogResponseHeaders\(\)**

Accepts a Boolean argument, which specifies whether to log every response header in the canary's logs\.

**getHarFile\(\)**

Returns whether the canary creates a HAR file\.

**getStepsReport\(\)**

Returns whether the canary reports a step execution summary\.

**getIncludeUrlPassword\(\)**

Returns whether the canary includes passwords that appear in URLs in logs and reports\.

**getRestrictedUrlParameters\(\)**

Returns whether the canary redacts URL path or query parameters\.

**getLogRequest\(\)**

Returns whether the canary logs every request in the canary's logs\.

**getLogResponse\(\)**

Returns whether the canary logs every response in the canary's logs\.

**getLogRequestBody\(\)**

Returns whether the canary logs every request body in the canary's logs\.

**getLogResponseBody\(\)**

Returns whether the canary logs every response body in the canary's logs\.

**getLogRequestHeaders\(\)**

Returns whether the canary logs every request header in the canary's logs\.

**getLogResponseHeaders\(\)**

Returns whether the canary logs every response header in the canary's logs\.

**Functions for all canaries**
+ `withIncludeRequestHeaders`\(includeRequestHeaders\)
+ `withIncludeResponseHeaders`\(includeResponseHeaders\)
+ `withRestrictedHeaders`\(restrictedHeaders\)
+ `withIncludeRequestBody`\(includeRequestBody\)
+ `withIncludeResponseBody`\(includeResponseBody\)
+ `enableReportingOptions`\(\)— Enables all reporting options\-\- **includeRequestHeaders**, **includeResponseHeaders**, **includeRequestBody**, and **includeResponseBody**, \.
+ `disableReportingOptions`\(\)— Disables all reporting options\-\- **includeRequestHeaders**, **includeResponseHeaders**, **includeRequestBody**, and **includeResponseBody**, \.

##### setConfig\(options\) for UI canaries<a name="CloudWatch_Synthetics_Library_setConfigUI"></a>

For UI canaries, **setConfig** can include the following Boolean parameters:
+ `continueOnStepFailure` \(boolean\)— Whether to continue with running the canary script after a step fails \(this refers to the **executeStep** function\)\. If any steps fail, the canary run will still be marked as failed\. The default is `false`\.
+ `harFile` \(boolean\)— Whether to create a HAR file\. The default is `True`\.
+ `screenshotOnStepStart` \(boolean\)— Whether to take a screenshot before starting a step\.
+ `screenshotOnStepSuccess` \(boolean\)— Whether to take a screenshot after completing a successful step\.
+ `screenshotOnStepFailure` \(boolean\)— Whether to take a screenshot after a step fails\.

##### Methods to enable or disable screenshots<a name="CloudWatch_Synthetics_Library_setConfig_screenshots"></a>

**disableStepScreenshots\(\)**

Disables all screenshot options \(screenshotOnStepStart, screenshotOnStepSuccess, and screenshotOnStepFailure\)\.

**enableStepScreenshots\(\)**

Enables all screenshot options \(screenshotOnStepStart, screenshotOnStepSuccess, and screenshotOnStepFailure\)\. By default, all these methods are enabled\.

**getScreenshotOnStepFailure\(\)**

Returns whether the canary takes a screenshot after a step fails\.

**getScreenshotOnStepStart\(\)**

Returns whether the canary takes a screenshot before starting a step\.

**getScreenshotOnStepSuccess\(\)**

Returns whether the canary takes a screenshot after completing a step successfully\.

**withScreenshotOnStepStart\(screenshotOnStepStart\)**

Accepts a Boolean argument, which indicates whether to take a screenshot before starting a step\.

**withScreenshotOnStepSuccess\(screenshotOnStepSuccess\)**

Accepts a Boolean argument, which indicates whether to take a screenshot after completing a step successfully\.

**withScreenshotOnStepFailure\(screenshotOnStepFailure\)**

Accepts a Boolean argument, which indicates whether to take a screenshot after a step fails\.

**Usage in UI canaries**

First, import the synthetics dependency and fetch the configuration\.

```
// Import Synthetics dependency
const synthetics = require('Synthetics');

// Get Synthetics configuration
const synConfig = synthetics.getConfiguration();
```

Then, set the configuration for each option by calling the setConfig method using one of the following options\.

```
// Set configuration values
    synConfig.setConfig({
        screenshotOnStepStart: true, 
        screenshotOnStepSuccess: false,
        screenshotOnStepFailure: false
    });
```

Or

```
synConfig.withScreenshotOnStepStart(false).withScreenshotOnStepSuccess(true).withScreenshotOnStepFailure(true)
```

To disable all screenshots, use the disableStepScreenshots\(\) function as in this example\.

```
synConfig.disableStepScreenshots();
```

You can enable and disable screenshots at any point in the code\. For example, to disable screenshots only for one step, disable them before running that step and then enable them after the step\.

##### setConfig\(options\) for API canaries<a name="CloudWatch_Synthetics_Library_setConfigAPI"></a>

For API canaries, **setConfig** can include the following Boolean parameters:
+ `continueOnHttpStepFailure` \(boolean\)— Whether to continue with running the canary script after an HTTP step fails \(this refers to the **executeHttpStep** function\)\. If any steps fail, the canary run will still be marked as failed\. The default is `true`\.

#### Visual monitoring<a name="CloudWatch_Synthetics_Library_SyntheticsLogger_VisualTesting"></a>

Visual monitoring compares screenshots taken during a canary run with screenshots taken during a baseline canary run\. If the discrepancy between the two screenshots is beyond a threshold percentage, the canary fails and you can see the areas with differences highlighted in color in the canary run report\. Visual monitoring is supported in canaries running **syn\-puppeteer\-node\-3\.2** and later\. It is not currently supported in canaries running Python and Selenium\.

To enable visual monitoring, add the following line of code to the canary script\. For more details, see [SyntheticsConfiguration class](#CloudWatch_Synthetics_Library_SyntheticsConfiguration)\.

```
syntheticsConfiguration.withVisualCompareWithBaseRun(true);
```

The first time that the canary runs successfully after this line is added to the script, it uses the screenshots taken during that run as the baseline for comparison\. After that first canary run, you can use the CloudWatch console to edit the canary to do any of the following:
+ Set the next run of the canary as the new baseline\.
+ Draw boundaries on the current baseline screenshot to designate areas of the screenshot to ignore during visual comparisons\.
+ Remove a screenshot from being used for visual monitoring\.

For more information about using the CloudWatch console to edit a canary, see [Editing or deleting a canary](synthetics_canaries_deletion.md)\.

**Other options for visual monitoring**

**syntheticsConfiguration\.withVisualVarianceThresholdPercentage\(desiredPercentage\)** 

Set the acceptable percentage for screenshot variance in visual comparisons\.

**syntheticsConfiguration\.withVisualVarianceHighlightHexColor\("\#fafa00"\)** 

Set the highlight color that designates variance areas when you look at canary run reports that use visual monitoring\.

**syntheticsConfiguration\.withFailCanaryRunOnVisualVariance\(failCanary\)** 

Set whether or not the canary fails when there is a visual difference that is more than the threshold\. The default is to fail the canary\.

### Synthetics logger<a name="CloudWatch_Synthetics_Library_SyntheticsLogger"></a>

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

### SyntheticsLogHelper class<a name="CloudWatch_Synthetics_Library_SyntheticsLogHelper"></a>

The `SyntheticsLogHelper` class is available in the runtime `syn-nodejs-puppeteer-3.2` and later runtimes\. It is already initialized in the CloudWatch Synthetics library and is configured with Synthetics configuration\. You can add this as a dependency in your script\. This class enables you to sanitize URLs, headers, and error messages to redact sensitive information\.

**Note**  
Synthetics sanitizes all URLs and error messages it logs before including them in logs, reports, HAR files, and canary run errors based on the Synthetics configuration setting `restrictedUrlParameters`\. You have to use `getSanitizedUrl` or `getSanitizedErrorMessage` only if you are logging URLs or errors in your script\. Synthetics does not store any canary artifacts except for canary errors thrown by the script\. Canary run artifacts are stored in your customer account\. For more information, see [Security considerations for Synthetics canaries](servicelens_canaries_security.md)\.

#### getSanitizedUrl\(url, stepConfig = null\)<a name="CloudWatch_Synthetics_Library_getSanitizedUrl"></a>

This function is available in `syn-nodejs-puppeteer-3.2` and later\. It returns sanitized url strings based on the configuration\. You can choose to redact sensitive URL parameters such as password and access\_token by setting the property `restrictedUrlParameters`\. By default, passwords in URLs are redacted\. You can enable URL passwords if needed by setting `includeUrlPassword` to true\. 

This function throws an error if the URL passed in is not a valid URL\.

**Parameters **
+ *url* is a string and is the URL to sanitize\.
+  *stepConfig* \(Optional\) overrides the global Synthetics configuration for this function\. If `stepConfig` is not passed in, the global configuration is used to sanitize the URL\.

**Example **

This example uses the following sample URL: `https://example.com/learn/home?access_token=12345&token_type=Bearer&expires_in=1200`\. In this example, `access_token` contains your sensitive information which shouldn't be logged\. Note that the Synthetics services doesn't store any canary run artifacts\. Artifacts such as logs, screenshots, and reports are all stored in an Amazon S3 bucket in your customer account\.

The first step is to set the Synthetics configuration\.

```
// Import Synthetics dependency
const synthetics = require('Synthetics');

// Import Synthetics logger for logging url
const log = require('SyntheticsLogger');

// Get Synthetics configuration
const synConfig = synthetics.getConfiguration();

// Set restricted parameters
synConfig.setConfig({
   restrictedUrlParameters: ['access_token'];
});
```

Next, sanitize and log the URL

```
// Import SyntheticsLogHelper dependency
const syntheticsLogHelper = require('SyntheticsLogHelper');

const sanitizedUrl = synthetics.getSanitizedUrl('https://example.com/learn/home?access_token=12345&token_type=Bearer&expires_in=1200');
```

This logs the following in your canary log\.

```
My example url is: https://example.com/learn/home?access_token=REDACTED&token_type=Bearer&expires_in=1200
```

You can override the Synthetics configuration for a URL by passing in an optional parameter containing Synthetics configuration options, as in the following example\.

```
const urlConfig = {
   restrictedUrlParameters = ['*']
};
const sanitizedUrl = synthetics.getSanitizedUrl('https://example.com/learn/home?access_token=12345&token_type=Bearer&expires_in=1200', urlConfig);
logger.info('My example url is: ' + sanitizedUrl);
```

The preceding example redacts all query parameters, and is logged as follows:

```
My example url is: https://example.com/learn/home?access_token=REDACTED&token_type=REDACTED&expires_in=REDACTED
```

#### getSanitizedErrorMessage<a name="CloudWatch_Synthetics_Library_getSanitizedErrorMessage"></a>

This function is available in `syn-nodejs-puppeteer-3.2` and later\. It returns sanitized error strings by sanitizing any URLs present based on the Synthetics configuration\. You can choose to override the global Synthetics configuration when you call this function by passing an optional `stepConfig` parameter\. 

**Parameters **
+ *error* is the error to sanitize\. It can be an Error object or a string\.
+  *stepConfig* \(Optional\) overrides the global Synthetics configuration for this function\. If `stepConfig` is not passed in, the global configuration is used to sanitize the URL\.

**Example **

This example uses the following error: `Failed to load url: https://example.com/learn/home?access_token=12345&token_type=Bearer&expires_in=1200`

The first step is to set the Synthetics configuration\.

```
// Import Synthetics dependency
const synthetics = require('Synthetics');

// Import Synthetics logger for logging url
const log = require('SyntheticsLogger');

// Get Synthetics configuration
const synConfig = synthetics.getConfiguration();

// Set restricted parameters
synConfig.setConfig({
   restrictedUrlParameters: ['access_token'];
});
```

Next, sanitize and log the error message

```
// Import SyntheticsLogHelper dependency
const syntheticsLogHelper = require('SyntheticsLogHelper');

try {
   // Your code which can throw an error containing url which your script logs
} catch (error) {
    const sanitizedErrorMessage = synthetics.getSanitizedErrorMessage(errorMessage);
    logger.info(sanitizedErrorMessage);
}
```

This logs the following in your canary log\.

```
Failed to load url: https://example.com/learn/home?access_token=REDACTED&token_type=Bearer&expires_in=1200
```

#### getSanitizedHeaders\(headers, stepConfig=null\)<a name="CloudWatch_Synthetics_Library_getSanitizedHeaders"></a>

This function is available in `syn-nodejs-puppeteer-3.2` and later\. It returns sanitized headers based on the `restrictedHeaders` property of `syntheticsConfiguration`\. The headers specified in the `restrictedHeaders` property are redacted from logs, HAR files, and reports\. 

**Parameters **
+ *headers* is an object containing the headers to sanitize\.
+ *stepConfig* \(Optional\) overrides the global Synthetics configuration for this function\. If `stepConfig` is not passed in, the global configuration is used to sanitize the headers\.

## Node\.js library classes and functions that apply to UI canaries only<a name="CloudWatch_Synthetics_Library_UIcanaries"></a>

The following CloudWatch Synthetics library functions for Node\.js are useful only for UI canaries\.

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

#### async executeStep\(stepName, functionToExecute, \[stepConfig\]\);<a name="CloudWatch_Synthetics_Library_executeStep"></a>

Executes the provided step, wrapping it with start/pass/fail logging, start/pass/fail screenshots, and pass/fail and duration metrics\.

**Note**  
If you are using the `syn-nodejs-2.1` or later runtime, you can configure whether and when screenshots are taken\. For more information, see [SyntheticsConfiguration class](#CloudWatch_Synthetics_Library_SyntheticsConfiguration)\.

The `executeStep` function also does the following:
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

**Updates with syn\-nodejs\-2\.2**

Starting with `syn-nodejs-2.2`, you can optionally pass step configurations to override CloudWatch Synthetics configurations at the step level\. For a list of options that you can pass to `executeStep`, see [SyntheticsConfiguration class](#CloudWatch_Synthetics_Library_SyntheticsConfiguration)\.

The following example overrides the default `false` configuration for `continueOnStepFailure` to `true` and specifies when to take screenthots\.

```
var stepConfig = {
    'continueOnStepFailure': true,
    'screenshotOnStepStart': false,
    'screenshotOnStepSuccess': true,
    'screenshotOnStepFailure': false
}

await executeStep('Navigate to amazon', async function (timeoutInMillis = 30000) {
      await page.goto(url, {waitUntil: ['load', 'networkidle0'], timeout: timeoutInMillis});
 }, stepConfig);
```

#### getDefaultLaunchOptions\(\);<a name="CloudWatch_Synthetics_Library_getDefaultLaunchOptions"></a>

The `getDefaultLaunchOptions()` function returns the browser launch options that are used by CloudWatch Synthetics\. For more information, see [puppeteer\.launch\(\[options\]\)](https://pptr.dev/#?product=Puppeteer&version=v3.3.0&show=api-puppeteerlaunchoptions) 

```
// This function returns default launch options used by Synthetics.
const defaultOptions = await synthetics.getDefaultLaunchOptions();
```

#### getPage\(\);<a name="CloudWatch_Synthetics_Library_getPage"></a>

Returns the current open page as a Puppeteer object\. For more information, see [Puppeteer API v1\.14\.0](https://github.com/puppeteer/puppeteer/blob/v1.14.0/docs/api.md)\.

Example:

```
let page = synthetics.getPage();
```

Response:

The page \(Puppeteer object\) that is currently open in the current browser session\.

#### getRequestResponseLogHelper\(\);<a name="CloudWatch_Synthetics_Library_getRequestResponseLogHelper"></a>

**Important**  
In canaries that use the `syn-nodejs-puppeteer-3.2` runtime or later, this function is deprecated along with the `RequestResponseLogHelper` class\. Any use of this function causes a warning to appear in your canary logs\. This function will be removed in future runtime versions\. If you are using this function, use [RequestResponseLogHelper class](#CloudWatch_Synthetics_Library_RequestResponseLogHelper) instead\. 

Use this function as a builder pattern for tweaking the request and response logging flags\.

Example:

```
synthetics.setRequestResponseLogHelper(getRequestResponseLogHelper().withLogRequestHeaders(false));;
```

Response:

```
{RequestResponseLogHelper}
```

#### launch\(options\)<a name="CloudWatch_Synthetics_Library_LaunchOptions"></a>

The options for this function are available only in the `syn-nodejs-2.1` runtime version or later\.

This function is used only for UI canaries\. It closes the existing browser and launches a new one\.

**Note**  
CloudWatch Synthetics always launches a browser before starting to run your script\. You don't need to call launch\(\) unless you want to launch a new browser with custom options\.

\(options\) is a configurable set of options to set on the browser\. For more information, see [puppeteer\.launch\(\[options\]\)](https://github.com/puppeteer/puppeteer/blob/main/docs/api.md#puppeteerlaunchoptions)\.

If you call this function with no options, Synthetics launches a browser with default arguments, `executablePath`, and `defaultViewport`\. The default viewport in CloudWatch Synthetics is 1920 by 1080\.

You can override launch parameters used by CloudWatch Synthetics and pass additional parameters when launching the browser\. For example, the following code snippet launches a browser with default arguments and a default executable path, but with a viewport of 800 x 600\.

```
await synthetics.launch({
        defaultViewport: { 
            "deviceScaleFactor": 1, 
            "width": 800,
            "height": 600 
    }});
```

The following sample code adds a new `ignoreHTTPSErrors` parameter to the CloudWatch Synthetics launch parameters:

```
await synthetics.launch({
        ignoreHTTPSErrors: true
 });
```

You can disable web security by adding a `--disable-web-security` flag to args in the CloudWatch Synthetics launch parameters:

```
// This function adds the --disable-web-security flag to the launch parameters
const defaultOptions = await synthetics.getDefaultLaunchOptions();
const launchArgs = [...defaultOptions.args, '--disable-web-security'];
await synthetics.launch({
     args: launchArgs
  });
```

#### RequestResponseLogHelper class<a name="CloudWatch_Synthetics_Library_RequestResponseLogHelper"></a>

**Important**  
In canaries that use the `syn-nodejs-puppeteer-3.2` runtime or later, this class is deprecated\. Any use of this class causes a warning to appear in your canary logs\. This function will be removed in future runtime versions\. If you are using this function, use [RequestResponseLogHelper class](#CloudWatch_Synthetics_Library_RequestResponseLogHelper) instead\.

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

**Important**  
In canaries that use the `syn-nodejs-puppeteer-3.2` runtime or later, this function is deprecated along with the `RequestResponseLogHelper` class\. Any use of this function causes a warning to appear in your canary logs\. This function will be removed in future runtime versions\. If you are using this function, use [RequestResponseLogHelper class](#CloudWatch_Synthetics_Library_RequestResponseLogHelper) instead\. 

Use this function as a builder pattern for setting the request and response logging flags\.

Example:

```
synthetics.setRequestResponseLogHelper().withLogRequestHeaders(true).withLogResponseHeaders(true);
```

Response:

```
{RequestResponseLogHelper}
```

#### async takeScreenshot\(name, suffix\);<a name="CloudWatch_Synthetics_Library_takeScreenshot"></a>

Takes a screenshot \(\.PNG\) of the current page with name and suffix \(optional\)\.

Example:

```
await synthetics.takeScreenshot("navigateToUrl", "loaded")
```

This example captures and uploads a screenshot named `01-navigateToUrl-loaded.png` to the canary's S3 bucket\.

You can take a screenshot for a particular canary step by passing the `stepName` as the first parameter\. Screenshots are linked to the canary step in your reports, to help you track each step while debugging\.

CloudWatch Synthetics canaries automatically take screeenshots before starting a step \(the `executeStep` function\) and after the step completion \(unless you configure the canary to disable screenshots\)\. You can take more screenshots by passing in the step name in the `takeScreesnot` function\.

The following example takes screenshot with the `signupForm` as the value of the `stepName`\. The screenshot will be named `02-signupForm-address` and will be linked to the step named `signupForm` in the canary report\.

```
await synthetics.takeScreenshot('signupForm', 'address')
```

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

const syntheticsLink = new SyntheticsLink("https://www.amazon.com");
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

## Node\.js library classes and functions that apply to API canaries only<a name="CloudWatch_Synthetics_Library_APIcanaries"></a>

The following CloudWatch Synthetics library functions for Node\.js are useful only for API canaries\.

**Topics**
+ [executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)](#CloudWatch_Synthetics_Library_executeHttpStep)

### executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)<a name="CloudWatch_Synthetics_Library_executeHttpStep"></a>

Executes the provided HTTP request as a step, and publishes `SuccessPercent` \(pass/fail\) and `Duration` metrics\.

**executeHttpStep** uses either HTTP or HTTPS native functions under the hood, depending upon the protocol specified in the request\.

This function also adds a step execution summary to the canary's report\. The summary includes details about each HTTP request, such as the following:
+ Start time
+ End time
+ Status \(PASSED/FAILED\)
+ Failure reason, if it failed
+ HTTP call details such as request/response headers, body, status code, status message, and performance timings\. 

#### Parameters<a name="CloudWatch_Synthetics_Library_executeHttpStep_parameters"></a>

**stepName\(*String*\)**

Specifies the name of the step\. This name is also used for publishing CloudWatch metrics for this step\.

**requestOptions\(*Object or String*\)**

The value of this parameter can be a URL, a URL string, or an object\. If it is an object, then it must be a set of configurable options to make an HTTP request\. It supports all options in [ http\.request\(options\[, callback\]\)](https://nodejs.org/api/http.html#http_http_request_options_callback) in the Node\.js documentation\.

In addition to these Node\.js options, **requestOptions** supports the additional parameter `body`\. You can use the `body` parameter to pass data as a request body\.

**callback\(*response*\)**

\(Optional\) This is a user function which is invoked with the HTTP response\. The response is of the type [ Class: http\.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage)\.

**stepConfig\(*object*\)**

\(Optional\) Use this parameter to override global synthetics configurations with a different configuration for this step\.

#### Examples of using executeHttpStep<a name="CloudWatch_Synthetics_Library_executeHttpStep_examples"></a>

The following series of examples build on each other to illustrate the various uses of this option\.

This first example configures request parameters\. You can pass a URL as **requestOptions**:

```
let requestOptions = 'https://www.amazon.com';
```

Or you can pass a set of options:

```
let requestOptions = {
        'hostname': 'myproductsEndpoint.com',
        'method': 'GET',
        'path': '/test/product/validProductName',
        'port': 443,
        'protocol': 'https:'
    };
```

The next example creates a callback function which accepts a response\. By default, if you do not specify **callback**, CloudWatch Synthetics validates that the status is between 200 and 299 inclusive\.

```
// Handle validation for positive scenario
    const callback = async function(res) {
        return new Promise((resolve, reject) => {
            if (res.statusCode < 200 || res.statusCode > 299) {
                throw res.statusCode + ' ' + res.statusMessage;
            }
     
            let responseBody = '';
            res.on('data', (d) => {
                responseBody += d;
            });
     
            res.on('end', () => {
                // Add validation on 'responseBody' here if required. For ex, your status code is 200 but data might be empty
                resolve();
            });
        });
    };
```

The next example creates a configuration for this step that overrides the global CloudWatch Synthetics configuration\. The step configuration in this example allows request headers, response headers, request body \(post data\), and response body in your report and restrict 'X\-Amz\-Security\-Token' and 'Authorization' header values\. By default, these values are not included in the report for security reasons\. If you choose to include them, the data is only stored in your S3 bucket\.

```
// By default headers, post data, and response body are not included in the report for security reasons. 
// Change the configuration at global level or add as step configuration for individual steps
let stepConfig = {
    includeRequestHeaders: true, 
    includeResponseHeaders: true,
    restrictedHeaders: ['X-Amz-Security-Token', 'Authorization'], // Restricted header values do not appear in report generated.
    includeRequestBody: true,
    includeResponseBody: true
};
```

This final example passes your request to **executeHttpStep** and names the step\.

```
await synthetics.executeHttpStep('Verify GET products API', requestOptions, callback, stepConfig);
```

With this set of examples, CloudWatch Synthetics adds the details from each step in your report and produces metrics for each step using **stepName**\.

 You will see `successPercent` and `duration` metrics for the `Verify GET products API` step\. You can monitor your API performance by monitoring the metrics for your API call steps\. 

For a sample complete script that uses these functions, see [Multi\-step API canary](CloudWatch_Synthetics_Canaries_Samples.md#CloudWatch_Synthetics_Canaries_Samples_APIsteps)\.