# Using condition keys to limit alarm actions<a name="iam-cw-condition-keys-alarm-actions"></a>

When CloudWatch alarms change state, they can perform different actions such as stopping and terminating EC2 instances and performing Systems Manager actions\. These actions can be initiated when the alarm changes to any state, including ALARM, OK, or INSUFFICIENT\_DATA\.

Use the `cloudwatch:AlarmActions` condition key to allow a user to create alarms that can only perform the actions you specify when the alarm state changes\. For example, you can allow a user to create alarms that can only perform actions which are not EC2 actions\.

**Allow a user to create alarms that can only send Amazon SNS notifications or perform Systems Manager actions**

The following policy limits the user to creating alarms that can only send Amazon SNS notifications and perform Systems Manager actions\. The user can't create alarms that perform EC2 actions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "CreateAlarmsThatCanPerformOnlySNSandSSMActions",
            "Effect": "Allow",
            "Action": "cloudwatch:PutMetricAlarm",
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringLike": {
                    "cloudwatch:AlarmActions": [
                        "arn:aws:sns:*",
                        "arn:aws:ssm:*"
                    ]
                }
            }
        }
    ]
}
```