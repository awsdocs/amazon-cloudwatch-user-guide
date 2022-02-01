# IAM policies to use CloudWatch RUM<a name="CloudWatch-RUM-permissions"></a>

To be able to fully manage CloudWatch RUM, you must be signed in as an IAM user or role that has the **AmazonCloudWatchRUMFullAccess** IAM policy\. Additionally, you may need other policies or permissions:
+ To create an app monitor that creates a new Amazon Cognito identity pool for authorization, you need to have the **Admin** IAM role or the **AdministratorAccess** IAM policy\.
+ To create an app monitor that sends data to CloudWatch Logs, you must be logged on to an IAM role or policy that has the following permission:

  ```
  {
      "Effect": "Allow",
      "Action": [
          "logs:PutResourcePolicy"
      ],
      "Resource": [
          "*"
      ]
  }
  ```

Other users who need to view CloudWatch RUM data but don't need to create CloudWatch RUM resources, can be granted the **AmazonCloudWatchRUMReadOnlyAccess** policy\.