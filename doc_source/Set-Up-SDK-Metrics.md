# Set Up SDK Metrics<a name="Set-Up-SDK-Metrics"></a>

To set up SDK Metrics, you perform some steps with your SDK and some with CloudWatch agent\.

**To set up SDK Metrics**

1. Create an application with an AWS SDK that supports SDK Metrics\.

1. Host your project on an Amazon EC2 instance or in your local environment\.

1. Install and use the latest version of your SDK\.

1. Install the CloudWatch agent on the EC2 instance or the local environment that is running an application that you want to receive metrics for\. For more information, see [Installing the CloudWatch Agent](install-CloudWatch-Agent-on-EC2-Instance.md)\.

1. Configure the CloudWatch agent for SDK Metrics\. For more information, see [Configure the CloudWatch Agent for SDK Metrics](Configure-CloudWatch-Agent-SDK-Metrics.md)\.

1. Authorize SDK Metrics to collect and send metrics\. For more information, see [Set IAM Permissions for SDK Metrics](Set-IAM-Permissions-For-SDK-Metrics.md)\.

1. Enable SDK Metrics\. There are multiple methods to do this, depending on which SDK you're using\. One method that is universal for all SDKs is to add the following to your environment variables:

   ```
   export AWS_CSM_ENABLED=true
   ```

   For information about the other methods, see your SDK documentation as shown in the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Set-Up-SDK-Metrics.html)