# Monitoring canary events with Amazon EventBridge<a name="monitoring-events-eventbridge"></a>

Amazon EventBridge event rules can notify you when canaries change status or complete runs\. EventBridge delivers a near\-real\-time stream of system events that describe changes in AWS resources\. CloudWatch Synthetics sends these events to EventBridge on a *best effort* basis\. Best effort delivery means that CloudWatch Synthetics attempts to send all events to EventBridge, but in some rare cases an event might not be delivered\. EventBridge processes all received events at least once\. Additionally, your event listeners might not receive the events in the order that the events occurred\.

**Note**  
Amazon EventBridge is an event bus service that you can use to connect your applications with data from a variety of sources\. For more information, see [What is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html) in the *Amazon EventBridge User Guide*\.

CloudWatch Synthetics emits an event when a canary changes state or completes a run\. You can create an EventBridge rule that includes an event pattern to match all event types sent from CloudWatch Synthetics, or that matches only specific event types\. When a canary triggers a rule, EventBridge invokes the target actions defined in the rule\. This allows you to send notifications, capture event information, and take corrective action, in response to a canary state change or the completion of a canary run\. For example, you can create rules for the following use cases:
+ Investigating when a canary run fails
+ Investigating when a canary has gone into the `ERROR` state
+ Tracking a canary's life cycle
+ Monitoring canary run success or failure as part of a workflow

## Example events from CloudWatch Synthetics<a name="synthetics-event-examples"></a>

This section lists example events from CloudWatch Synthetics\. For more information about event format, see [ Events and Event Patterns in EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html)\. 

**Canary status change**

In this event type, the values of `current-state` and `previous-state` can be the following:

`CREATING` \| `READY` \| `STARTING` \| `RUNNING` \| `UPDATING` \| `STOPPING` \| `STOPPED` \| `ERROR`

```
{
                "version": "0",
                "id": "8a99ca10-1e97-2302-2d64-316c5dedfd61",
                "detail-type": "Synthetics Canary Status Change",
                "source": "aws.synthetics",
                "account": "123456789012",
                "time": "2021-02-09T22:19:43Z",
                "region": "us-east-1",
                "resources": [],
                "detail": {
                                "account-id": "123456789012",
                                "canary-id": "EXAMPLE-dc5a-4f5f-96d1-989b75a94226",
                                "canary-name": "events-bb-1",
                                "current-state": "STOPPED",
                                "previous-state": "UPDATING",
                                "source-location": "NULL",
                                "updated-on": 1612909161.767,
                                "changed-config": {
                                                "executionArn": {
                                                                "previous-value": "arn:aws:lambda:us-east-1:123456789012:function:cwsyn-events-bb-1-af3e3a05-dc5a-4f5f-96d1-989EXAMPLE:1",
                                                                "current-value": "arn:aws:lambda:us-east-1:123456789012:function:cwsyn-events-bb-1-af3e3a05-dc5a-4f5f-96d1-989EXAMPLE:2"
                                                },
                                                "vpcId": {
                                                                "current-value": "NULL"
                                                },
                                                "testCodeLayerVersionArn": {
                                                                "previous-value": "arn:aws:lambda:us-east-1:123456789012:layer:cwsyn-events-bb-1-af3e3a05-dc5a-4f5f-96d1-989EXAMPLE:1",
                                                                "current-value": "arn:aws:lambda:us-east-1:123456789012:layer:cwsyn-events-bb-1-af3e3a05-dc5a-4f5f-96d1-989EXAMPLE:2"
                                                }
                                },
                                "message": "Canary status has changed"
                }
}
```

**Successful canary run completed**

