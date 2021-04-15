# Using the CloudWatch Synthetics Recorder for Google Chrome<a name="CloudWatch_Synthetics_Canaries_Recorder"></a>

Amazon provides a CloudWatch Synthetics Recorder to help you create canaries more easily\. The recorder is a Google Chrome extension\.

The recorder records your click and type actions on a website and automatically generates a Node\.js script that can be used to create a canary that follows the same steps\.

After you start recording, the CloudWatch Synthetics Recorder detects your actions in the browser and converts them to a script\. You can pause and resume the recording as needed\. When you stop recording, the recorder produces a Node\.js script of your actions, which you can easily copy with the **Copy to Clipboard** button\. You can then use this script to create a canary in CloudWatch Synthetics\. 

**Credits**: The CloudWatch Synthetics Recorder is based on the [ Headless recorder ](https://github.com/checkly/headless-recorder)\.

## Installing the CloudWatch Synthetics recorder extension for Google Chrome<a name="CloudWatch_Synthetics_Canaries_Recorder-install"></a>

To use the CloudWatch Synthetics recorder, you can start creating a canary and choose the **Canary Recorder** blueprint\. If you do this when you haven't already downloaded the recorder, the CloudWatch Synthetics console provides a link to download it\.

Alternatively, you can follow these steps to download and install the recorder directly\.

**To install the CloudWatch Synthetics Recorder**

1. Using Google Chrome, go to this website: [ https://chrome\.google\.com/webstore/detail/cloudwatch\-synthetics\-rec/bhdnlmmgiplmbcdmkkdfplenecpegfno ](https://chrome.google.com/webstore/detail/cloudwatch-synthetics-rec/bhdnlmmgiplmbcdmkkdfplenecpegfno)

1. Choose **Add to Chrome**, then choose **Add extension**\.

## Using the CloudWatch Synthetics Recorder for Google Chrome<a name="CloudWatch_Synthetics_Canaries_Recorder-using"></a>

To use the CloudWatch Synthetics recorder to help you create a canary, you can choose **Create canary** in the CloudWatch console, and then choose **Use a blueprint**, **Canary Recorder**\. For more information, see [Creating a canary](CloudWatch_Synthetics_Canaries_Create.md)\. 

Alternatively, you can use the recorder to record steps without immediately using them to create a canary\.

**To use the CloudWatch Synthetics recorder to record your actions on a website**

1. Navigate to the page that you want to monitor\.

1. Choose the Chrome extensions icon, and then choose **CloudWatch Synthetics Recorder**\.

1. Choose **Start Recording**\.

1. Perform the steps that you want to record\. To pause recording, choose **Pause**\.

1. When you are finished recording the workflow, choose **Stop recording**\.

1. Choose **Copy to clipboard** to copy the generated script to your clipboard\. Or, if you want to start over, choose **New recording**\.

1. To create a canary with the copied script, you can paste your copied script into the recorder blueprint inline editor, or save it to an Amazon S3 bucket and import it from there\.

1. If you're not immediately creating a canary, you can save your recorded script to a file\.

## Known limitations of the CloudWatch Synthetics Recorder<a name="CloudWatch_Synthetics_Canaries_Recorder-limitations"></a>

The CloudWatch Synthetics recorder for Google Chrome currently has the following limitations\.
+ HTML elements that don’t have IDs will use CSS selectors\. This can break canaries if the webpage structure changes later\. We plan to provide some configuration options \(such as using data\-id\) around this in a future version of the recorder\. 
+ The recorder doesn't support actions such as double\-click or copy/paste, and doesn't support key combinations such as CMD\+0\. 
+ To verify the presence of an element or text on the page, users must add assertions after the script is generated\. The recorder doesn't support verifying an element without performing any action on that element\. This is similar to the “Verify text” or “Verify element” options in the canary workflow builder\. We plan to add some assertions support in a future version of the recorder\. 
+ The recorder records all actions in the tab where the recording is initiated\. It doesn't record pop\-ups \(for instance, to allow location tracking\) or navigation to different pages from pop\-ups\. 