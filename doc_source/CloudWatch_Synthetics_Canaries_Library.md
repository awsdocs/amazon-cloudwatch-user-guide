# Canary Runtime Versions<a name="CloudWatch_Synthetics_Canaries_Library"></a>

When you create or update a canary, you choose a Synthetics runtime version for the canary\. A Synthetics runtime is a combination of the Synthetics code that calls your script handler, and the Lambda layers of bundled dependencies\.

We recommend that you always use the most recent runtime version for your canaries, to be able to use the latest features and updates made to the Synthetics library\.

Synthetics runtime versions are labeled `syn-majorversion .minorversion`\. Runtime versions with the same major version number are backward compatible\.

The current Synthetics runtime version is `syn-1.0`, which includes the following:
+ Synthetics library 1\.0
+ Synthetics handler code 1\.0
+ Lambda runtime Node\.js 10\.x
+ Puppeteer\-core version 1\.14\.0
+ The Chromium version that matches Puppeteer\-core 1\.14\.0

Be sure that your canary scripts are compatible with Node\.js 10\.x\.

The Lambda code in a canary is configured to have a maximum memory of 1 GB\. Each run of a canary times out after a configured timeout value\. If no timeout value is specified for a canary, CloudWatch chooses a timeout value based on the canary's frequency\.

## Canary Runtime Support Policy<a name="CloudWatch_Synthetics_Canaries_runtime_support"></a>

Synthetics runtime versions are subject to maintenance and security updates\. When any component of a runtime version is no longer supported for security updates, that Synthetics runtime version is deprecated\.

You can't create canaries using deprecated runtime versions\. Canaries that use deprecated runtimes continue to run\. You can stop, start, and delete these canaries\. You can update an existing canary that uses a deprecated runtime versions by updating the canary to use a supported runtime version\.