```
{
                "version": "0",
                "id": "989EXAMPLE-f4a5-57a7-1a8f-d9cc768a1375",
                "detail-type": "Synthetics Canary TestRun Successful",
                "source": "aws.synthetics",
                "account": "123456789012",
                "time": "2021-02-09T22:24:01Z",
                "region": "us-east-1",
                "resources": [],
                "detail": {
                                "account-id": "123456789012",
                                "canary-id": "989EXAMPLE-dc5a-4f5f-96d1-989b75a94226",
                                "canary-name": "events-bb-1",
                                "canary-run-id": "c6c39152-8f4a-471c-9810-989EXAMPLE",
                                "artifact-location": "cw-syn-results-123456789012-us-east-1/canary/us-east-1/events-bb-1-ec3-28ddbe266797/2021/02/09/22/23-41-200",
                                "test-run-status": "PASSED",
                                "state-reason": "null",
                                "canary-run-timeline": {
                                                "started": 1612909421,
                                                "completed": 1612909441
                                },
                                "message": "Test run result is generated successfully"
                }
}
```

**Failed canary run completed**

```
{
                "version": "0",
                "id": "2644b18f-3e67-5ebf-cdfd-bf9f91392f41",
                "detail-type": "Synthetics Canary TestRun Failure",
                "source": "aws.synthetics",
                "account": "123456789012",
                "time": "2021-02-09T22:24:27Z",
                "region": "us-east-1",
                "resources": [],
                "detail": {
                                "account-id": "123456789012",
                                "canary-id": "af3e3a05-dc5a-4f5f-96d1-9989EXAMPLE",
                                "canary-name": "events-bb-1",
                                "canary-run-id": "0df3823e-7e33-4da1-8194-b04e4d4a2bf6",
                                "artifact-location": "cw-syn-results-123456789012-us-east-1/canary/us-east-1/events-bb-1-ec3-989EXAMPLE/2021/02/09/22/24-21-275",
                                "test-run-status": "FAILED",
                                "state-reason": "\"Error: net::ERR_NAME_NOT_RESOLVED \""
                                "canary-run-timeline": {
                                                "started": 1612909461,
                                                "completed": 1612909467
                                },
                                "message": "Test run result is generated successfully"
                }
}
```

It's possible that events might be duplicated or out of order\. To determine the order of events, use the `time` property\.

## Prerequisites for creating EventBridge rules<a name="create-events-rule-prereqs"></a>

Before you create an EventBridge rule for CloudWatch Synthetics, you should do the following:
+ Familiarize yourself with events, rules, and targets in EventBridge\.
+ Create and configure the targets invoked by your EventBridge rules\. Rules can invoke many types of targets, including:
  + Amazon SNS topics
  + AWS Lambda functions
  + Kinesis streams
  + Amazon SQS queues

For more information, see [What is Amazon EventBridge?](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html) and [Getting started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*\.

## Create an EventBridge rule \(CLI\)<a name="create-events-rule-cli"></a>

The steps in the following example create an EventBridge rule that publishes an Amazon SNS topic when the canary named `my-canary-name` in `us-east-1` completes a run or changes state\.

1. Create the rule\.

   ```
   aws events put-rule \
     --name TestRule \
     --region us-east-1 \ 
     --event-pattern "{\"source\": [\"aws.synthetics\"], \"detail\": {\"canary-name\": [\"my-canary-name\"]}}"
   ```

   Any properties you omit from the pattern are ignored\.

1. Add the topic as a rule target\.
   + Replace *topic\-arn* with the Amazon Resource Name \(ARN\) of your Amazon SNS topic\.

   ```
   aws events put-targets \
     --rule TestRule \
     --targets "Id"="1","Arn"="topic-arn"
   ```
**Note**  
To allow Amazon EventBridge to call your target topic, you must add a resource\-based policy to your topic\. For more information, see [Amazon SNS permissions](https://docs.aws.amazon.com/eventbridge/latest/userguide/resource-based-policies-eventbridge.html#sns-permissions) in the *Amazon EventBridge User Guide*\.

For more information, see [Events and event patterns in EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-and-event-patterns.html) in the *Amazon EventBridge User Guide*\.