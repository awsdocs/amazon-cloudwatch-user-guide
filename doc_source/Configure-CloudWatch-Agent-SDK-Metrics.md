# Configure the CloudWatch Agent for SDK Metrics<a name="Configure-CloudWatch-Agent-SDK-Metrics"></a>

After installing the CloudWatch agent, you need to configure it to work with SDK Metrics\. The easiest way is to use AWS Systems Manager, but you can also do it manually\.

**Topics**
+ [Configure the CloudWatch Agent for SDK Metrics Using AWS Systems Manager](#Configure-CloudWatch-Agent-SDK-Metrics-Using-Systems-Manager)
+ [Configure the CloudWatch Agent for SDK Metrics Manually](#Configure-CloudWatch-Agent-SDK-Metrics-Manually)

## Configure the CloudWatch Agent for SDK Metrics Using AWS Systems Manager<a name="Configure-CloudWatch-Agent-SDK-Metrics-Using-Systems-Manager"></a>

This section explains how to use SSM to configure the CloudWatch agent to work with SDK Metrics\. For more information about SSM agents, see [Installing and Configuring SSM Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)\.

**To configure SDK Metrics using SSM**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

1. In the **Command document** list, choose the button next to **AWS\-UpdateSSMAgent**\.

1. In the **Targets** area, choose the instance where you installed the CloudWatch agent\.

1. In the navigation pane, choose **Parameter Store**\.

1. Choose **Create Parameter**\.

1. Do the following:

   1. Name the parameter `AmazonCSM`\.

   1. Select type `string`\.

   1. For **Value**, enter `{ "csm": {"memory_limit_in_mb": 20, "port": 31000}}`\.

1. Select **Create Parameter**\.

To complete the configuration, see your SDK documentation\.

## Configure the CloudWatch Agent for SDK Metrics Manually<a name="Configure-CloudWatch-Agent-SDK-Metrics-Manually"></a>

This section explains how to manually configure the CloudWatch agent to work with SDK Metrics\.

**Create Your CloudWatch Agent Configuration File**

If you don't already have a CloudWatch agent configuration file, create one\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

**Add the SDK Metrics Configuration**

Add the following `csm` entry to the top\-level JSON object in the CloudWatch agent configuration file\.

```
"csm": {
    "memory_limit_in_mb": 20,
    "port": 31000
  }
```

For example:

```
{
    "agent": {
      ...
    },
    "metrics": {
      ...
    },
    "csm": {
      "memory_limit_in_mb": 20,
      "port": 31000
    }
  }
```

You can now start the CloudWatch agent using this configuration file\. For more information, see [Start the CloudWatch Agent](install-CloudWatch-Agent-on-EC2-Instance-fleet.md#start-CloudWatch-Agent-EC2-fleet)\.

To complete the configuration, see your SDK documentation\.