# Encrypting canary artifacts<a name="CloudWatch_Synthetics_artifact_encryption"></a>

CloudWatch Synthetics stores canary artifacts such as screenshots, HAR files, and reports in your Amazon S3 bucket\. By default, these artifacts are encrypted at rest using an AWS managed key\. For more information, see [Customer keys and AWS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#key-mgmt.html)\. 

You can choose to use a different encryption option\. CloudWatch Synthetics supports the following:
+ **SSE\-S3**– Server\-side encryption \(SSE\) with an Amazon S3\-managed key\.
+ **SSE\-KMS**– Server\-side encryption \(SSE\) with an AWS KMS customer managed key\.

If you want to use the default encryption option with an AWS managed key, you don't need any additional permissions\. 

To use SSE\-S3 encryption, you specify **SSE\_S3** as the encryption mode when you create or update your canary\. You do not need any additional permissions to use this encryption mode\. For more information, see [Protecting data using server\-side encryption with Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html)\.

To use an AWS KMS customer managed key, you specify **SSE\-KMS** as the encryption mode when you create or update your canary, and you also provide the Amazon Resource Name \(ARN\) of your key\. You can also use a cross\-account KMS key\.

To use a customer managed key, you need the following settings:
+ The IAM role for your canary must have permission to encrypt your artifacts using your key\. If you are using visual monitoring, you must also give it permission to decrypt artifacts\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": {"Effect": "Allow",
          "Action": [
              "kms:GenerateDataKey",
              "kms:Decrypt"
          ],
          "Resource": "Your KMS key ARN"
      }
  }
  ```
+ Instead of adding permissions to your IAM role, you can add your IAM role to your key policy\. If you use the same role for multiple canaries, you should consider this approach\.

  ```
  {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
          "AWS": "Your synthetics IAM role ARN"
      },
      "Action": [
          "kms:GenerateDataKey",
          "kms:Decrypt"
      ],
      "Resource": "*"
  }
  ```
+ If you are using a cross\-account KMS key, see [Allowing users in other accounts to use a KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying-external-accounts.html)\.

**Viewing encrypted canary artifacts when using a customer managed key**

To view canary artifacts, update your customer managed key to give AWS KMS the decrypt permission to the user viewing the artifacts\. Alternatively, add decrypt permissions to the user or IAM role that is viewing the artifacts\.

The default AWS KMS policy enables IAM policies in the account to allow access to the KMS keys\. If you are using a cross\-account KMS key, see [Why are cross\-account users getting Access Denied errors when they try to access Amazon S3 objects encrypted by a custom AWS KMS key?](http://aws.amazon.com/premiumsupport/knowledge-center/cross-account-access-denied-error-s3/)\. 

For more information about troubleshooting access denied issues because of a KMS key, see [Troubleshooting key access](https://docs.aws.amazon.com/kms/latest/developerguide/policy-evaluation.html)\. 

## Updating artifact location and encryption when using visual monitoring<a name="CloudWatch_Synthetics_artifact_encryption_visual"></a>

To perform visual monitoring, CloudWatch Synthetics compares your screenshots with baseline screenshots acquired in the run selected as the baseline\. If you update your artifact location or encryption option, you must do one of the following:
+ Ensure that your IAM role has sufficient permission for both the previous Amazon S3 location and the new Amazon S3 location for artifacts\. Also ensure that it has permission for both the previous and new encryption methods and KMS keys\.
+ Create a new baseline by selecting the next canary run as a new baseline\. If you use this option, you only need to ensure that your IAM role has sufficient permissions for the new artifact location and encryption option\.

We recommend the second option of selecting the next run as the new baseline\. This avoids having a dependency on an artifact location or encryption option that you're not using anymore for the canary\.

For example, suppose that your canary uses artifact location A and KMS key K for uploading artifacts\. If you update your canary to artifact location B and KMS key L, you can ensure that your IAM role has permissions to both of the artifact locations \(A and B\) and both of the KMS keys \(K and L\)\. Alternatively, you can select the next run as the new baseline and ensure that your canary IAM role has permissions to artifact location B and KMS key L\. 