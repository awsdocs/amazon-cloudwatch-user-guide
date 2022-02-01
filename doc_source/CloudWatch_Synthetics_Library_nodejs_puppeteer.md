# Runtime versions using Node\.js and Puppeteer<a name="CloudWatch_Synthetics_Library_nodejs_puppeteer"></a>

The first runtime version for Node\.js and Puppeteer was named `syn-1.0`\. Later runtime versions have the naming convention `syn-language-majorversion.minorversion`\. Starting with `syn-nodejs-puppeteer-3.0`, the naming convention is `syn-language-framework-majorversion.minorversion` 

An additional `-beta` suffix shows that the runtime version is currently in a beta preview release\.

Runtime versions with the same major version number are backward compatible\. 

**Notes for all runtime versions**

When using `syn-nodejs-puppeteer-3.0` runtime version, make sure that your canary script is compatible with Node\.js 12\.x\. If you use an earlier version of a `syn-nodejs` runtime version, make sure that that your script is compatible with Node\.js 10\.x\.

The Lambda code in a canary is configured to have a maximum memory of 1 GB\. Each run of a canary times out after a configured timeout value\. If no timeout value is specified for a canary, CloudWatch chooses a timeout value based on the canary's frequency\. If you configure a timeout value, make it no shorter than 15 seconds to allow for Lambda cold starts and the time it takes to boot up the canary instrumentation\.

## syn\-nodejs\-puppeteer\-3\.3<a name="CloudWatch_Synthetics_runtimeversion-nodejs-puppeteer-3.3"></a>

The `syn-nodejs-puppeteer-3.3` runtime is the newest runtime version\.

**Major dependencies**:
+ Lambda runtime Node\.js 12\.x
+ Puppeteer\-core version 5\.5\.0
+ Chromium version 88\.0\.4298\.0

**New features in syn\-nodejs\-puppeteer\-3\.3**:
+ **More options for artifact encryption**— For canaries using this runtime or later, instead of using an AWS managed key to encrypt artifacts that the canary stores in Amazon S3, you can choose to use an AWS KMS customer managed key or an Amazon S3\-managed key\. For more information, see [Encrypting canary artifacts](CloudWatch_Synthetics_artifact_encryption.md)\. 

## syn\-nodejs\-puppeteer\-3\.2<a name="CloudWatch_Synthetics_runtimeversion-nodejs-puppeteer-3.2"></a>

**Major dependencies**:
+ Lambda runtime Node\.js 12\.x
+ Puppeteer\-core version 5\.5\.0
+ Chromium version 88\.0\.4298\.0

