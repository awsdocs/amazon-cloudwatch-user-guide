# ServiceLens troubleshooting<a name="servicelens_troubleshooting"></a>

The following sections can help if you're having issues with CloudWatch ServiceLens\.

## I don't see all my logs<a name="servicelens_troubleshooting_Nologs"></a>

How to configure logs to appear in ServiceLens depends on the service\.
+ API Gateway logs appear if logging is turned on in API Gateway\.
+ Amazon ECS and Amazon EKS logs appear if you are using the latest versions of the X\-Ray SDK and the CloudWatch agent\. For more information, see [Deploying ServiceLens](deploy_servicelens.md)\.
+ Lambda logs appear if the request ID is in the log entry\. This happens automatically for the situations listed in the following table\. For other cases, where the runtime does not automatically include the trace ID, you can manually include the trace ID\.


| Runtime | Method | Request ID automatically in log entry? | 
| --- | --- | --- | 
|  Java  |  context\.getLogger\.log aws\-lambda\-java\-log4j2  |  Yes  | 
|  Java  |  System\.out\.println  |  No  | 
|  Python  |  context\.log logging\.info/error/log/etc\.\.\.  |  Yes  | 
|  Python  |  print  |  No  | 
|  Node\.js  |  context\.log console\.log/info/error/etc\.\.\.  |  Yes  | 
|  dotnet  |  context\.Logger\.log Console\.WriteLine\(\)  |  No  | 
|  Go  |  fmt\.Printf log\.Print  |  No  | 
|  Ruby  |  puts  |  No  | 

## I don't see all my alarms on the service map<a name="servicelens_troubleshooting_NoAlarms"></a>

ServiceLens shows only the alert icon for a node if any alarms associated with that node are in the ALARM state\.

ServiceLens associates alarms with nodes using the following logic:
+ If the node represents an AWS service, then all alarms with the namespace associated with that service are associated with the node\. For example, a node of type `AWS::Kinesis` is linked with all alarms that are based on metrics in the CloudWatch namespace `AWS/Kinesis`\.
+ If the node represents an AWS resource, then the alarms on that specific resource are linked\. For example, a node of type `AWS::DynamoDB::Table` with the name “MyTable” is linked to all alarms that are based on a metric with the namespace `AWS/DynamoDB` and have the `TableName` dimension set to `MyTable`\.
+ If the node is of unknown type, which is identified by a dashed border around the name, then no alarms are associated with that node\.

## I don't see some AWS resources on the service map<a name="servicelens_troubleshooting_MissingResources"></a>

For AWS resources to be traced on the service map, the AWS SDK must be captured using the X\-Ray SDK\. For more information about X\-Ray, see [What Is AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html)\.

Not every AWS resource is represented by a dedicated node\. Some AWS services are represented by a single node for all requests to the service\. The following resource types are displayed with a node per resource: 
+ `AWS::DynamoDB::Table`
+ `AWS::Lambda::Function`

  Lambda functions are represented by two nodes— one for the Lambda Container, and one for the function\. This helps to identify cold start problems with Lambda functions\. Lambda container nodes are associated with alarms and dashboards in the same way as Lambda function nodes\.
+ `AWS::ApiGateway::Stage`
+ `AWS::SQS::Queue`
+ `AWS::SNS::Topic`

## There are too many nodes on my service map<a name="servicelens_troubleshooting_MapTooBig"></a>

Use X\-Ray groups to break your map into multiple maps\. For more information, see [ Using Filter Expressions with Groups](https://docs.aws.amazon.com/xray/latest/devguide/xray-console-filters.html#groups)\.