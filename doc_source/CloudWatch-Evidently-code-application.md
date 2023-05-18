# Adding code to your application<a name="CloudWatch-Evidently-code-application"></a>

To work with CloudWatch Evidently, you add code to your application to assign a variation to each user session, and to send metrics to Evidently\. You use the CloudWatch Evidently `EvaluateFeature` operation to assign variations to user sessions, and you use the `PutProjectEvents` operation to send events to Evidently to be used to calculate metrics for your launches or experiments\.

When you create variations or custom metrics, the CloudWatch Evidently console provides samples of the code you need to add\.

For an end\-to\-end example, see [Tutorial: A/B testing with the Evidently sample application](CloudWatch-Evidently-sample-application.md)\.

## Using EvaluateFeature<a name="CloudWatch-Evidently-code-EvaluateFeature"></a>

When feature variations are used in a launch or experiment, the application uses the [ EvaluateFeature](https://docs.aws.amazon.com/cloudwatchevidently/latest/APIReference/API_EvaluateFeature.html) operation to assign each user session a variation\. The assignment of a variation to a user is an *evaluation event*\. When you call this operation, you pass in the following:
+ **Feature name**– Required\. Evidently processes the evaluation according to the feature evaluation rules of the launch or experiment, and selects a variation for the entity\.
+ **entityId**– Required\. Represents a unique user\.
+ **evaluationContext**– Optional\. A JSON object representing additional information about a user\. Evidently will use this value to match the user to a *segment* of your audience during feature evaluations, if you have created segments\. For more information, see [Use segments to focus your audience](CloudWatch-Evidently-segments.md)\.

  The following is an example of an `evaluationContext` value that you can send to Evidently\.

  ```
  {
      "Browser": "Chrome",
      "Location": {
          "Country": "United States",
          "Zipcode": 98007
      }
  }
  ```

**Sticky evaluations**

CloudWatch Evidently uses "sticky" evaluations\. A single configuration of `entityId`, feature, *feature configuration*, and `evaluationContext` always receives the same variation assignment\. The only time this variation assignment changes is when an entity is added to an override or the experiment traffic is dialed up\.

A feature configuration includes the following:
+ The feature variations
+ The variation configuration \(percentages assigned to each variation\) for a currently running experiment for this feature, if any\.
+ The variation configuration for a currently running launch for this feature, if any\. The variation configuration includes the defined segment overrides, if any\.

If an experiment's traffic allocation is *increased*, any `entityId` that was previously assigned to an experiment treatment group will continue to receive the same treatment\. Any `entityId` that was previously assigned to the control group, might be assigned to an experiment treatment group, according to the variation configuration specified for the experiment\.

If an experiment's traffic allocation is *decreased*, an `entityId` might go from a treatment group to a control group, but it will not go into a different treatment group\.

## Using PutProjectEvents<a name="CloudWatch-Evidently-code-PutProjectEvents"></a>

To code a custom metric for Evidently, you use the [ PutProjectEvents](https://docs.aws.amazon.com/cloudwatchevidently/latest/APIReference/API_PutProjectEvents.html) operation\. The following is a simple payload example\.

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

To ensure that events are associated with the correct launch or experiment, you must pass the same `entityId` when you call both `EvaluateFeature` and `PutProjectEvents`\. Be sure to call `PutProjectEvents` after the `EvaluateFeature` call, otherwise data is dropped and won't be used by CloudWatch Evidently\.

The `PutProjectEvents` operation does not require the feature name as an input parameter\. This way, you can use a single event in multiple experiments\. For example, suppose you call `EvaluateFeature` with the `entityId` set to `userDetails.userId`\. If you have two or more experiments running, you can have a single event from that user's session emit metrics for each of those experiments\. To do this, you call `PutProjectEvents` once for each experiment, using that same `entityId`\.

**Timing**

After your application calls `EvaluateFeature`, there is a one\-hour time period where metric events from `PutProjectEvents` are attributed based on that evaluation\. If any more events occur after the one\-hour period, they are not attributed\.

However, if the same `entityId` is used for a new `EvaluateFeature` call during that initial call's one\-hour window, the later `EvaluateFeature` result is now used instead, and the one\-hour timer is restarted\. This can only happen in certain circumstances, such as when experiment traffic is dialed up between the two assignments, as explained in the previous **Sticky evaluations** section\.

For an end\-to\-end example, see [Tutorial: A/B testing with the Evidently sample application](CloudWatch-Evidently-sample-application.md)\.