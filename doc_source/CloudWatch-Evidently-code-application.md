# Adding code to your application<a name="CloudWatch-Evidently-code-application"></a>

To work with CloudWatch Evidently, you add code to your application to assign a variation to each user session, and to send metrics to Evidently\. When you create variations or custom metrics, the CloudWatch Evidently console provides samples of the code you need to add\.

When feature variations are used in a launch or experiment, the application uses the `EvaluateFeature` operation to assign each user session a variation\. The assignment of a variation to a user is an *evaluation event*\. When you call this operation, you pass in the following:
+ **Feature name**– Required\. Evidently processes the evaluation according to the feature evaluation rules of the launch or experiment, and selects a variation for the entity\.
+ **entityId**– Required\. Represents a user session\.
+ **evaluationContext**– Optional\. This can includes attributes of the entity to record along with the evaluation event\.

To code a custom metric for Evidently, you use the `PutProjectEvents` operation\. The following is a simple payload example\.

```
{
    "events": [
        {
            "timestamp": {{$timestamp}},
            "type": "aws.evidently.custom",
            "data": "{\"details\": {\"pageLoadTime\": 800.0}, \"userDetails\": {\"userId\": \"test-user\"}}"
        }
    ]
}
```

The `entityIdKey` can just be an `entityId` or you can rename it to anything else, such as `userId`\. In the actual event, `entityId` can be a username, a session ID, and so on\.

```
"metricDefinition":{
    "name": "noFilter",
    "entityIdKey": "userDetails.userId", //should be consistent with jsonValue in events "data" fields
    "valueKey": "details.pageLoadTime"
},
```

Be sure to call `PutProjectEvents` after the `EvaluateFeature` call, otherwise data is dropped and won't be available on the experiment results page\. When you call `PutProjectEvents`, the `entityId` must be the same value as the `entityId` used in the `EvaluateFeature` call, otherwise the custom metric data will be dropped\.

For an end\-to\-end example, see [Tutorial: A/B testing with the sample bookstore app](CloudWatch-Evidently-bookstoreexample.md)\.