# Step 1: Authorize your application to send data to AWS<a name="CloudWatch-RUM-get-started-authorization"></a>

To use CloudWatch RUM, your application must have authorization\.

You have three options to set up authorization:
+ Let CloudWatch RUM create a new Amazon Cognito identity pool for the application\. This method requires the least effort to set up\. It's the default option\.

  The identity pool will contain an unauthenticated identity\. This allows the CloudWatch RUM web client to send data to CloudWatch RUM without authenticating the user of the application\.

  The Amazon Cognito identity pool has an attached IAM role\. The Amazon Cognito unauthenticated identity allows the web client to assume the IAM role that is authorized to send data to CloudWatch RUM\.
+ Use an existing Amazon Cognito identity pool\. In this case, you must also modify the IAM role that is attached to the identity pool\.
+ Use authentication from an existing identity provider that you have already set up\. In this case, you must get credentials from the identity provider and your application must forward these credentials to the RUM web client\.

The following sections include more details about these options\.

## CloudWatch RUM creates a new Amazon Cognito identity pool<a name="CloudWatch-RUM-get-started-authorization-newcognito"></a>

This is the simplest option to set up, and if you choose this no further setup steps are required\. You must have administrative permissions to use this option\. For more information, see [IAM policies to use CloudWatch RUM](CloudWatch-RUM-permissions.md)\.

With this option, CloudWatch RUM creates the following resources:
+ A new Amazon Cognito identity pool
+ An unauthenticated Amazon Cognito identity\. This allows the RUM web client to assume an IAM role without authenticating the user of the application\.
+ The IAM role that the RUM web client will assume\. The IAM policy attached to this role allows it to use the `PutRumEvents` API with the app monitor resource\. In other words, it allows the RUM web client to send data to RUM\.

The RUM web client uses the Amazon Cognito identity to obtain AWS credentials\. The AWS credentials are associated with the IAM role\. The IAM role is authorized to use `PutRumEvents` with the AppMonitor resource\.

Amazon Cognito sends the necessary security token to enable your application to send data to CloudWatch RUM\. The JavaScript code snippet that CloudWatch RUM generates includes the following lines to enable authentication\.

```
   {
    identityPoolId: [identity pool id],  // e.g., 'us-west-2:EXAMPLE4a-66f6-4114-902a-EXAMPLEbad7'
    guestRoleArn: [iam role arn]         // e.g., 'arn:aws:iam::123456789012:role/Nexus-Monitor-us-east-1-123456789012_Unauth_5889316876161'
   }
);
```

## Use an existing Amazon Cognito identity pool<a name="CloudWatch-RUM-get-started-authorization-existingcognito"></a>

If you choose to use an existing Amazon Cognito identity pool, you specify the identity pool when you add the application to CloudWatch RUM\. The pool must support enabling access to unauthenticated identities\. You also must add the following permissions to the IAM policy that is attached to the IAM role that is associated with this identity pool\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "rum:PutRumEvents",
            "Resource": "arn:aws:rum:[region]:[accountid]:appmonitor/[app monitor name]"
        }
    ]
}
```

Amazon Cognito will then send the necessary security token to enable your application to access CloudWatch RUM\.

## Third\-party provider<a name="CloudWatch-RUM-get-started-authorization-thirdparty"></a>

If you choose to use private authentication from a third\-party provider, you must get credentials from the identity provider and forward them to AWS\. The best way to do this is by using a *security token vendor*\. You can use any security token vendor, including Amazon Cognito with AWS Security Token Service\. For more information about AWS STS, see [Welcome to the AWS Security Token Service API Reference](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)\. 

If you want to use Amazon Cognito as the token vendor in this scenario, you can configure Amazon Cognito to work with an authentication provider\. For more information, see [Getting Started with Amazon Cognito Identity Pools \(Federated Identities\)](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html)\.

After you configure Amazon Cognito to work with your identity provider, you also need to do the following:
+ Create an IAM role with the following permissions\. Your application will use this role to access AWS\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "rum:PutRumEvents",
              "Resource": "arn:aws:rum:[region]:[accountID]:appmonitor/[app monitor name]"
          }
      ]
  }
  ```
+ Add the following to your application to have it pass the credentials from your provider to CloudWatch RUM\. Insert the line so that it runs after a user has signed in to your application and the application has received the credentials to use to access AWS\.

  ```
  cwr('setAwsCredentials', {/* Credentials or CredentialProvider */});
  ```

For more information about credential providers in the AWS JavaScript SDK, see [Setting credentials in a web browser](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-credentials-browser.html) in the v3 developer guide for SDK for JavaScript, [Setting credentials in a web browser](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-credentials-browser.html) in the v2 developer guide for SDK for JavaScript, , and [@aws\-sdk/credential\-providers](https://www.npmjs.com/package/@aws-sdk/credential-providers)\. 

You can also use the SDK for the CloudWatch RUM web client to configure the web client authentication methods\. For more information about the web client SDK, see [CloudWatch RUM web client SDK](https://github.com/aws-observability/aws-rum-web)\. 