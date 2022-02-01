# Create a custom widget<a name="add_custom_widget_dashboard_create"></a>

To create a custom widget, you can use one of the samples provided by Amazon, or you can create your own\. The Amazon samples include samples in both JavaScript and Python, and are created by a AWS CloudFormation stack\. For a list of samples, see [Sample custom widgets](add_custom_widget_samples.md)\.

**To create a custom widget in a CloudWatch dashboard**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Dashboards**, and then select a dashboard\.

1. Choose **Add widget**\.

1. Choose **Custom widget**\.

1. Use one of the following methods:
   + To use a sample custom widget provided by Amazon, do the following:

     1. Select the sample in the dropdown box\.

        The AWS CloudFormation console launches in a new browser\. In the AWS CloudFormation console, do the following:

     1. \(Optional\) Customize the AWS CloudFormation stack name\.

     1. Make selections for any parameters used by the sample\.

     1. Select **I acknowledge that AWS CloudFormation might create IAM resources**, and choose **Create stack**\.
   + To create your own custom widget provided by Amazon, do the following:

     1. Choose **Next**\.

     1. Choose to either select your Lambda function from a list, or enter its Amazon Resource Name \(ARN\)\. If you select it from a list, also specify the Region where the function is and the version to use\.

     1. For **Parameters**, make selections for any parameters used by the function\.

     1. Enter a title for the widget\.

     1. For **Update on**, configure when the widget should be updated \(when the Lambda function should be called again\)\. This can be one or more of the following: **Refresh** to update it when the dashboard auto\-refreshes, **Resize** to update it whenever the widget is resized, or **Time Range** to update it whenever the dashboard's time range is adjusted, including when graphs are zoomed into\.

     1. If you are satisfied with the preview, choose **Create widget**\.