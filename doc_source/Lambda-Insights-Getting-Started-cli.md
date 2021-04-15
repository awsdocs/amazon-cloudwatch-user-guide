# Using the AWS CLI to enable Lambda Insights on an existing Lambda function<a name="Lambda-Insights-Getting-Started-cli"></a>

Follow these steps to use the AWS CLI to enable Lambda Insights on an existing Lambda function\.

**Step 1: Update function permissions**

**To update the function's permissions**
+ Attach the **CloudWatchLambdaInsightsExecutionRolePolicy** managed IAM policy to the function's execution role by entering the following command\. 

  ```
  aws iam attach-role-policy \
  --role-name function-execution-role \
  --policy-arn "arn:aws:iam::aws:policy/CloudWatchLambdaInsightsExecutionRolePolicy"
  ```

**Step 2: Install the Lambda extension**

Install the Lambda extension by entering the following command\. Replace the ARN value for the `layers` parameter with the ARN matching your Region and the extension version that you want to use\. For more information, see [Available versions of the Lambda Insights extension](Lambda-Insights-extension-versions.md)\.

```
aws lambda update-function-configuration \
   --function-name function-name \
   --layers "arn:aws:lambda:us-west-1:580247275435:layer:LambdaInsightsExtension:14"
```

**Step 3: Enable the CloudWatch Logs VPC endpoint**

This step is necessary only for functions running in a private subnet with no internet access, and if you have not already configured a CloudWatch Logs virtual private cloud \(VPC\) endpoint\.

If you need to do this step, enter the following command, replacing the placeholders with information for your VPC\.

For more information, see [ Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html)\.

```
aws ec2 create-vpc-endpoint \
--vpc-id vpcId \
--vpc-endpoint-type Interface \
--service-name com.amazonaws.region.logs \
--subnet-id subnetId 
--security-group-id securitygroupId
```