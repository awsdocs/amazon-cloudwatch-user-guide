# Edit your application<a name="CloudWatch-RUM-edit-application"></a>

To change an app monitor's settings, follow these steps\. You can change any settings except the app monitor name\.

**To edit how your application uses CloudWatch RUM**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Application monitoring**, **RUM**\.

1. Choose **List view**\.

1. Choose the button next to the name of the application, and then choose **Actions**, **Edit**\.

1. Change any settings except the application name\. For more information about the settings, see [Step 2: Create an app monitor](CloudWatch-RUM-get-started-create-app-monitor.md)\.

1. When finished, choose **Save**\.

   Changing the settings changes the code snippet\. You must now paste the updated code snippet into your application\.

1. After the JavaScript code snippet is created, choose **Copy to clipboard** or **Download**, and then choose **Done**\.

   To start monitoring with the new settings, you insert the code snippet into your application\. Insert the code snippet inside the `<head>` element of your application, before the `<body>` element or any other `<script>` tags\.