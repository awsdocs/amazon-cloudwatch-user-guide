# AWS Billing and Cost Management Dimensions and Metrics<a name="billing-metricscollected"></a>

The AWS Billing and Cost Management service sends metrics to CloudWatch\. For more information, see [Monitoring Charges with Alerts and Notifications](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/monitor-charges.html) in the *AWS Billing and Cost Management User Guide*\.

## AWS Billing and Cost Management Metrics<a name="billing-metrics"></a>

The `AWS/Billing` namespace includes the following metrics\.


| Metric | Description | 
| --- | --- | 
| `EstimatedCharges` |  The estimated charges for your AWS usage\. This can either be estimated charges for one service or a roll\-up of estimated charges for all services\.  | 

## Dimensions for AWS Billing and Cost Management Metrics<a name="billing-metricdimensions"></a>

Billing and Cost Management supports filtering metrics by the following dimensions\.


| Dimension | Description | 
| --- | --- | 
| `ServiceName` |  The name of the AWS service\. This dimension is omitted for the total of estimated charges across all services\.  | 
| `LinkedAccount` |  The linked account number\. This is used for consolidated billing only\. This dimension is included only for accounts that are linked to a separate paying account in a consolidated billing relationship\. It is not included for accounts that are not linked to a consolidated billing paying account\.  | 
| `Currency` |  The monetary currency to bill the account\. This dimension is required\. Unit: USD  | 