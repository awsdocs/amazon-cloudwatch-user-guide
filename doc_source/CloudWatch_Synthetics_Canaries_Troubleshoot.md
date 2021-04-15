# Troubleshooting a failed canary<a name="CloudWatch_Synthetics_Canaries_Troubleshoot"></a>

If your canary fails, check the following for troubleshooting\.

**General troubleshooting**
+ Use the canary details page to find more information\. In the CloudWatch console, choose **Canaries** in the navigation pane and then choose the name of the canary to open the canary details page\. In the **Availability** tab, check the **SuccessPercent** metric to see whether the problem is constant or intermittent\.

  While still in the **Availability** tab, choose a failed data point to see screenshots, logs, and step reports \(if available\) for that failed run\.

  If a step report is available because steps are part of your script, check to see which step has failed and see the associated screenshots to see the issue that your customers are seeing\.

  You can also check the HAR files to see if one or more requests are failing\. You can dig deeper by using logs to drill down on failed requests and errors\. Finally, you can compare these artifacts with the artifiacts from a successful canary run to pinpoint the issue\.

  By default, CloudWatch Synthetics captures screenshots for each step in a UI canary\. However, your script might be configured to disable screenshots\. During debugging, you may want to enable screenshots again\. Similarly, for API canaries you might want to see HTTP request and response headers and body during debugging\. For information about how to include this data in the report, see [executeHttpStep\(stepName, requestOptions, \[callback\], \[stepConfig\]\)](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_executeHttpStep)\.
+ If you had a recent deployment to your application, roll it back and then debug later\.
+ Connect to your endpoint manually to see if you can reproduce the same issue\.

