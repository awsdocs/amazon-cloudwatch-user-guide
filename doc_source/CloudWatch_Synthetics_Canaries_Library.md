# Canary Runtime Versions<a name="CloudWatch_Synthetics_Canaries_Library"></a>

When you create or update a canary, you choose a Synthetics runtime version for the canary\. A Synthetics runtime is a combination of the Synthetics code that calls your script handler, and the Lambda layers of bundled dependencies\.

We recommend that you always use the most recent runtime version for your canaries, to be able to use the latest features and updates made to the Synthetics library\.

Synthetics runtime versions are labeled `syn-language-majorversion.minorversion`\. The exception to this naming convention is the first runtime version, which is named `syn-1.0`\.

An additional `-beta` suffix shows that the runtime version is currently in a beta preview release\.

Currently, Node\.js is the only supported language\. Runtime versions with the same major version number are backward compatible\. 

**Notes for all runtime versions**

When using either of the current runtime versions, be sure that your canary scripts are compatible with Node\.js 10\.x\.

The Lambda code in a canary is configured to have a maximum memory of 1 GB\. Each run of a canary times out after a configured timeout value\. If no timeout value is specified for a canary, CloudWatch chooses a timeout value based on the canary's frequency\.

## syn\-nodejs\-2\.0\-beta<a name="CloudWatch_Synthetics_runtimeversion-2.0"></a>

The `syn-nodejs-2.0-beta` is the newest runtime version\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 3\.3\.0
+ Chromium version 81\.0\.4044\.0

**New features in syn\-nodejs\-2\.0\-beta**:
+ **Upgraded dependencies**— This runtime version uses Puppeteer\-core version 3\.3\.0 and Chromium version 81\.0\.4044\.0
+ **Synthetics reporting**— For each canary run, CloudWatch Synthetics creates a report named `SyntheticsReport-PASSED.json` or `SyntheticsReport-FAILED.json` which records data such as start time, end time, status, and failures\. It also records the PASSED/FAILED status of each step of the canary script, and failures and screenshots captured for each step\.
+ **Broken link checker report**— The new version of the broken link checker included in this runtime creates a report that includes the links that were checked, status code, failure reason \(if any\), and source and destination page screenshots\.
+ **New CloudWatch metrics**— Synthetics publishes metrics named `2xx`, `4xx`, `5xx`, and `RequestFailed` in the `CloudWatchSynthetics` namespace\. These metrics show the number of 200s, 400s, 500s, and request failures in the canary runs\.
+ **Sortable HAR files**— You can now sort your HAR files by status code, request size, and duration\.
+ **Metrics timestamp**— CloudWatch metrics are now reported based on the Lambda invocation time instead of the canary run end time\.

**Bug fixes in syn\-nodejs\-2\.0\-beta**:
+ Fixed the issue of canary artifact upload errors not being reported\. Such errors are now surfaced as execution errors\.
+ Fixed the issue of redirected requests \(3xx\) being incorrectly logged as errors\.
+ Fixed the issue of screenshots being numbered starting from 0\. They should now start with 1\.
+ Fixed the issue of screenshots being garbled for Chinese and Japanese fonts\.

## syn\-1\.0<a name="CloudWatch_Synthetics_runtimeversion-1.0"></a>

The first Synthetics runtime version is `syn-1.0`\.

**Major dependencies**:
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 1\.14\.0
+ The Chromium version that matches Puppeteer\-core 1\.14\.0

## Canary Runtime Support Policy<a name="CloudWatch_Synthetics_Canaries_runtime_support"></a>

Synthetics runtime versions are subject to maintenance and security updates\. When any component of a runtime version is no longer supported for security updates, that Synthetics runtime version is deprecated\.

You can't create canaries using deprecated runtime versions\. Canaries that use deprecated runtimes continue to run\. You can stop, start, and delete these canaries\. You can update an existing canary that uses deprecated runtime versions by updating the canary to use a supported runtime version\.