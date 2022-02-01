# Security and JavaScript<a name="add_custom_widget_dashboard_security"></a>

For security reasons, JavaScript is not allowed in the returned HTML\. Removing the JavaScript prevents permission escalation issues, where the writer of the Lambda function injects code that could run with higher permissions than the user viewing the widget on the dashboard\.

If the returned HTML contains any JavaScript code or other known security vulnerabilities, it is cleaned from the HTML before it is rendered on the dashboard\. For example, the **<iframe>** and **<use>** tags are not allowed and are removed\.

Custom Widgets won't run by default in a dashboard\. Instead, you must explicitly allow a custom widget to run if you trust the Lambda function that it invokes\. You can choose to allow it once or allow always, for both individual widgets and entire dashboard\. You can also deny permission for individual widgets and the entire dashboard\.