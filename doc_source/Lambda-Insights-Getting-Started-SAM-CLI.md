# Using the AWS SAM CLI to enable Lambda Insights on an existing Lambda function<a name="Lambda-Insights-Getting-Started-SAM-CLI"></a>

Follow these steps to use the AWS SAM AWS CLI to enable Lambda Insights on an existing Lambda function\.

If you don't already have the latest version of the AWS SAM CLI installed, you must first install or upgrade it\. For more information, see [ Installing the AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)\.

**Step 1: Install the layer**

To make the Lambda Insights extension available to all of your Lambda functions, add a `Layers` property to the `Globals` section of your SAM template with the ARN of the Lambda Insights layer\. The example below uses the layer for the initial release of Lambda Insights\. For the latest release version of the Lambda Insights extension layer, see [Available versions of the Lambda Insights extension](Lambda-Insights-extension-versions.md)\.

```
Globals:
  Function:
    Layers:
      - !Sub "arn:aws:lambda:${AWS::Region}:580247275435:layer:LambdaInsightsExtension:14"
```

To enable this layer for only a single function, add the `Layers` property to the function as shown in this example\.

```
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Layers:
        - !Sub "arn:aws:lambda:${AWS::Region}:580247275435:layer:LambdaInsightsExtension:14"
```

**Step 2: Add the managed policy**

For each function, add the **CloudWatchLambdaInsightsExecutionRolePolicy** IAM policy\.

AWS SAM doesn't support global policies, so you must enable those on each function individually, as shown in this example\. For more information about globals, see [ Globals Section](https://github.com/aws/serverless-application-model/blob/master/docs/globals.rst)\. 

```
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Policies:
        - CloudWatchLambdaInsightsExecutionRolePolicy
```

**Invoking locally**

The AWS SAM CLI supports Lambda extensions\. However, every locally executed invocation resets the runtime environment\. Lambda Insights data won’t be available from local invocations because the runtime is restarted without a shutdown event\. For more information, see [ Release 1\.6\.0 \- Add support for local testing of AWS Lambda extensions](https://github.com/aws/aws-sam-cli/releases/tag/v1.6.0)\.

**Troubleshooting**

To troubleshoot your Lambda Insights installation, add the following environment variable to your Lambda function to enable debug logging\.

```
Resources:
  MyFunction:
    Type: AWS::Serverless::Function
    Properties:
      Environment:
        Variables:
          LAMBDA_INSIGHTS_LOG_LEVEL: info
```
