# Getting set up<a name="GettingSetup"></a>

To use Amazon CloudWatch you need an AWS account\. Your AWS account allows you to use services \(for example, Amazon EC2\) to generate metrics that you can view in the CloudWatch console, a point\-and\-click web\-based interface\. In addition, you can install and configure the AWS command line interface \(CLI\)\.

## Sign up for Amazon Web Services \(AWS\)<a name="signup"></a>

When you create an AWS account, we automatically sign up your account for all AWS services\. You pay only for the services that you use\. 

If you have an account already, skip to the next step\. If you don't have an account, use the following procedure to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Sign in to the Amazon CloudWatch console<a name="ConsoleSignIn"></a>

**To sign in to the Amazon CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. If necessary, use the navigation bar to change the Region to the Region where you have your AWS resources\.

1. Even if this is the first time you are using the CloudWatch console, **Your Metrics** could already report metrics, because you have used an AWS product that automatically pushes metrics to Amazon CloudWatch for free\. Other services require that you enable metrics\.

   If you do not have any alarms, the **Your Alarms** section will have a **Create Alarm** button\.

## Set up the AWS CLI<a name="SetupCLI"></a>

You can use the AWS CLI or the Amazon CloudWatch CLI to perform CloudWatch commands\. Note that the AWS CLI replaces the CloudWatch CLI; we include new CloudWatch features only in the AWS CLI\.

For information about how to install and configure the AWS CLI, see [Getting Set Up with the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html) in the *AWS Command Line Interface User Guide*\.

For information about how to install and configure the Amazon CloudWatch CLI, see [Set Up the Command Line Interface](https://docs.aws.amazon.com/AmazonCloudWatch/latest/cli/SetupCLI.html) in the *Amazon CloudWatch CLI Reference*\.