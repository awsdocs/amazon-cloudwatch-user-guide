# Use client\-side evaluation \- powered by AWS AppConfig<a name="CloudWatch-Evidently-client-side-evaluation"></a>

You can use *client\-side evaluation \- powered by AWS AppConfig \(client\-side evaluation\)* in a project, which lets your application assign variations to user sessions locally instead of assigning variations by calling the [EvaluateFeature](https://docs.aws.amazon.com/cloudwatchevidently/latest/APIReference/API_EvaluateFeature.html) operation\. This mitigates the latency and availability risks that come with an API call\.

To use client\-side evaluation, attach the AWS AppConfig Lambda extension as a layer to your Lambda functions and configure the environment variables\. The client\-side evaluation runs as a side process on the local host\. Then, you can call the **EvaluationFeature** and **PutProjectEvent** operations against `localhost`\. The client\-side evaluation process handles the variation assignment, caching, and data synchronization\. For more information about AWS AppConfig, see [ How AWS AppConfig works](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html#learn-more-appconfig-how-it-works)\.

When you integrate with AWS AppConfig, you specify an AWS AppConfig *application ID* and an AWS AppConfig *environment ID* to Evidently\. You can use the same application ID and environment ID across Evidently projects\.

When you create a project with client\-side evaluation enabled, Evidently creates an AWS AppConfig configuration profile for that project\. The configuration profile for each project will be different\.

## Client\-side evaluation access control<a name="CloudWatch-Evidently-client-side-evaluation-access"></a>

Evidently client\-side evaluation uses a different access control mechanism than the rest of Evidently does\. We strongly recommend that you understand this so that you can implement the proper security measures\.

With Evidently, you can create IAM policies that limit the actions a user can perform on individual resources\. For example, you can create a user role that disallows a user from having the **EvaluateFeature** action\. For more information about the Evidently actions that can be controlled with IAM policies, see [Actions defined by Amazon CloudWatch Evidently](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazoncloudwatchevidently.html#amazoncloudwatchevidently-actions-as-permissions)\.

The client\-side evaluation model allows local evaluations of Evidently features that use project metadata\. A user of a project with client\-side evaluation enabled can call the **EvaluateFeature** API against a local host endpoint, and this API call does not reach Evidently and is not authenticated by the Evidently service's IAM policies\. This call is successful even if the user doesn't have the IAM permission to use the **EvaluateFeature** action\. However, a user still needs the **PutProjectEvents** permission for the agent to buffer the evaluation events or custom events and to offload data to Evidently asynchronously\.

Additionally, a user must have the `evidently:ExportProjectAsConfiguration` permission to be able to create a project that uses client\-side evaluation\. This helps you control access to **EvaluateFeature** actions that are called during client\-side evaluation\.

If you aren't careful, the client\-side evaluation security model can subvert the policies that you have set on the rest of Evidently\. A user who has the `evidently:ExportProjectAsConfiguration` permission can create a project with client\-side evaluation enabled, and then use the **EvaluateFeature** action for client\-side evaluation with that project even if they are expressly denied the **EvaluateFeature** action in an IAM policy\.

## Get started with Lambda<a name="CloudWatch-Evidently-client-side-evaluation-lambda"></a>

Evidently currently supports client\-side evaluation by using an AWS Lambda environment\. To get started, first decide which AWS AppConfig application and environment to use\. Choose an existing application and environment, or create new ones\.

The following sample AWS AppConfig AWS CLI commands create an application and environment\.

```
aws appconfig create-application --name YOUR_APP_NAME
```

```
aws appconfig create-environment --application-id YOUR_APP_ID --name YOUR_ENVIRONMENT_NAME
```

Next, create an Evidently project by using these AWS AppConfig resources\. For more information, see [Create a new project](CloudWatch-Evidently-newproject.md)\.

Client\-side evaluation is supported in Lambda by using a Lambda layer\. This is a public layer that is part of `AWS-AppConfig-Extension`, a public AWS AppConfig extension created by the AWS AppConfig service\. For more information about Lambda layers, see [ Layer](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-concepts.html#gettingstarted-concepts-layer)\.

To use client\-side evaluation, you must add this layer to your Lambda function and configure permissions and environment variables\.

**To add the Evidently client\-side evaluation Lambda layer to your Lambda function and configure it**

1. Create a Lambda function if you haven't already\.

1. Add the client\-side evaluation layer to your function\. You can either specify its ARN or select it from the list of AWS layers if you haven't already\. For more information, see [ Configuring functions to use layers](https://docs.aws.amazon.com/lambda/latest/dg/invocation-layers.html#invocation-layers-using) and [ Available versions of the AWS AppConfig Lambda extension](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-integration-lambda-extensions-versions.html)\.

1. Create an IAM policy named **EvidentlyAppConfigCachingAgentPolicy** with the following contents, and attach it to the function's execution role\. For more information, see [ Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": [
                   "appconfig:GetLatestConfiguration",
                   "appconfig:StartConfigurationSession",
                   "evidently:PutProjectEvents"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Add the required environment variable `AWS_APPCONFIG_EXTENSION_EVIDENTLY_CONFIGURATIONS` to your Lambda function\. This environment variable specifies the mapping between the Evidently project and the AWS AppConfig resources\.

   If you are using this function for one Evidently project, set the value of the environment variable to: `applications/APP_ID/environments/ENVIRONMENT_ID/configurations/PROJECT_NAME`

   If you are using this function for multiple Evidently projects, use a comma to separate the values, as in the following example: `applications/APP_ID_1/environments/ENVIRONMENT_ID_1/configurations/PROJECT_NAME_1, applications/APP_ID_2/environments/ENVIRONMENT_ID_2/configurations/PROJECT_NAME_2`

1. \(Optional\) Set other environment variables\. For more information, see [ Configuring the AWS AppConfig Lambda extension](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-integration-lambda-extensions.html#appconfig-integration-lambda-extensions-config)\.

1. In your application, get Evidently evaluations locally by sending `EvaluateFeature` to `localhost`\.

   Python example:

   ```
   import boto3
   from botocore.config import Config
           
   def lambda_handler(event, context):
       local_client = boto3.client(
           'evidently',
           endpoint_url="http://localhost:2772",
           config=Config(inject_host_prefix=False)
       )
       response = local_client.evaluate_feature(
           project=event['project'],
           feature=event['feature'],
           entityId=event['entityId']
       )
       print(response)
   ```

   Node\.js example:

   ```
   const AWS = require('aws-sdk');
   const evidently = new AWS.Evidently({
       region: "us-west-2",
       endpoint: "http://localhost:2772",
       hostPrefixEnabled: false
   });
   
   exports.handler = async (event) => {
       
       const evaluation = await evidently.evaluateFeature({
           project: 'John_ETCProject_Aug2022',
           feature: 'Feature_IceCreamFlavors',
           entityId: 'John'
       }).promise()
       
       console.log(evaluation)
       const response = {
           statusCode: 200,
           body: evaluation,
       };
       return response;
   };
   ```

   Kotlin example:

   ```
   String localhostEndpoint = "http://localhost:2772/"
   public AmazonCloudWatchEvidentlyClient getEvidentlyLocalClient() {
       return AmazonCloudWatchEvidentlyClientBuilder.standard()
           .withEndpointConfiguration(AwsClientBuilder.EndpointConfiguration(localhostEndpoint, region))
           .withClientConfiguration(ClientConfiguration().withDisableHostPrefixInjection(true))
           .withCredentials(credentialsProvider)
           .build();
   }
   
   AmazonCloudWatchEvidentlyClient evidently = getEvidentlyLocalClient();
   
   // EvaluateFeature via local client.
   EvaluateFeatureRequest evaluateFeatureRequest = new EvaluateFeatureRequest().builder()
      .withProject(${YOUR_PROJECT}) //Required.
      .withFeature(${YOUR_FEATURE}) //Required.
      .withEntityId(${YOUR_ENTITY_ID}) //Required.
      .withEvaluationContext(${YOUR_EVAL_CONTEXT}) //Optional: a JSON object of attributes that you can optionally pass in as part of the evaluation event sent to Evidently.
      .build();
      
   EvaluateFeatureResponse evaluateFeatureResponse = evidently.evaluateFeature(evaluateFeatureRequest);
   
   // PutProjectEvents via local client. 
   PutProjectEventsRequest putProjectEventsRequest = new PutProjectEventsRequest().builder()
       .withData(${YOUR_DATA})
       .withTimeStamp(${YOUR_TIMESTAMP})
       .withType(${YOUR_TYPE})
       .build();
       
   PutProjectEvents putProjectEventsResponse = evidently.putProjectEvents(putProjectEventsRequest);
   ```

### Configure how often the client sends data to Evidently<a name="CloudWatch-Evidently-client-side-evaluation-frequency"></a>

To specify how often client\-side evaluation sends data to Evidently, you can optionally configure two environment variables\.
+ `AWS_APPCONFIG_EXTENSION_EVIDENTLY_EVENT_BATCH_SIZE` specifies the number of events per project to batch before sending them to Evidently\. Valid values are integers between 1 and 50, and the default is 40\.
+ `AWS_APPCONFIG_EXTENSION_EVIDENTLY_BATCH_COLLECTION_DURATION` specifies the duration in seconds to wait for events before sending them to Evidently\. The default is 30\.

### Troubleshooting<a name="CloudWatch-Evidently-client-side-evaluation-lambda-troubleshooting"></a>

Use the following information to help troubleshoot problems with using CloudWatch Evidently with client\-side evaluation \- powered by AWS AppConfig\. 

#### An error occurred \(BadRequestException\) when calling the EvaluateFeature operation: HTTP method not supported for provided path<a name="Evidently-client-side-evaluation-troubleshooting-BadRequestException"></a>

Your environment variables might be configured incorrectly\. For example, you might have used `EVIDENTLY_CONFIGURATIONS` as the environment variable name instead of `AWS_APPCONFIG_EXTENSION_EVIDENTLY_CONFIGURATIONS`\.

#### ResourceNotFoundException: Deployment not found<a name="Evidently-client-side-evaluation-troubleshooting-ResourceNotFoundException"></a>

Your update to the project metadata has not been deployed to AWS AppConfig\. Check for an active deployment in the AWS AppConfig environment that you used for client\-side evaluation\.

#### ValidationException: No Evidently configuration for project<a name="Evidently-client-side-evaluation-troubleshooting-ValidationException"></a>

Your `AWS_APPCONFIG_EXTENSION_EVIDENTLY_CONFIGURATIONS` environment variable might be configured with the incorrect project name\.