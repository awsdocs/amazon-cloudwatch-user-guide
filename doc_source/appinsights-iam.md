# IAM policy<a name="appinsights-iam"></a>

To use CloudWatch Application Insights, you must create an [Identity and Access Management \(IAM\) policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) and attach it to your IAM user identity\. The IAM policy defines the user permissions\.

**To create an IAM policy using the console**  
To create an IAM policy using the IAM console, follow these steps\.

1. Go to the [IAM console](https://console.aws.amazon.com/iam/home)\. In the left navigation pane, select **Policies**\.

1. At the top of the page, select **Create policy**\.

1. Select the **JSON** tab\.

1. Copy and paste the following JSON document under the **JSON** tab\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": [
                   "applicationinsights:*",
                   "iam:CreateServiceLinkedRole",
                   "iam:ListRoles"
               ],
               "Effect": "Allow",
               "Resource": "*"
           }
       ]
   }
   ```

1. Select **Review Policy**\.

1. Enter a **Name** for the policy, for example, “AppInsightsPolicy\.” Optionally, enter a **Description**\.

1. Select **Create Policy**\.

1. Select **Users** from the left navigation pane\.

1. Select the **User name** of the user to which you would like to attach the policy\.

1. Select **Add permissions**\.

1. Select **Attach existing policies directly**\.

1. Search for the policy that you just created, and select the check box to the left of the policy name\.

1. Select **Next: Review**\.

1. Make sure that the correct policy is listed, and select **Add permissions**\.

1. Make sure that you log in with the user associated with the policy that you just created when you use CloudWatch Application Insights\.

**To create an IAM policy using the AWS CLI**  
To create an IAM policy using the AWS CLI, run the [create\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/create-policy.html) operation from the command line using the JSON document above as a file in your current folder\. 

**To create an IAM policy using AWS Tools for Windows PowerShell**  
To create an IAM policy using the AWS Tools for Windows PowerShell, run the [New\-IAMPolicy](https://docs.aws.amazon.com/powershell/latest/reference/items/New-IAMPolicy.html) cmdlt using the JSON document above as a file in your current folder\. 