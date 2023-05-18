# Security considerations for Synthetics canaries<a name="servicelens_canaries_security"></a>

The following sections explain security issues that you should consider when creating and running canaries in Synthetics\.

## Use secure connections<a name="servicelens_canaries_connections"></a>

Because canary code and the results from canary test runs can contain sensitive information, do not have your canary connect to endpoints over unencrypted connections\. Always use encrypted connections, such as those that begin with `https://`\.

## Canary naming considerations<a name="servicelens_canaries_security_canary_arn_name"></a>

The Amazon Resource Name \(ARN\) of a canary is included in the user\-agent header as a part of outbound calls made from the Puppeteer\-driven Chromium browser that is included as a part of the CloudWatch Synthetics wrapper library\. This helps identify CloudWatch Synthetics canary traffic and relate it back to the canaries that are making calls\. 

The canary ARN includes the canary name\. Choose canary names that do not reveal proprietary information\.

Additionally, be sure to point your canaries only at websites and endpoints that you control\.

## Secrets and sensitive information in canary code<a name="servicelens_canaries_secrets"></a>

If you pass your canary code directly into the canary using a zip file, the contents of the script can be seen in AWS CloudTrail logs\.

If you have sensitive information or secrets \(such as access keys or database credentials\) in a canary script, we strongly recommend that you store the script as a versioned object in Amazon S3 and pass the Amazon S3 location into the canary, instead of passing the canary code by a zip file\.

If you do use a zip file to pass the canary script, we strongly recommend that you don't include secrets or sensitive information in your canary source code\. For more information about how to use AWS Secrets Manager to help keep your secrets safe, see [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)\.

## Permissions considerations<a name="servicelens_canaries_security_results"></a>

We recommend that you restrict access to resources that are created or used by CloudWatch Synthetics\. Use tight permissions on the Amazon S3 buckets where canaries store test run results and other artifacts, such as logs and screenshots\.

Similarly, keep tight permissions on the locations where your canary source code is stored, so that no user accidentally or maliciously deletes the Lambda layers or Lambda functions used for the canary\.

To help make sure you run the canary code you intend, you can use object versioning on the Amazon S3 bucket where your canary code is stored\. Then when you specify this code to run as a canary, you can include the object `versionId` as part of the path, as in the following examples\.

```
https://bucket.s3.amazonaws.com/path/object.zip?versionId=version-id
https://s3.amazonaws.com/bucket/path/object.zip?versionId=version-id
https://bucket.s3-region.amazonaws.com/path/object.zip?versionId=version-id
```

## Stack traces and exception messages<a name="servicelens_canaries_security_stack_traces"></a>

By default, CloudWatch Synthetics canaries capture any exception thrown by your canary script, no matter whether the script is custom or is from a blueprint\. CloudWatch Synthetics logs both the exception message and the stack trace to three locations:
+ Back into the CloudWatch Synthetics service to speed up debugging when you describe test runs
+ Into CloudWatch Logs according to the configuration that your Lambda functions are created with
+ Into the Synthetics log file, which is a plaintext file that is uploaded to the Amazon S3 location specified by the value you set for the `resultsLocation` of the canary

If you want to send and store less information, you can capture exceptions before they return to the CloudWatch Synthetics wrapper library\.

You can also have request URLs in your errors\. CloudWatch Synthetics scans for any URLs in the error thrown by your script and redacts restricted URL parameters from them based on the **restrictedUrlParameters** configuration\. If you are logging error messages in your script, you can use [getSanitizedErrorMessage ](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_getSanitizedErrorMessage) to redact URLs before logging\. 

## Scope your IAM roles narrowly<a name="servicelens_canaries_security_canary_iam_scope"></a>

We recommend that you do not configure your canary to visit potentially malicious URLs or endpoints\. Pointing your Canary to untrusted or unknown websites or endpoints could expose your Lambda function code to malicious user’s scripts\. Assuming a malicious website can break out of Chromium, it could have access to your Lambda code in a similar way to if you connected to it using an internet browser\. 

Run your Lambda function with an IAM execution role that has scoped\-down permissions\. This way, if your Lambda function is compromised by a malicious script, it is limited in the actions it can take when running as your canary’s AWS account\.

When you use the CloudWatch console to create a canary, it is created with a scoped\-down IAM execution role\.

## Sensitive data redaction<a name="servicelens_canaries_security_logging"></a>

CloudWatch Synthetics captures URLs, status code, failure reason \(if any\), and headers and bodies of requests and responses\. This enables a canary user to understand, monitor, and debug canaries\. 

 The configurations described in the following sections can be set at any point in canary execution\. You can also choose to apply different configurations to different synthetics steps\. 

### Request URLs<a name="servicelens_canaries_security_logging_url"></a>

By default, CloudWatch Synthetics logs request URLs, status codes, and the status reason for each URL in canary logs\. Request URLs can also appear in canary execution reports, HAR files, and so on\. Your request URL might contain sensitive query parameters, such as access tokens or passwords\. You can redact sensitive information from being logged by CloudWatch Synthetics\.

To redact sensitive information, set the configuration property **restrictedUrlParameters**\. For more information, see [SyntheticsConfiguration class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_SyntheticsConfiguration)\. This causes CloudWatch Synthetics to redact URL parameters, including path and query parameter values, based on **restrictedUrlParameters** before logging\. If you are logging URLs in your script, you can use [getSanitizedUrl\(url, stepConfig = null\)](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_getSanitizedUrl) to redact URLs before logging\. For more information, see [SyntheticsLogHelper class](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_SyntheticsLogHelper)\. 

### Headers<a name="servicelens_canaries_security_logging_headers"></a>

By default, CloudWatch Synthetics doesn't log request/response headers\. For UI canaries, this is the default behavior for canaries using runtime version `syn-nodejs-puppeteer-3.2` and later\.

 If your headers don't contain sensitive information, you can enable headers in HAR file and HTTP reports by setting the **includeRequestHeaders** and **includeResponseHeaders** properties to `true`\. You can enable all headers but choose to restrict values of sensitive header keys\. For example, you can choose to only redact `Authorization` headers from artifacts produced by canaries\. 

### Request and response body<a name="servicelens_canaries_security_logging_body"></a>

By default, CloudWatch Synthetics doesn't log the request/response body in canary logs or reports\. This information is particularly useful for API canaries\. Synthetics captures all HTTP requests and can show headers, request and response bodies\. For more information, see [executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_executeHttpStep)\. You can choose to enable request/response body by setting the **includeRequestBody** and **includeResponseBody** properties to `true`\. 