**New features in syn\-nodejs\-puppeteer\-3\.2**:
+ **visual monitoring with screenshots**— Canaries using this runtime or later can compare a screenshot taken during a run with a baseline version of the same screenshot\. If the screenshots are more different than a specified percentage threshold, the canary fails\. For more information, see [Visual monitoring](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_SyntheticsLogger_VisualTesting) or [Visual monitoring blueprint](CloudWatch_Synthetics_Canaries_Blueprints.md#CloudWatch_Synthetics_Canaries_Blueprints_VisualTesting)\. 
+ **New functions regarding sensitive data** You can prevent sensitive data from appearing in canary logs and reports\. For more information, see [SyntheticsLogHelper class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_SyntheticsLogHelper)\.
+ **Deprecated function** The `RequestResponseLogHelper` class is deprecated in favor of other new configuration options\. For more information, see [RequestResponseLogHelper class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_RequestResponseLogHelper)\.

## syn\-nodejs\-puppeteer\-3\.1<a name="CloudWatch_Synthetics_runtimeversion-nodejs-puppeteer-3.1"></a>

**Major dependencies**:
+ Lambda runtime Node\.js 12\.x
+ Puppeteer\-core version 5\.5\.0
+ Chromium version 88\.0\.4298\.0

**New features in syn\-nodejs\-puppeteer\-3\.1**:
+ **Ability to configure CloudWatch metrics**— With this runtime, you can disable the metrics that you do not require\. Otherwise, canaries publish various CloudWatch metrics for each canary run\.
+ **Screenshot linking**— You can link a screenshot to a canary step after the step has completed\. To do this, you take the screenshot by using the **takeScreenshot** method, using the name of the step that you want to associate the screenshot with\. For example, you might want to perform a step, add a wait time, and then take the screenshot\.
+ **Heartbeat monitor blueprint can monitor multiple URLs**— You can use the heartbeat monitoring blueprint in the CloudWatch console to monitor multiple URLs and see the status, duration, associated screenshots, and failure reason for each URL in the step summary of the canary run report\.

## syn\-nodejs\-puppeteer\-3\.0<a name="CloudWatch_Synthetics_runtimeversion-nodejs-puppeteer-3.0"></a>

**Major dependencies**:
+ Lambda runtime Node\.js 12\.x
+ Puppeteer\-core version 5\.5\.0
+ Chromium version 88\.0\.4298\.0

**New features in syn\-nodejs\-puppeteer\-3\.0**:
+ **Upgraded dependencies**— This version uses Puppeteer version 5\.5\.0, Node\.js 12\.x, and Chromium 88\.0\.4298\.0\.
+ **Cross\-Region bucket access**— You can now specify an S3 bucket in another Region as the bucket where your canary stores its log files, screenshots, and HAR files\.
+ **New functions available**— This version adds library functions to retrieve the canary name and the Synthetics runtime version\.

  For more information, see [Synthetics class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_Synthetics_Class_all)\.

## syn\-nodejs\-2\.2<a name="CloudWatch_Synthetics_runtimeversion-2.2"></a>

This section contains information about the `syn-nodejs-2.2` runtime version\.

**Important**  
This runtime version is scheduled to be deprecated on May 28, 2021\. For more information, see [CloudWatch Synthetics runtime support policy](CloudWatch_Synthetics_Canaries_Library.md#CloudWatch_Synthetics_Canaries_runtime_support)\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 3\.3\.0
+ Chromium version 83\.0\.4103\.0

**New features in syn\-nodejs\-2\.2**:
+ **Monitor your canaries as HTTP steps**— You can now test multiple APIs in a single canary\. Each API is tested as a separate HTTP step, and CloudWatch Synthetics monitors the status of each step using step metrics and the CloudWatch Synthetics step report\. CloudWatch Synthetics creates `SuccessPercent` and `Duration` metrics for each HTTP step\.

  This functionality is implemented by the **executeHttpStep\(stepName, requestOptions, callback, stepConfig\)** function\. For more information, see [executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_executeHttpStep)\.

  The API canary blueprint is updated to use this new feature\.
+ **HTTP request reporting**— You can now view detailed HTTP requests reports which capture details such as request/response headers, response body, status code, error and performance timings, TCP connection time, TLS handshake time, first byte time, and content transfer time\. All HTTP requests which use the HTTP/HTTPS module under the hood are captured here\. Headers and response body are not captured by default but can be enabled by setting configuration options\.
+ **Global and step\-level configuration**— You can set CloudWatch Synthetics configurations at the global level, which are applied to all steps of canaries\. You can also override these configurations at the step level by passing configuration key/value pairs to enable or disable certain options\.

  For more information, see [SyntheticsConfiguration class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_SyntheticsConfiguration)\.
+ **Continue on step failure configuration**— You can choose to continue canary execution when a step fails\. For the `executeHttpStep` function, this is turned on by default\. You can set this option once at global level or set it differently per\-step\. 

## syn\-nodejs\-2\.1<a name="CloudWatch_Synthetics_runtimeversion-2.1"></a>

**Important**  
This runtime version is scheduled to be deprecated on May 28, 2021\. For more information, see [CloudWatch Synthetics runtime support policy](CloudWatch_Synthetics_Canaries_Library.md#CloudWatch_Synthetics_Canaries_runtime_support)\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 3\.3\.0
+ Chromium version 83\.0\.4103\.0

**New features in syn\-nodejs\-2\.1**:
+ **Configurable screenshot behavior**— Provides the ability to turn off the capturing of screenshots by UI canaries\. In canaries that use previous versions of the runtimes, UI canaries always capture screenshots before and after each step\. With `syn-nodejs-2.1`, this is configurable\. Turning off screenshots can reduce your Amazon S3 storage costs, and can help you comply with HIPAA regulations\. For more information, see [SyntheticsConfiguration class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_SyntheticsConfiguration)\.
+ **Customize the Google Chrome launch parameters** You can now configure the arguments used when a canary launches a Google Chrome browser window\. For more information, see [launch\(options\)](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_LaunchOptions)\.

There can be a small increase in canary duration when using syn\-nodejs\-2\.0 or later, compared to earlier versions of the canary runtimes\.

## syn\-nodejs\-2\.0<a name="CloudWatch_Synthetics_runtimeversion-2.0"></a>

**Important**  
This runtime version is scheduled to be deprecated on May 28, 2021\. For more information, see [CloudWatch Synthetics runtime support policy](CloudWatch_Synthetics_Canaries_Library.md#CloudWatch_Synthetics_Canaries_runtime_support)\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 3\.3\.0
+ Chromium version 83\.0\.4103\.0

**New features in syn\-nodejs\-2\.0**:
+ **Upgraded dependencies**— This runtime version uses Puppeteer\-core version 3\.3\.0 and Chromium version 83\.0\.4103\.0
+ **Support for X\-Ray active tracing\.** When a canary has tracing enabled, X\-Ray traces are sent for all calls made by the canary that use the browser, the AWS SDK, or HTTP or HTTPS modules\. Canaries with tracing enabled appear on the service map in both CloudWatch ServiceLens and in X\-Ray, even when they don't send requests to other services or applications that have tracing enabled\. For more information, see [Canaries and X\-Ray tracing](CloudWatch_Synthetics_Canaries_tracing.md)\.
+ **Synthetics reporting**— For each canary run, CloudWatch Synthetics creates a report named `SyntheticsReport-PASSED.json` or `SyntheticsReport-FAILED.json` which records data such as start time, end time, status, and failures\. It also records the PASSED/FAILED status of each step of the canary script, and failures and screenshots captured for each step\.
+ **Broken link checker report**— The new version of the broken link checker included in this runtime creates a report that includes the links that were checked, status code, failure reason \(if any\), and source and destination page screenshots\.
+ **New CloudWatch metrics**— Synthetics publishes metrics named `2xx`, `4xx`, `5xx`, and `RequestFailed` in the `CloudWatchSynthetics` namespace\. These metrics show the number of 200s, 400s, 500s, and request failures in the canary runs\. With this runtime version, these metrics are reported only for UI canaries, and are not reported for API canaries\. They are also reported for API canaries starting with runtime version `syn-nodejs-puppeteeer-2.2`\.
+ **Sortable HAR files**— You can now sort your HAR files by status code, request size, and duration\.
+ **Metrics timestamp**— CloudWatch metrics are now reported based on the Lambda invocation time instead of the canary run end time\.

**Bug fixes in syn\-nodejs\-2\.0**:
+ Fixed the issue of canary artifact upload errors not being reported\. Such errors are now surfaced as execution errors\.
+ Fixed the issue of redirected requests \(3xx\) being incorrectly logged as errors\.
+ Fixed the issue of screenshots being numbered starting from 0\. They should now start with 1\.
+ Fixed the issue of screenshots being garbled for Chinese and Japanese fonts\.

There can be a small increase in canary duration when using syn\-nodejs\-2\.0 or later, compared to earlier versions of the canary runtimes\.

## syn\-nodejs\-2\.0\-beta<a name="CloudWatch_Synthetics_runtimeversion-2.0-beta"></a>

**Important**  
This runtime version was deprecated on February 8, 2021\. For more information, see [CloudWatch Synthetics runtime support policy](CloudWatch_Synthetics_Canaries_Library.md#CloudWatch_Synthetics_Canaries_runtime_support)\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 3\.3\.0
+ Chromium version 83\.0\.4103\.0

**New features in syn\-nodejs\-2\.0\-beta**:
+ **Upgraded dependencies**— This runtime version uses Puppeteer\-core version 3\.3\.0 and Chromium version 83\.0\.4103\.0
+ **Synthetics reporting**— For each canary run, CloudWatch Synthetics creates a report named `SyntheticsReport-PASSED.json` or `SyntheticsReport-FAILED.json` which records data such as start time, end time, status, and failures\. It also records the PASSED/FAILED status of each step of the canary script, and failures and screenshots captured for each step\.
+ **Broken link checker report**— The new version of the broken link checker included in this runtime creates a report that includes the links that were checked, status code, failure reason \(if any\), and source and destination page screenshots\.
+ **New CloudWatch metrics**— Synthetics publishes metrics named `2xx`, `4xx`, `5xx`, and `RequestFailed` in the `CloudWatchSynthetics` namespace\. These metrics show the number of 200s, 400s, 500s, and request failures in the canary runs\. These metrics are reported only for UI canaries, and are not reported for API canaries\.
+ **Sortable HAR files**— You can now sort your HAR files by status code, request size, and duration\.
+ **Metrics timestamp**— CloudWatch metrics are now reported based on the Lambda invocation time instead of the canary run end time\.

**Bug fixes in syn\-nodejs\-2\.0\-beta**:
+ Fixed the issue of canary artifact upload errors not being reported\. Such errors are now surfaced as execution errors\.
+ Fixed the issue of redirected requests \(3xx\) being incorrectly logged as errors\.
+ Fixed the issue of screenshots being numbered starting from 0\. They should now start with 1\.
+ Fixed the issue of screenshots being garbled for Chinese and Japanese fonts\.

## syn\-1\.0<a name="CloudWatch_Synthetics_runtimeversion-1.0"></a>

**Important**  
This runtime version is scheduled to be deprecated on May 28, 2021\. For more information, see [CloudWatch Synthetics runtime support policy](CloudWatch_Synthetics_Canaries_Library.md#CloudWatch_Synthetics_Canaries_runtime_support)\.

The first Synthetics runtime version is `syn-1.0`\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 1\.14\.0
+ The Chromium version that matches Puppeteer\-core 1\.14\.0