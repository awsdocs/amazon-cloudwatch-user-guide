# Enabling Lambda Insights on a Lambda container image deployment<a name="Lambda-Insights-Getting-Started-docker"></a>

To enable Lambda Insights on a Lambda function that is deployed as a container image, add the following lines in your Dockerfile\. These lines install the Lambda Insights agent as an extension in your container image\.

```
RUN curl -O https://lambda-insights-extension.s3-ap-northeast-1.amazonaws.com/amazon_linux/lambda-insights-extension.rpm && \
    rpm -U lambda-insights-extension.rpm && \
    rm -f lambda-insights-extension.rpm
```

**Note**  
The Lambda Insights agent is supported only on Lambda runtimes that use Amazon Linux 2\.

After you create your Lambda function, assign the **CloudWatchLambdaInsightsExecutionRolePolicy** IAM policy to the function's execution role, and Lambda Insights is enabled on the container image\-based Lambda function\.

**Note**  
To use an older version of the Lambda Insights extension, replace the URL in the previous commands with this URL: `https://lambda-insights-extension.s3-ap-northeast-1.amazonaws.com/amazon_linux/lambda-insights-extension.1.0.111.0.rpm`\. Currently, only Lambda Insights versions 1\.0\.111\.0 and later are available\. For more information, see [Available versions of the Lambda Insights extension](Lambda-Insights-extension-versions.md)\.

**To verify the signature of the Lambda Insights agent package on a Linux server**

1. Enter the following command to download the public key\.

   ```
   shell$ wget https://lambda-insights-extension.s3-ap-northeast-1.amazonaws.com/lambda-insights-extension.gpg
   ```

1. Enter the following command to import the public key into your keyring\.

   ```
   shell$ gpg --import lambda-insights-extension.gpg
   ```

   The output will be similar to the following\. Make a note of the `key` value, you will need it in the next step\. In this example output, the key value is `848ABDC8`\.

   ```
   gpg: key 848ABDC8: public key "Amazon Lambda Insights Extension" imported
   gpg: Total number processed: 1
   gpg: imported: 1  (RSA: 1)
   ```

1. Verify the fingerprint by entering the following command\. Replace `key-value` with the value of the key from the preceding step\.

   ```
   shell$  gpg --fingerprint key-value
   ```

   The fingerprint string in the output of this command should be `E0AF FA11 FFF3 5BD7 349E E222 479C 97A1 848A BDC8`\. If the string doesn't match, don't install the agent and contact AWS\.

1. After you have verified the fingerprint, you can use it to verify the Lambda Insights agent package\. Download the package signature file by entering the following command\.

   ```
   shell$  wget https://lambda-insights-extension.s3-ap-northeast-1.amazonaws.com/amazon_linux/lambda-insights-extension.rpm.sig
   ```

1. Verify the signature by entering the following command\.

   ```
   shell$ gpg --verify lambda-insights-extension.rpm.sig lambda-insights-extension.rpm
   ```

   The output should look like the following:

   ```
   gpg: Signature made Thu 08 Apr 2021 06:41:00 PM UTC using RSA key ID 848ABDC8
   gpg: Good signature from "Amazon Lambda Insights Extension"
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: E0AF FA11 FFF3 5BD7 349E  E222 479C 97A1 848A BDC8
   ```

   In the expected output, there might be a warning about a trusted signature\. A key is trusted only if you or someone who you trust has signed it\. This doesn't mean that the signature is invalid, only that you have not verified the public key\.

   If the output contains `BAD signature`, check whether you performed the steps correctly\. If you continue to get a `BAD signature` response, contact AWS and avoid using the downloaded file\.

## Example<a name="Lambda-Insights-Getting-Started-docker-example"></a>

This section includes an example of enabling Lambda Insights on a container image\-based Python Lambda function\.

**An example of enabling Lambda Insights on a Lambda container image**

1. Create a Dockerfile that is similar to the following:

   ```
   FROM public.ecr.aws/lambda/python:3.8
   
   // extra lines to install the agent here
   RUN curl -O https://lambda-insights-extension.s3-ap-northeast-1.amazonaws.com/amazon_linux/lambda-insights-extension.rpm && \
       rpm -U lambda-insights-extension.rpm && \
       rm -f lambda-insights-extension.rpm
     
   COPY index.py ${LAMBDA_TASK_ROOT}
   CMD [ "index.handler" ]
   ```

1. Create a Python file named `index.py` that is similar to the following:

   ```
   def handler(event, context):
     return {
       'message': 'Hello World!'
     }
   ```

1. Put the Dockerfile and `index.py` in the same directory\. Then, in that directory, run the following steps to build the docker image and upload it to Amazon ECR\.

   ```
   // create an ECR repository
   aws ecr create-repository --repository-name test-repository
   // build the docker image
   docker build -t test-image .
   // sign in to AWS
   aws ecr get-login-password | docker login --username AWS --password-stdin "${ACCOUNT_ID}".dkr.ecr."${REGION}".amazonaws.com
   // tag the image
   docker tag test-image:latest "${ACCOUNT_ID}".dkr.ecr."${REGION}".amazonaws.com/test-repository:latest
   // push the image to ECR
   docker push "${ACCOUNT_ID}".dkr.ecr."${REGION}".amazonaws.com/test-repository:latest
   ```

1. Use that Amazon ECR image that you just created to create the Lambda function\.

1.  Assign the **CloudWatchLambdaInsightsExecutionRolePolicy** IAM policy to the function's execution role\.