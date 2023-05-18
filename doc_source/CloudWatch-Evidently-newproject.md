# Create a new project<a name="CloudWatch-Evidently-newproject"></a>

Use these steps to set up a new CloudWatch Evidently project\.

**To create a new CloudWatch Evidently project**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **Evidently**\.

1. Choose **Create project**\.

1. For **Project name**, enter a name to be used to identify this project within the CloudWatch Evidently console\.

   You can optionally add a project description\.

1. For **Evaluation event storage**, choose whether you want to store the evaluation events that you collect with Evidently\. Even if you don't store these events, Evidently aggregates them to create metrics and other experiment data that you can view in the Evidently dashboard\. For more information, see [Project data storage](CloudWatch-Evidently-datastorage.md)\.

1. For **Use client\-side evaluation**, choose whether you want to enable client\-side evaluation for this project\. With client\-side evaluation, your application can assign variations to user sessions locally instead of by calling the [EvaluateFeature](https://docs.aws.amazon.com/cloudwatchevidently/latest/APIReference/API_EvaluateFeature.html) operation\. This mitigates the latency and availability risks that come with an API call\. For more information, see [Use client\-side evaluation \- powered by AWS AppConfig](CloudWatch-Evidently-client-side-evaluation.md)\.

   To create a project with client\-side evaluation, you must have the `evidently:ExportProjectAsConfiguration` permission\.

   If you enable client\-side evaluation, also do the following:

   1. Choose whether to use an existing AWS AppConfig application or create a new one\. 

   1. Choose whether to use an existing AWS AppConfig environment or create a new one\. 

      For more information about applications and environments in AWS AppConfig, see [ How AWS AppConfig works](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html#learn-more-appconfig-how-it-works)\. 

1. \(Optional\) To add tags to this project, choose **Tags**, **Add new tag**\.

   Then, for **Key**, enter a name for the tag\. You can add an optional value for the tag in **Value**\. 

   To add another tag, choose **Add new tag** again\.

   For more information, see [Tagging AWS Resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

1. Choose **Create project**\.