**Topics**
+ [Canary runtime version upgrade and downgrade issues](#CloudWatch_Synthetics_Canaries_Troubleshoot_upgradeissues)
+ [Waiting for an element to appear](#CloudWatch_Synthetics_Canaries_Troubleshoot_waiting)
+ [Node is either not visible or not an HTMLElement for page\.click\(\)](#CloudWatch_Synthetics_Canaries_Troubleshoot_notvisible)
+ [Unable to upload artifacts to S3, Exception: Unable to fetch S3 bucket location: Access Denied](#CloudWatch_Synthetics_Canaries_Troubleshoot_noupload)
+ [Error: Protocol error \(Runtime\.callFunctionOn\): Target closed\.](#CloudWatch_Synthetics_Canaries_Troubleshoot_protocolError)
+ [Canary Failed\. Error: No datapoint \- Canary Shows timeout error](#CloudWatch_Synthetics_Canaries_Troubleshoot_nodatapoint)
+ [Trying to access an internal endpoint](#CloudWatch_Synthetics_Canaries_Troubleshoot_internalendpoint)
+ [Cross\-origin request sharing \(CORS\) issue](#CloudWatch_Synthetics_Canaries_CORS)
+ [Troubleshooting a canary on a VPC](CloudWatch_Synthetics_Canaries_VPC_troubleshoot.md)

## Canary runtime version upgrade and downgrade issues<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_upgradeissues"></a>

If you recently upgraded the canary from runtime version `syn-1.0` to a later version, it may be a cross\-origin request sharing \(CORS\) issue\. For more information, see [Cross\-origin request sharing \(CORS\) issue](#CloudWatch_Synthetics_Canaries_CORS)\.

If you recently downgraded the canary to an older runtime version, check to make sure that the CloudWatch Synthetics functions that you are using are available in the older runtime version that you downgraded to\. For example, the `executeHttpStep` function is available for runtime version `syn-nodejs-2.2` and later\. To check on the availability of functions, see [Writing a canary script](CloudWatch_Synthetics_Canaries_WritingCanary.md)\. 

**Note**  
When you plan to updgrade or downgrade the runtime version for a canary, we recommend that you first clone the canary and update the runtime version in the cloned canary\. Once you have verified that the clone with the new runtime version works, you can update the runtime version of your original canary and delete the clone\.

## Waiting for an element to appear<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_waiting"></a>

After analyzing your logs and screenshots, if you see that your script is waiting for an element to appear on screen and times out, check the relevant screenshot to see if the element appears on the page\. Verify your `xpath` to make sure that it is correct\.

For Puppetteer\-related issues, check [ Puppeteer's GitHub page](https://github.com/puppeteer/puppeteer/issues) or internet forums\.

## Node is either not visible or not an HTMLElement for page\.click\(\)<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_notvisible"></a>

If a node is not visible or is not an `HTMLElement` for `page.click()`, first verify the `xpath` that you are using to click the element\. Also, if your element is at the bottom of the screen, adjust your viewport\. CloudWatch Synthetics by default uses a viewport of 1920 \* 1080\. You can set a different viewport when you launch the browser or by using the Puppeteer function `page.setViewport`\.

## Unable to upload artifacts to S3, Exception: Unable to fetch S3 bucket location: Access Denied<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_noupload"></a>

This means that CloudWatch Synthetics was unable to upload screenshots, logs, or reports created for the canary because of permission issues\. Make sure that canary role has the necessary permissions\. For more information, see [Required roles and permissions for CloudWatch canaries](CloudWatch_Synthetics_Canaries_Roles.md)\.

**Note**  
Your CloudWatch metrics might show a datapoint as `Passed` even when the canary has failed with this error\. This is because CloudWatch Synthetics publishes the `SuccessPercent` metric as `Passed` if your script has passed\.  
Failure to upload artifacts does not indicate any issues with your script\. Therefore, these errors fail the canary but will not trigger any alarms configured on your canary\.

You can add your own execution errors by using the CloudWatch Synthetics `addExecutionError` function\. You should only track errors as execution errors if they are not important to indicate the success or failure of your script\. For more information about this function, see [addExecutionError\(errorMessage, ex\);](CloudWatch_Synthetics_Canaries_Library_Nodejs.md#CloudWatch_Synthetics_Library_addExecutionError)\. 

## Error: Protocol error \(Runtime\.callFunctionOn\): Target closed\.<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_protocolError"></a>

This error appears if there are some network requests after the page or browser is closed\. You might have forgotten to wait for an asynchronous operation\. After executing your script, CloudWatch Synthetics closes the browser\. The execution of any asynchronous operation after the browser is closed might cause `target closed error`\. 

## Canary Failed\. Error: No datapoint \- Canary Shows timeout error<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_nodatapoint"></a>

This means that your canary run exceeded the timeout\. The canary execution stopped before CloudWatch Synthetics could publish success percent CloudWatch metrics or update artifacts such as HAR files, logs and screenshots\. If your timeout is too low, you can increase it\.

By default, a canary timeout value is equal to its frequency\. You can manually adjust the timeout value to be less than or equal to the canary frequency\. If your canary frequency is low, you must increase the frequency to increase the timeout\. You can adjust both the frequency and the timeout value c under **Schedule** when you create or update a canary by using the CloudWatch Synthetics console\.

Canary artifacts are not available to view in the CloudWatch Synthetics console when this error happens\. You can use CloudWatch Logs to see the canary's logs\.

**To use CloudWatch Logs to see the logs for a canary**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Log groups**\.

1. Find the log group by typing the canary name in the filter box\. Log groups for canaries have the name **/aws/lambda/cwsyn\-*canaryName*\-randomId**\.

## Trying to access an internal endpoint<a name="CloudWatch_Synthetics_Canaries_Troubleshoot_internalendpoint"></a>

If you want your canary to access an endpoint on your internal network, we recommend that you set up CloudWatch Synthetics to use VPC\. For more information, see [Running a canary on a VPC](CloudWatch_Synthetics_Canaries_VPC.md)\.

## Cross\-origin request sharing \(CORS\) issue<a name="CloudWatch_Synthetics_Canaries_CORS"></a>

In a UI canary, if some network requests are failing with `403` or `net::ERR_FAILED`, check whether the canary has active tracing enabled and also uses the Puppeteer function `page.setExtraHTTPHeaders` to add headers\. If so, the failed network requests might be caused by cross\-origin request sharing \(CORS\) restrictions\. You can confirm whether this is the case by disabling active tracing or removing the extra HTTP headers\.

**Why does this happen?**

When active tracing is used, an extra header is added to all outgoing requests to trace the call\. Modifying the request headers by adding a trace header or adding extra headers using Puppeteer’s `page.setExtraHTTPHeaders` causes a CORS check for XMLHttpRequest \(XHR\) requests\.

If you don't want to disable active tracing or remove the extra headers, you can update your web application to allow cross\-origin access or you can disable web security by using the `disable-web-security` flag when you launch the Chrome browser in your script\.

You can override launch parameters used by CloudWatch Synthetics and pass additional `disable-web-security` flag parameters by using the CloudWatch Synthetics launch function\. For more information, see [Library functions available for Node\.js canary scripts](CloudWatch_Synthetics_Canaries_Library_Nodejs.md)\.

**Note**  
You can override launch parameters used by CloudWatch Synthetics when you use runtime version `syn-nodejs-2.1` or later\.