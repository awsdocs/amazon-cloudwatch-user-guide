# Troubleshooting and known issues<a name="Lambda-Insights-Troubleshooting"></a>

The first step to troubleshooting any issues is to enable debug logging on the Lambda Insights extension\. To do this, set the following environment variable on your Lambda function: `LAMBDA_INSIGHTS_LOG_LEVEL=info`\. For more information, see [ Using AWS Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html)\.

The extension emits logs into the same log group as your function \(`/aws/lambda/function-name)`\. Review those logs to see if the error might be related to a setup issue\. 

## I don't see any metrics from Lambda Insights<a name="Lambda-Insights-Troubleshooting-nometrics"></a>

If you don't see Lambda Insights metrics that you expect to see, check the following possibilities:
+ **The metrics might just be delayed**— If the function has not yet been invoked or the data has not been flushed yet, you won't see the metrics in CloudWatch\. For more information, see **Known Issues** later in this section\.
+ **Confirm that the Lambda function has the correct permissions**—Make sure that the **CloudWatchLambdaInsightsExecutionRolePolicy** IAM policy is assigned to the function's execution role\.
+ **Check the Lambda runtime**—Lambda Insights supports only certain Lambda runtimes\. For a list of supported runtimes, see [Using Lambda Insights](Lambda-Insights.md)\.

  For example, to use Lambda Insights on Java 8, you must use the `java8.al2` runtime, not the `java8` runtime\.
+ **Check network access**—The Lambda function might be on a VPC private subnet with no internet access and you don't have a VPC endpoint configured for CloudWatch Logs\. To help debug this issue, you can set the environment variable `LAMBDA_INSIGHTS_LOG_LEVEL=info`\.

## Known issues<a name="Lambda-Insights-Troubleshooting-knownissues"></a>

Data delay can be as high as 20 minutes\. When a function handler completes, Lambda freezes the sandbox, which also freezes the Lambda Insights extension\. While the function is running, we use an adaptive batching strategy based on the function TPS to output data\. However, if the function stops being invoked for an extended period and there is still event data in the buffer, this data can be delayed until Lambda shuts down the idle sandbox\. When Lambda shuts down the sandbox, we flush the buffered data\.