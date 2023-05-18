# Limiting a user to viewing specific canaries<a name="CloudWatch_Synthetics_Canaries_Restricted"></a>

You can limit a user's ability to view information about canaries, so that they can only see information about the canaries you specify\. To do this, use an IAM policy with a `Condition` statement similar to the following, and attach this policy to a user or an IAM role\.

The following example limits the user to only viewing information about `name-of-allowed-canary-1` and `name-of-allowed-canary-2`\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "synthetics:DescribeCanaries",
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringEquals": {
                    "synthetics:Names": [
                        "name-of-allowed-canary-1",
                        "name-of-allowed-canary-2"
                    ]
                }
            }
        }
    ]
}
```

CloudWatch Synthetics supports listing as many as five items in the `synthetics:Names` array\.

You can also create a policy that uses a \*** as a wildcard in canary names that are to be allowed, as in the following example:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "synthetics:DescribeCanaries",
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringLike": {
                    "synthetics:Names": [
                        "my-team-canary-*"
                    ]
                }
            }
        }
    ]
}
```

Any user signed in with one of these policies attached can't use the CloudWatch console to view any canary information\. They can view canary information only for the canaries authorized by the policy and only by using the [DescribeCanaries](https://docs.aws.amazon.com/AmazonSynthetics/latest/APIReference/API_DescribeCanaries.html) API or the [describe\-canaries](https://docs.aws.amazon.com/cli/latest/reference/synthetics/describe-canaries.html) AWS CLI command\.