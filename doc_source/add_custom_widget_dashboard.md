# Add a custom widget to a CloudWatch dashboard<a name="add_custom_widget_dashboard"></a>

A *custom widget* is a CloudWatch dashboard widget that can call any AWS Lambda function with custom parameters\. It then displays the returned HTML or JSON\. Custom widgets are a simple way to build a custom data view on a dashboard\. If you can write Lambda code and create HTML, you can create a useful custom widget\. Additionally, Amazon provides several pre\-built custom widgets that you can create without any code\.

When you create a Lambda function to use as a custom widget, we strongly recommend that you include the prefix **customWidget** in the function name\. This helps you know which of your Lambda functions are safe to use when you add custom widgets to your dashboard\.

Custom widgets behave like other widgets on your dashboard\. They can be refreshed and auto\-refreshed, resized, and moved around\. They react to the time range of the dashboard\.

If you have set up CloudWatch console cross\-account functionality, you can add a custom widget created in one account to dashboards in other accounts\. For more information, see [Cross\-account cross\-Region CloudWatch console](Cross-Account-Cross-Region.md)\.

You can also use custom widgets on your own website by using the CloudWatch dashboard sharing feature\. For more information, see [Sharing CloudWatch dashboards](cloudwatch-dashboard-sharing.md)\.

**Topics**
+ [Details about custom widgets](add_custom_widget_dashboard_about.md)
+ [Security and JavaScript](add_custom_widget_dashboard_security.md)
+ [Interactivity in the custom widget](add_custom_widget_dashboard_interactivity.md)
+ [Create a custom widget](add_custom_widget_dashboard_create.md)
+ [Sample custom widgets](add_custom_widget_samples.md)