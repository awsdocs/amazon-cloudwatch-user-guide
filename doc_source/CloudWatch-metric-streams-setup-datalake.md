# Set up a metric stream to an AWS service \(data lake scenario\)<a name="CloudWatch-metric-streams-setup-datalake"></a>

You can use the CloudWatch console, the AWS CLI, AWS CloudFormation, or the AWS Cloud Development Kit \(AWS CDK\) to set up a metric stream\.

The Kinesis Data Firehose delivery stream that you use for your metric stream must be in the same account and the same Region where you set up the metric stream\. To achieve cross\-Region functionality, you can configure the Kinesis Data Firehose delivery stream to stream to a final destination that is in a different account or a different Region\.

## CloudWatch console<a name="CloudWatch-metric-streams-setup-datalake-console"></a>

This section describes how to use the CloudWatch console to set up a metric stream to another AWS service\.

When you use the console to set up a metric stream, you have the option of choosing the **Quick S3 Setup**\. This method works well if want the final output as JSON or to be queried by Amazon Athena\. If you want the final format to be Parquet format or Optimized Row Columnar \(ORC\), you should create your own Kinesis Data Firehose delivery stream and choose **Select an existing Firehose owned by your account** instead of choosing **Quick S3 Setup**\.

**To set up a metric stream to an AWS service**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**, **Streams**\. Then choose **Create metric stream**\.

1. \(Optional\) If you are signed in to an account that is set up as a monitoring account in CloudWatch cross\-account observability, you can choose whether to include metrics from linked source accounts in this metric stream\. To include metrics from source accounts, choose **Include source account metrics**\.

1. Choose the CloudWatch metric namespaces to include in the metric stream\.
   + To include all or most of your metric namespaces in the metric stream, choose **All namespaces**\.

     Then, if you want to exclude some metric namespaces from the stream, choose **Exclude metric namespaces** and select the namespaces to exclude\. If you specify a namespace in **Exclude metric namespaces**, all metrics in that namespace are excluded from the stream\.
   + To include only a few metric namespaces in the metric stream, choose **Selected namespaces** and then select the namespaces to include\.
**Note**  
Consider carefully whether to stream all metrics, because the more metrics that you stream the higher your metric stream charges will be\.

1. \(Optional\) Use the **Select metrics for the metric stream** option to create additional filters for specific metrics\.
   + If you chose to stream **All namespaces**, you can use this section to exclude specific metrics from the stream while still including other metrics from that same namespace in the stream\. To do so, do the following:

     1. Choose **Exclude metrics by metric name**

     1. Specify each metric to exclude by selecting its namespace and then its metric name\.

        After you select one metric, you can repeat this step to exclude another metric from the same namespace or a different one\.
   + If you chose to stream **Selected namespaces**, you can use this section to include only a subset of metrics from those namespace in the metric stream\. To do so, do the following:

     1. Choose **Include metrics by metric name**

     1. Specify each metric to include by selecting its namespace and then its metric name\.

        After you select one metric, you can repeat this step to include another metric from the same namespace or a different one\.

        If you don't select any metrics from one of your included namespaces, then all metric from that namespace are included in the metric stream\.

1. Choose one of the following:
   + For a quick setup, choose **Quick S3 setup**\. CloudWatch will create all necessary resources including the Kinesis Data Firehose delivery stream and the necessary IAM roles\. The default format for this option is JSON, but you can change the format later in this procedure\.
   + To use a Kinesis Data Firehose delivery stream that already exists, choose **Select an existing Firehose owned by your account**\. The Kinesis Data Firehose delivery stream must be in the same account\. The default format for this option is OpenTelemetry, but you can change the format later in this procedure\.

     Then select the Kinesis Data Firehose delivery stream to use under **Select your Kinesis Data Firehose delivery stream**\.

1. \(Optional\) If you're using **Quick S3 Setup**, you can choose **Select existing resources** to use an existing S3 bucket or existing IAM roles instead of having CloudWatch create new ones for you\.

