# ARM64 platforms<a name="Lambda-Insights-extension-versionsARM"></a>

This section lists the versions of the Lambda Insights extension for ARM64 platforms, and the ARNs to use for these extensions in each AWS Region\.

## 1\.0\.135\.0<a name="Lambda-Insights-extension-ARM-1.0.135.0"></a>

Version 1\.0\.135\.0 includes bug fixes for how Lambda Insights collects and reports disk and file descriptor usage\. In previous versions of the extension, the `tmp_free` metric reported the maximum free space in the `/tmp` directory while a function runs\. This version changes the metric to report the minimum value instead, making it more useful when assessing disk usage\. For more information about `tmp` directory storage quotas, see [Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)\.

Version 1\.0\.135\.0 also now reports file descriptor usage \(`fd_use` and `fd_max`\) as the maximum value across processes rather than reporting the operating system level\. 


| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\) |  `arn:aws:lambda:us-east-1:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  US East \(Ohio\) |  `arn:aws:lambda:us-east-2:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  US West \(Oregon\) |  `arn:aws:lambda:us-west-2:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Asia Pacific \(Mumbai\) |  `arn:aws:lambda:ap-south-1:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Asia Pacific \(Singapore\) |  `arn:aws:lambda:ap-southeast-1:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Asia Pacific \(Sydney\) |  `arn:aws:lambda:ap-southeast-2:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Asia Pacific \(Tokyo\) |  `arn:aws:lambda:ap-northeast-1:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Europe \(Frankfurt\) |  `arn:aws:lambda:eu-central-1:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Europe \(Ireland\) |  `arn:aws:lambda:eu-west-1:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 
|  Europe \(London\) |  `arn:aws:lambda:eu-west-2:580247275435:layer:LambdaInsightsExtension-Arm64:2`  | 

## 1\.0\.119\.0<a name="Lambda-Insights-extension-ARM-1.0.119.0"></a>


| Region | ARN | 
| --- | --- | 
|  US East \(N\. Virginia\) |  `arn:aws:lambda:us-east-1:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  US East \(Ohio\) |  `arn:aws:lambda:us-east-2:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  US West \(Oregon\) |  `arn:aws:lambda:us-west-2:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Asia Pacific \(Mumbai\) |  `arn:aws:lambda:ap-south-1:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Asia Pacific \(Singapore\) |  `arn:aws:lambda:ap-southeast-1:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Asia Pacific \(Sydney\) |  `arn:aws:lambda:ap-southeast-2:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Asia Pacific \(Tokyo\) |  `arn:aws:lambda:ap-northeast-1:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Europe \(Frankfurt\) |  `arn:aws:lambda:eu-central-1:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Europe \(Ireland\) |  `arn:aws:lambda:eu-west-1:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 
|  Europe \(London\) |  `arn:aws:lambda:eu-west-2:580247275435:layer:LambdaInsightsExtension-Arm64:1`  | 