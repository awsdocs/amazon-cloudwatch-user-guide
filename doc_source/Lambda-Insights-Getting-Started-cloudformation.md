# Using AWS CloudFormation to enable Lambda Insights on an existing Lambda function<a name="Lambda-Insights-Getting-Started-cloudformation"></a>

Follow these steps to use AWS CloudFormation to enable Lambda Insights on an existing Lambda function\.

**Step 1: Install the layer**

Add the Lambda Insights layer to the `Layers` property within the Lambda Insights layer ARN\. The example below uses the layer for the initial release of Lambda Insights\. For the latest release version of the Lambda Insights extension layer, see [Available versions of the Lambda Insights extension](Lambda-Insights-extension-versions.md)\.

```
Resources:
  MyFunction:
    Type: AWS::Lambda::Function
    Properties:
      Layers:
        - !Sub "arn:aws:lambda:${AWS::Region}:580247275435:layer:LambdaInsightsExtension:14"
```

**Step 2: Add the managed policy**

Add the **CloudWatchLambdaInsightsExecutionRolePolicy** IAM policy to your function execution role\.

```
Resources:
  MyFunctionExecutionRole:
    Type: 'AWS::IAM::Role'
    Properties:
      ManagedPolicyArns:
        - 'arn:aws:iam::aws:policy/CloudWatchLambdaInsightsExecutionRolePolicy'
```

**Step 3: \(Optional\) Add VPC endpoint**

This step is necessary only for functions running in a private subnet with no internet access, and if you have not already configured a CloudWatch Logs virtual private cloud \(VPC\) endpoint\. For more information, see [ Using CloudWatch Logs with Interface VPC Endpoints](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html)\.

```
Resources:
  CloudWatchLogsVpcPrivateEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      PrivateDnsEnabled: 'true'
      VpcEndpointType: Interface
      VpcId: !Ref: VPC
      ServiceName: !Sub com.amazonaws.${AWS::Region}.logs
      SecurityGroupIds:
        - !Ref InterfaceVpcEndpointSecurityGroup
      SubnetIds:
        - !Ref PublicSubnet01
        - !Ref PublicSubnet02
        - !Ref PublicSubnet03
```