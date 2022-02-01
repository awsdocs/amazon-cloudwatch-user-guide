# Project data storage<a name="CloudWatch-Evidently-datastorage"></a>

Evidently collects two types of events:
+ **Evaluation events** are related to which feature variation is assigned to a user session\. Evidently uses these events to produce metrics and other experiment and launch data, which you can view in the Evidently console\. 

  You can also choose to store these evaluation events in Amazon CloudWatch Logs or Amazon S3\.
+ **Custom events** are used to generate metrics from user actions such as clicks and checkouts\. Evidently doesn't provide a method for you to store custom events\. If you want to save them, you must modify your application code to send them to a storage option outside of Evidently\.

## Format of evaluation event logs<a name="CloudWatch-Evidently-datastorage-logformat"></a>

If you choose to store evaluation events in CloudWatch Logs or Amazon S3, each evaluation event is stored as a log event with the following format:

```
{
    "event_timestamp": 1642624900215,
    "event_type": "evaluation",
    "version": "1.0.0",
    "project_arn": "arn:aws:evidently:us-east-1:123456789012:project/petfood",
    "feature": "petfood-upsell-text",
    "variation": "Variation1",
    "entity_id": "7",
    "entity_attributes": {},
    "evaluation_type": "EXPERIMENT_RULE_MATCH",
    "treatment": "Variation1",
    "experiment": "petfood-experiment-2"
}
```

Here are more details about the preceding evaluation event format:
+ The timestamp is in UNIX time with milliseconds 
+ The variation is the name of the variation of the feature which was assigned to this user session\.
+ The entity ID is a string\.
+ Entity attributes are a hash of arbitrary values sent by the client\. For example, if the `entityId` is mapped to blue or green, then you can optionally send userIDs, session data, or whatever else that you want want from a correlation and data warehouse perspective\. 

## IAM policy and encryption for evaluation event storage in Amazon S3<a name="CloudWatch-Evidently-datastorage-s3"></a>

If you choose to use Amazon S3 to store evaluation events, you must add an IAM policy like the following to allow Evidently to publish logs to the Amazon S3 bucket\. This is because Amazon S3 buckets and the objects they contain are private, and they don't allow access to other services by default\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSLogDeliveryWrite",
            "Effect": "Allow",
            "Principal": {"Service": "delivery.logs.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::bucket_name/optional_folder/AWSLogs/account_id/*",
            "Condition": {"StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}}
        },
        {
            "Sid": "AWSLogDeliveryCheck",
            "Effect": "Allow",
            "Principal": {"Service": "delivery.logs.amazonaws.com"},
            "Action": ["s3:GetBucketAcl", "s3:ListBucket"],
            "Resource": "arn:aws:s3:::bucket_name"
        }
    ]
}
```

If you store Evidently data in Amazon S3, you can also choose to encrypt it with Server\-Side Encryption with AWS Key Management Service Keys \(SSE\-KMS\)\. For more information, see [ Protecting data using server\-side encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html)\.

If you use a customer managed key from AWS KMS, you must add the following to the IAM policy for your key\. This allows Evidently to write to the bucket\.

```
{
    "Sid": "AllowEvidentlyToUseCustomerManagedKey",
    "Effect": "Allow",
    "Principal": {
        "Service": [
            "delivery.logs.amazonaws.com"
        ]
    },
   "Action": [
       "kms:Encrypt",
       "kms:Decrypt",
       "kms:ReEncrypt*",
       "kms:GenerateDataKey*",
       "kms:DescribeKey"
    ],
    "Resource": "*"
}
```