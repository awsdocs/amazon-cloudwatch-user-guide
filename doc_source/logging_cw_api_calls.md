# Logging Amazon CloudWatch API Calls in AWS CloudTrail<a name="logging_cw_api_calls"></a>

AWS CloudTrail is a service that captures API calls made by or on behalf of your AWS account\. This information is collected and written to log files that are stored in an Amazon S3 bucket that you specify\. API calls are logged whenever you use the API, the console, or the AWS CLI\. Using the information collected by CloudTrail, you can determine what request was made, the source IP address the request was made from, who made the request, when it was made, and so on\. 

To learn more about CloudTrail, including how to configure and enable it, see the [What is AWS CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/what_is_cloud_trail_top_level.html) in the *AWS CloudTrail User Guide*\.


+ [CloudWatch Information in CloudTrail](#cw_info_in_ct)
+ [Understanding Log File Entries](#understanding_cw_log_file_entries)

## CloudWatch Information in CloudTrail<a name="cw_info_in_ct"></a>

If CloudTrail logging is turned on, calls made to API actions are captured in log files\. Every log file entry contains information about who generated the request\. For example, if a request is made to create or update a CloudWatch alarm \(`PutMetricAlarm`\), CloudTrail logs the user identity of the person or service that made the request\.

The user identity information in the log entry helps you determine the following:

+ Whether the request was made with root or IAM user credentials

+ Whether the request was made with temporary security credentials for a role or federated user

+ Whether the request was made by another AWS service

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html) in the *AWS CloudTrail User Guide*\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted by using Amazon S3 server\-side encryption \(SSE\)\.

If you want to be notified upon log file delivery, you can configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html) in the *AWS CloudTrail User Guide*\.

You can also aggregate Amazon CloudWatch Logs log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html) in the *AWS CloudTrail User Guide*\.

When logging is turned on, the following API actions are written to CloudTrail:

**CloudWatch** 

+ DeleteAlarms

+ DescribeAlarmHistory

+ DescribeAlarms

+ DescribeAlarmsForMetric

+ DisableAlarmActions

+ EnableAlarmActions

+ PutMetricAlarm

+ SetAlarmState

The CloudWatch `GetMetricStatistics`, `ListMetrics`, and `PutMetricData` API actions are not supported\. For more information about all of these actions, see the [Amazon CloudWatch API Reference](http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/)\.

**CloudWatch Events** 

+ DeleteRule

+ DescribeRule

+ DisableRule

+ EnableRule

+ ListRuleNamesByTarget

+ ListRules

+ ListTargetsByRule

+ PutRule

+ PutTargets

+ RemoveTargets

+ TestEventPattern

