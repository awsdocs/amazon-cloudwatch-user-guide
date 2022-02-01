# Step 5: Test your app monitor setup by generating user events<a name="CloudWatch-RUM-get-started-generate-data"></a>

After you have inserted the code snippet and your updated application is running, you can test it by manually generating user events\. To test this, we recommend that you do the following\. This testing incurs standard CloudWatch RUM charges\.
+ Navigate between pages in your web application\.
+ Create multiple user sessions, using different browsers and devices\.
+ Make requests\.
+ Cause JavaScript errors\.

After you have generated some events, view them in the CloudWatch RUM dashboard\. For more information, see [Viewing the CloudWatch RUM dashboard](CloudWatch-RUM-view-data.md)\.

Data from user sessions might take up to 15 minutes to appear in the dashboard\.

If you don't see data 15 minutes after you generated events in the application, see [Troubleshooting CloudWatch RUM](CloudWatch-RUM-troubleshooting.md)\.