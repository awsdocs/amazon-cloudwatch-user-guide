# Using Serverless Framework to enable Lambda Insights on an existing Lambda function<a name="Lambda-Insights-Getting-Started-serverless"></a>

Follow these steps to use Serverless Framework to enable Lambda Insights on an existing Lambda function\. For more information about Serverless Framework, see [serverless\.com](https://www.serverless.com/)\.

This is done through a Lambda Insights plugin for Serverless\. For more information, see [serverless\-plugin\-lambda\-insights](https://www.npmjs.com/package/serverless-plugin-lambda-insights)\.

If you don't already have the latest version of the Serverless command\-line interface installed, you must first install or upgrade it\. For more information, see [Get started with Serverless Framework Open Source & AWS](https://www.serverless.com/framework/docs/getting-started/)\.

**To use Serverless Framework to enable Lambda Insights on a Lambda function**

1. Install the Serverless plugin for Lambda Insights by running the following command in your Serverless directory:

   ```
   npm install --save-dev serverless-plugin-lambda-insights
   ```

1. In your `serverless.yml` file, add the plugin in the `plugins` section as shown:

   ```
   provider:
     name: aws
   plugins:
     - serverless-plugin-lambda-insights
   ```

1. Enable Lambda Insights\.
   + You can enable Lambda Insights individually per function by adding the following property to the serverless\.yml file

     ```
     functions:
       myLambdaFunction:
         handler: src/app/index.handler
         lambdaInsights: true #enables Lambda Insights for this function
     ```
   + You can enable Lambda Insights for all functions within the `serverless.yml` file by adding the following custom section:

     ```
     custom:
       lambdaInsights:
         defaultLambdaInsights: true #enables Lambda Insights for all functions
     ```

1. Re\-deploy the Serverless service by entering the following command:

   ```
   serverless deploy
   ```

   This re\-deploys all functions and enables Lambda Insights for those functions that you have specified\. It enables Lambda Insights by adding the Lambda Insights layer and attaching the necessary permissions using the `arn:aws:iam::aws:policy/CloudWatchLambdaInsightsExecutionRolePolicy` IAM policy\.