For more information about these actions, see the [Amazon CloudWatch Events API Reference](http://docs.aws.amazon.com/AmazonCloudWatchEvents/latest/APIReference/)\.

**CloudWatch Logs** Request and response elements are logged for these API actions: 

+ CancelExportTask

+ CreateExportTask

+ CreateLogGroup

+ CreateLogStream

+ DeleteDestination

+ DeleteLogGroup

+ DeleteLogStream

+ DeleteMetricFilter

+ DeleteRetentionPolicy

+ DeleteSubscriptionFilter

+ PutDestination

+ PutDestinationPolicy

+ PutMetricFilter

+ PutRetentionPolicy

+ PutSubscriptionFilter

+ TestMetricFilter

Only Request elements are logged for these API actions:

+ DescribeDestinations

+ DescribeExportTasks

+ DescribeLogGroups

+ DescribeLogStreams

+ DescribeMetricFilters

+ DescribeSubscriptionFilters

The CloudWatch Logs `GetLogEvents`, `PutLogEvents`, and `FilterLogEvents` API actions are not supported\. For more information about these actions, see the [Amazon CloudWatch Logs API Reference](http://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/)\.

## Understanding Log File Entries<a name="understanding_cw_log_file_entries"></a>

CloudTrail log files contain one or more log entries\. Each entry lists multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. The log entries are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. Log file entries for all API actions are similar to the examples below\.

The following log file entry shows that a user called the CloudWatch **PutMetricAlarm** action\.

```
{
    "Records": [{
        "eventVersion": "1.01",
        "userIdentity": {
            "type": "Root",
            "principalId": "EX_PRINCIPAL_ID",
            "arn": "arn:aws:iam::123456789012:root",
            "accountId": "123456789012",
            "accessKeyId": "EXAMPLE_KEY_ID"
        },
        "eventTime": "2014-03-23T21:50:34Z",
        "eventSource": "monitoring.amazonaws.com",
        "eventName": "PutMetricAlarm",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "127.0.0.1",
        "userAgent": "aws-sdk-ruby2/2.0.0.rc4 ruby/1.9.3 x86_64-linux Seahorse/0.1.0",
        "requestParameters": {
            "threshold": 50.0,
            "period": 60,
            "metricName": "CloudTrail Test",
            "evaluationPeriods": 3,
            "comparisonOperator": "GreaterThanThreshold",
            "namespace": "AWS/CloudWatch",
            "alarmName": "CloudTrail Test Alarm",
            "statistic": "Sum"
        },
        "responseElements": null,
        "requestID": "29184022-b2d5-11e3-a63d-9b463e6d0ff0",
        "eventID": "b096d5b7-dcf2-4399-998b-5a53eca76a27"
    },
    ..additional entries
  ]
  }
```

The following log file entry shows that a user called the CloudWatch Events **PutRule** action\.

```
{
         "eventVersion":"1.03",
         "userIdentity":{
            "type":"Root",
            "principalId":"123456789012",
            "arn":"arn:aws:iam::123456789012:root",
            "accountId":"123456789012",
            "accessKeyId":"AKIAIOSFODNN7EXAMPLE",
            "sessionContext":{
               "attributes":{
                  "mfaAuthenticated":"false",
                  "creationDate":"2015-11-17T23:56:15Z"
               }
            }
         },
         "eventTime":"2015-11-18T00:11:28Z",
         "eventSource":"events.amazonaws.com",
         "eventName":"PutRule",
         "awsRegion":"us-east-1",
         "sourceIPAddress":"AWS Internal",
         "userAgent":"AWS CloudWatch Console",
         "requestParameters":{
            "description":"",
            "name":"cttest2",
            "state":"ENABLED",
            "eventPattern":"{\"source\":[\"aws.ec2\"],\"detail-type\":[\"EC2 Instance State-change Notification\"]}",
            "scheduleExpression":""
         },
         "responseElements":{
            "ruleArn":"arn:aws:events:us-east-1:123456789012:rule/cttest2"
         },
         "requestID":"e9caf887-8d88-11e5-a331-3332aa445952",
         "eventID":"49d14f36-6450-44a5-a501-b0fdcdfaeb98",
         "eventType":"AwsApiCall",
         "apiVersion":"2015-10-07",
         "recipientAccountId":"123456789012"
}
```

The following log file entry shows that a user called the CloudWatch Logs **CreateExportTask** action\.

```
{
        "eventVersion": "1.03",
        "userIdentity": {
            "type": "IAMUser",
            "principalId": "EX_PRINCIPAL_ID",
            "arn": "arn:aws:iam::123456789012:user/someuser",
            "accountId": "123456789012",
            "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
            "userName": "someuser"
        },
        "eventTime": "2016-02-08T06:35:14Z",
        "eventSource": "logs.amazonaws.com",
        "eventName": "CreateExportTask",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "127.0.0.1",
        "userAgent": "aws-sdk-ruby2/2.0.0.rc4 ruby/1.9.3 x86_64-linux Seahorse/0.1.0",
        "requestParameters": {
            "destination": "yourdestination",
            "logGroupName": "yourloggroup",
            "to": 123456789012,
            "from": 0,
            "taskName": "yourtask"
        },
        "responseElements": {
            "taskId": "15e5e534-9548-44ab-a221-64d9d2b27b9b"
        },
        "requestID": "1cd74c1c-ce2e-12e6-99a9-8dbb26bd06c9",
        "eventID": "fd072859-bd7c-4865-9e76-8e364e89307c",
        "eventType": "AwsApiCall",
        "apiVersion": "20140328",
        "recipientAccountId": "123456789012"
}
```