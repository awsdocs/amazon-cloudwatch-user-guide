# Running a canary on a VPC<a name="CloudWatch_Synthetics_Canaries_VPC"></a>

You can run canaries on endpoints on a VPC and public internal endpoints\. To run a canary on a VPC, you must have both the **DNS Resolution** and ** DNS hostnames** options enabled on the VPC\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html)\.

When you run a canary on a VPC endpoint, you must provide a way for it to send its metrics to CloudWatch and its artifacts to Amazon S3\. If the VPC is already enabled for internet access, there's nothing more for you to do\. The canary executes in your VPC, but can access the internet to upload its metrics and artifacts\.

If the VPC is not already enabled for internet access, you have two options:
+ Enable it for internet access\. For more information, see [How do I give internet access to my Lambda function in a VPC?](https://aws.amazon.com/premiumsupport/knowledge-center/internet-access-lambda-function/) on AWS Support\.
+ If you want to keep your VPC private, you can configure the canary to send its data to CloudWatch and Amazon S3 through private VPC endpoints\. If you have not already done so, you must create endpoints for these services on your VPC\. For more information, see [Using CloudWatch and CloudWatch Synthetics with Interface VPC Endpoints](cloudwatch-and-interface-VPC.md) and [Amazon VPC Endpoints for Amazon S3](https://docs.aws.amazon.com/glue/latest/dg/vpc-endpoints-s3.html)\. 