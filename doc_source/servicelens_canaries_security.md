# Security Considerations for Synthetics Canaries<a name="servicelens_canaries_security"></a>


****  

|  | 
| --- |
| CloudWatch Synthetics is in open preview\. The preview is currently available in the following Regions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/servicelens_canaries_security.html) The preview is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [cw\-synthetics@amazon\.com](mailto:cw-synthetics@amazon.com) | 

The following sections explain security issues that you should consider when creating and running canaries in Synthetics\.

## Use Secure Connections<a name="servicelens_canaries_connections"></a>

Because canary code and the results from canary test runs can contain sensitive information, do not have your canary connect to endpoints over unencrypted connections\. Always use encrypted connections, such as those that begin with `https://`\.

## Canary Naming Considerations<a name="servicelens_canaries_security_canary_arn_name"></a>

The Amazon Resource Name \(ARN\) of a canary is included in the user\-agent header as a part of outbound calls made from the Puppeteer\-driven Chromium browser that is included as a part of the CloudWatch Synthetics wrapper library\. This helps identify CloudWatch Synthetics canary traffic and relate it back to the canaries that are making calls\. 

The canary ARN includes the canary name\. Choose canary names that do not reveal proprietary information\.

Additionally, be sure to point your canaries only at websites and endpoints that you control\.

## Secrets in Canary Code<a name="servicelens_canaries_secrets"></a>

We recommend that you don't include secrets, such as access keys or database credentials, in your canary source code\. For more information about how to use AWS Secrets Manager to help keep your secrets safe, see [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)\.

## Permissions Considerations<a name="servicelens_canaries_security_results"></a>

We recommend that you restrict access to resources that are created or used by CloudWatch Synthetics\. Use tight permissions on the Amazon S3 buckets where canaries store test run results and other artifacts, such as logs and screenshots\.

Similarly, keep tight permissions on the locations where your canary source code is stored, so that no user accidentally or maliciously deletes the Lambda layers or Lambda functions used for the canary\.

To help make sure you run the canary code you intend, you can use object versioning on the Amazon S3 bucket where your canary code is stored\. Then when you specify this code to run as a canary, you can include the object `versionId` as part of the path, as in the following examples\.

```
https://bucket.s3.amazonaws.com/path/object.zip?versionId=version-id
https://s3.amazonaws.com/bucket/path/object.zip?versionId=version-id
https://bucket.s3-region.amazonaws.com/path/object.zip?versionId=version-id
```

## Stack Traces and Exception Messages<a name="servicelens_canaries_security_stack_traces"></a>

By default, CloudWatch Synthetics canaries capture any exception thrown by your canary script, no matter whether the script is custom or is from a blueprint\. CloudWatch Synthetics logs both the exception message and the stack trace to three locations:
+ Back into the CloudWatch Synthetics service to speed up debugging when you describe test runs
+ Into CloudWatch Logs according to the configuration that your Lambda functions are created with
+ Into the Synthetics log file, which is a plaintext file that is uploaded to the Amazon S3 location specified by the value you set for the `resultsLocation` of the canary

If you want to send and store less information, you can capture exceptions before they return to the CloudWatch Synthetics wrapper library\.

## Scope Your IAM Roles Narrowly<a name="servicelens_canaries_security_canary_iam_scope"></a>

We recommend that you do not configure your canary to visit potentially malicious URLs or endpoints\. Pointing your Canary to untrusted or unknown websites or endpoints could expose your Lambda function code to malicious user’s scripts\. Assuming a malicious website can break out of Chromium, it could have access to your Lambda code in a similar way to if you connected to it using an internet browser\. 

Run your Lambda function with an IAM execution role that has scoped\-down permissions\. This way, if your Lambda function is compromised by a malicious script, it is limited in the actions it can take when running as your canary’s AWS account\.

When you use the CloudWatch console to create a canary, it is created with a scoped\-down IAM execution role\.

## Header Logging<a name="servicelens_canaries_security_logging"></a>

By default, canary header logging logs only the request URL, response code, and response status\. Within your canary script, you can customize the logging to log more or less information\. 