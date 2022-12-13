# Using the AWS CDK to enable Lambda Insights on an existing Lambda function<a name="Lambda-Insights-Getting-Started-clouddevelopmentkit"></a>

Follow these steps to use the AWS CDK to enable Lambda Insights on an existing Lambda function\. To use these steps, you must already be using the AWS CDK to manage your resources\.

The commands in this section are in TypeScript\.

First, update the function permissions\.

```
executionRole.addManagedPolicy(
 ManagedPolicy.fromAwsManagedPolicyName('CloudWatchLambdaInsightsExecutionRolePolicy')
);
```

Next, install the extension on the Lambda function\. Replace the ARN value for the `layerArn` parameter with the ARN matching your Region and the extension version that you want to use\. For more information, see [Available versions of the Lambda Insights extension](Lambda-Insights-extension-versions.md)\.

```
import lambda = require('@aws-cdk/aws-lambda');
const layerArn = 'arn:aws:lambda:us-west-1:580247275435:layer:LambdaInsightsExtension:14';
const layer = lambda.LayerVersion.fromLayerVersionArn(this, 'LayerFromArn', layerArn);
```

If necessary, enable the virtual private cloud \(VPC\) endpoint for CloudWatch Logs\. This step is necessary only for functions running in a private subnet with no internet access, and if you have not already configured a CloudWatch Logs VPC endpoint\.

```
const cloudWatchLogsEndpoint = vpc.addInterfaceEndpoint('cwl-gateway', {
  service: InterfaceVpcEndpointAwsService.CLOUDWATCH_LOGS,
});

cloudWatchLogsEndpoint.connections.allowDefaultPortFromAnyIpv4();
```
