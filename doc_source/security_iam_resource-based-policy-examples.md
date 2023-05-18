# Resource\-based policy examples for Amazon CloudWatch<a name="security_iam_resource-based-policy-examples"></a>

To learn how to create a *widget*, see ***ADD XREF TO YOUR DOCS HERE***\.

**Topics**
+ [Restricting Amazon S3 bucket access to specific IP addresses](#security_iam_resource-based-policy-examples-restrict-bucket-by-ip)

## Restricting Amazon S3 bucket access to specific IP addresses<a name="security_iam_resource-based-policy-examples-restrict-bucket-by-ip"></a>

The following example grants permissions to any user to perform any Amazon S3 operations on objects in the specified bucket\. However, the request must originate from the range of IP addresses specified in the condition\.

The condition in this statement identifies the 54\.240\.143\.\* range of allowed Internet Protocol version 4 \(IPv4\) IP addresses, with one exception: 54\.240\.143\.188\.

The `Condition` block uses the `IpAddress` and `NotIpAddress` conditions and the `aws:SourceIp` condition key, which is an AWS\-wide condition key\. For more information about these condition keys, see [Specifying conditions in a policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/amazon-s3-policy-keys.html)\. The`aws:sourceIp` IPv4 values use the standard CIDR notation\. For more information, see [IP address condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_IPAddress) in the *IAM User Guide*\.

```
{
  "Version": "2012-10-17",
  "Id": "S3PolicyId1",
  "Statement": [
    {
      "Sid": "IPAllow",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": "arn:aws:s3:::examplebucket/*",
      "Condition": {
         "IpAddress": {"aws:SourceIp": "54.240.143.0/24"},
         "NotIpAddress": {"aws:SourceIp": "54.240.143.188/32"} 
      } 
    } 
  ]
}
```