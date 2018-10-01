# AWS Key Management Service Metrics and Dimensions<a name="kms-metricscollected"></a>

When you use AWS Key Management Service \(AWS KMS\) to [import key material](https://docs.aws.amazon.com/kms/latest/developerguide/importing-keys.html) into a customer master key \(CMK\) and set it to expire, AWS KMS sends metrics and dimensions to CloudWatch\. For more information, see [Monitoring with Amazon CloudWatch](https://docs.aws.amazon.com/kms/latest/developerguide/monitoring-cloudwatch.html) in the *AWS Key Management Service Developer Guide*\.

## AWS KMS Metrics<a name="kms-metrics"></a>

The `AWS/KMS` namespace includes the following metrics\.

**SecondsUntilKeyMaterialExpiration**  
This metric tracks the number of seconds remaining until imported key material expires\. This metric is valid only for CMKs whose origin is `EXTERNAL` and whose key material is or was set to expire\. The most useful statistic for this metric is `Minimum`, which tells you the smallest amount of time remaining for all data points in the specified statistic period\. The only valid unit for this metric is `Seconds`\.  
Use this metric to track the amount of time that remains until your imported key material expires\. When that amount of time falls below a threshold that you define, you might want to take action such as reimporting the key material with a new expiration date\. You can create a CloudWatch alarm to notify you when that happens\. For more information, see [Creating CloudWatch Alarms to Monitor AWS KMS Metrics](https://docs.aws.amazon.com/kms/latest/developerguide/monitoring-cloudwatch.html#creating-alarms) in the *AWS Key Management Service Developer Guide*\.

## Dimensions for AWS KMS Metrics<a name="kms-dimensions"></a>

AWS KMS metrics use the `AWS/KMS` namespace and have only one valid dimension: `KeyId`\. You can use this dimension to view metric data for a specific CMK or set of CMKs\.