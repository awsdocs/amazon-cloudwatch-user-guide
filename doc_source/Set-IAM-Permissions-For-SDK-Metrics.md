# Set IAM Permissions for SDK Metrics<a name="Set-IAM-Permissions-For-SDK-Metrics"></a>

To configure permissions so you can use SDK Metrics, you must create an IAM policy to allow the SDK Metrics process and attach it to the role or user managing the EC2 instance\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\.

1. Choose **Create Policy**, **JSON**\.

1. Enter the following inline policy into the content window\. Ignore the warning about the non\-supported action\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "sdkmetrics:*"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. Name the policy **AmazonSDKMetrics**\.

1. Attach the policy to the IAM user or role that manages the EC2 instances that you want to monitor\.