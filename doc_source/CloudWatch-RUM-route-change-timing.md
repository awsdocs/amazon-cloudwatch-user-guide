# Route change timing for single\-page applications<a name="CloudWatch-RUM-route-change-timing"></a>

In a traditional multi\-page application, when a user requests for new content to be loaded, the user is actually requesting a new HTML page from the server\. As a result, the CloudWatch RUM web client captures the load times using the regular performance API metrics\.

However, single\-page web applications use JavaScript and Ajax to update the interface without loading a new page from the server\. Single\-page updates are not recorded by the browser timing API, but instead use route change timing\.

CloudWatch RUM supports the monitoring of both full page loads from the server and single\-page updates, with the following differences:
+ For route change timing, there are no browser\-provided metrics such as `tlsTime`, `timeToFirstByte`, and so on\.
+ For route change timing, the `initiatorType` field will be `route_change`\. 

The CloudWatch RUM web client listens to user interactions that may lead to a route change, and when such a user interaction is recorded, the web client records a timestamp\. Then route change timing will begin if both of the following are true:
+ A browser history API \(except browser forward and back buttons\) was used to perform the route change\.
+ The difference between the time of route change detection and latest user interaction timestamp is less than 1000 ms\. This avoids data skew\.

Then, once route change timing begins, that timing completes if there are no ongoing AJAX requests and DOM mutations\. Then the timestamp of the latest completed activity will be used as the completion timestamp\.

Route change timing will time out if there are ongoing AJAX requests or DOM mutations for more than 10 seconds \(by default\)\. In this case, the CloudWatch RUM web client will no longer record timing for this route change\.

As a result, the duration of a route change event is calculated as the following:

```
(time of latest completed activity) - (latest user interaction timestamp)
```