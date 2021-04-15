# Setting up a metric stream<a name="CloudWatch-metric-streams-setup"></a>

Use the steps in the following sections to set up a CloudWatch metric stream\.

After a metric stream is created, the time it takes for metric data to appear at the destination depends on the configured buffering settings on the Kinesis Data Firehose delivery stream\. The buffering is expressed in maximum payload size or maximum wait time, whichever is reached first\. If these are set to the minimum values \(60 seconds, 1MB\) the expected latency is within 3 minutes if the selected CloudWatch namespaces have active metric updates\.

In a CloudWatch metric stream, data is sent every minute\. Data might arrive at the final destination out of order\. All metrics in the specified namespaces are sent in the metric stream, except metrics with a timestamp that is more than two hours old\. 

To create and manage metric streams, you must be logged on to an account that has the **CloudWatchFullAccess** policy and the `iam:PassRole` permission, or an account that has the following list of permissions:
+ `iam:PassRole`
+ `cloudwatch:PutMetricStream`
+ `cloudwatch:DeleteMetricStream`
+ `cloudwatch:GetMetricStream`
+ `cloudwatch:ListMetricStreams`
+ `cloudwatch:StartMetricStreams`
+ `cloudwatch:StopMetricStreams`

If you're going to have CloudWatch set up the IAM role needed for metric streams, you must also have the `iam:CreateRole` and `iam:PutRolePolicy` permissions\.

**Important**  
A user with the `cloudwatch:PutMetricStream` has access the CloudWatch metric data that is being streamed, even if they don't have the `cloudwatch:GetMetricData` permission\.

**Topics**
+ [Setting up a metric stream to an AWS service \(data lake scenario\)](CloudWatch-metric-streams-setup-datalake.md)
+ [Setting up a metric stream to a third\-party solution](CloudWatch-metric-streams-setup-thirdparty.md)