1. \(Optional\) If you're using **Select an existing Firehose owned by your account**, you can choose **Select existing service role** to use an existing IAM role instead of having CloudWatch create a new one for you\.

1. \(Optional\) To change the output format from the default format for your scenario, choose **Change output format**\. The supported formats are JSON and OpenTelemetry 0\.7\.0\.

1. \(Optional\) To stream additional statistics for some of these metrics beyond Minimum, Maximum, Sample count, and Sum, choose **Add additional statistics**\. Either choose **Add recommended metrics** to add some commonly used statistics, or manually select the namespace and metric name to stream additional statistics for\. Next, select the additional statistics to stream\.

   To then choose another group of metrics to stream a different set of additional statistics for, choose **Add additional statistics**\. Each metric can include as many as 20 additional statistics, and as many as 100 metrics within a metric stream can include additional statistics\.

   Streaming additional statistics incurs more charges\. For more information, see [Statistics that can be streamed](CloudWatch-metric-streams-statistics.md)\.

   For definitions of the additional statistics, see [CloudWatch statistics definitions](Statistics-definitions.md)\.

1. \(Optional\) Customize the name of the new metric stream under **Metric stream name**\.

1. Choose **Create metric stream**\.

## AWS CLI or AWS API<a name="CloudWatch-metric-streams-setup-datalake-CLI"></a>

Use the following steps to create a CloudWatch metric stream to another AWS service\.

**To use the AWS CLI or AWS API to create a metric stream**

1. If you're streaming to Amazon S3, first create the bucket\. For more information, see [ Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)\.

1. Create the Kinesis Data Firehose delivery stream\. For more information, see [ Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html)\.

1. Create an IAM role that enables CloudWatch to write to the Kinesis Data Firehose delivery stream\. For more information about the contents of this role, see [Trust between CloudWatch and Kinesis Data Firehose](CloudWatch-metric-streams-trustpolicy.md)\.

1. Use the `aws cloudwatch put-metric-stream` CLI command or the `PutMetricStream` API to create the CloudWatch metric stream\.

## AWS CloudFormation<a name="CloudWatch-metric-streams-setup-datalake-CFN"></a>

You can use AWS CloudFormation to set up a metric stream\. For more information, see [ AWS::CloudWatch::MetricStream](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudwatch-metricstream.html)\.

**To use AWS CloudFormation to create a metric stream**

1. If you're streaming to Amazon S3, first create the bucket\. For more information, see [ Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)\.

1. Create the Kinesis Data Firehose delivery stream\. For more information, see [ Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html)\.

1. Create an IAM role that enables CloudWatch to write to the Kinesis Data Firehose delivery stream\. For more information about the contents of this role, see [Trust between CloudWatch and Kinesis Data Firehose](CloudWatch-metric-streams-trustpolicy.md)\.

1. Create the stream in AWS CloudFormation\. For more information, see [ AWS::CloudWatch::MetricStream](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudwatch-metricstream.html)\.

## AWS Cloud Development Kit \(AWS CDK\)<a name="CloudWatch-metric-streams-setup-datalake-CDK"></a>

You can use AWS Cloud Development Kit \(AWS CDK\) to set up a metric stream\. 

**To use the AWS CDK to create a metric stream**

1. If you're streaming to Amazon S3, first create the bucket\. For more information, see [ Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html)\.

1. Create the Kinesis Data Firehose delivery stream\. For more information, see [ Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html)\.

1. Create an IAM role that enables CloudWatch to write to the Kinesis Data Firehose delivery stream\. For more information about the contents of this role, see [Trust between CloudWatch and Kinesis Data Firehose](CloudWatch-metric-streams-trustpolicy.md)\.

1. Create the metric stream\. The metric stream resource is available in AWS CDK as a Level 1 \(L1\) Construct named `CfnMetricStream`\. For more information, see [ Using L1 constructs](https://docs.aws.amazon.com/cdk/latest/guide/constructs.html#constructs_l1_using.html)\.