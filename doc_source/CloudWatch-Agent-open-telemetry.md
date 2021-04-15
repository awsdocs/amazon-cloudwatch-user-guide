# OpenTelemetry support in the CloudWatch Agent<a name="CloudWatch-Agent-open-telemetry"></a>

Versions 1\.247347\.3 and later of the CloudWatch agent have an embedded AWS OpenTelemetry Collector\. This enables the CloudWatch agent to integrate with AWS OpenTelemetry APIs and SDKs, and send application telemetry data from EC2 instances to CloudWatch and AWS X\-Ray\. This feature is intended for existing CloudWatch agent users who want to begin monitoring with OpenTelemetry without installing or configuring multiple agents\. For more information about AWS OpenTelemetry, see [AWS Distro for OpenTelemetry](https://aws.amazon.com/otel/)\.

By using the CloudWatch agent with the embedded AWS OpenTelemetry Collector, you don't have to install a separate AWS OpenTelemetry Collector\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/images/composite agent-1.png)

The CloudWatch agent with the embedded OpenTelemetry Collector can receive metrics and traces from the AWS OpenTelemetry SDK, and publish them to CloudWatch and X\-Ray\. The OpenTelemetry Collector that is embedded in the CloudWatch agent has the same behavior as the AWS OpenTelemetry Collector, and using it means that you don’t need to install the separate AWS OpenTelemetry Collector\. But if you do install both on the same server, be aware that the embedded OpenTelemetry Collector in the CloudWatch agent and the AWS OpenTelemetry Collector are located in different directories, managed through different tools, and run as separate processes\. For example, if you configure and run both processes in the same server, be sure that the local ports that they use to listen do not conflict with each other\.

OpenTelemetry in the CloudWatch agent is supported for the CloudWatch agent running on EC2 instances, but not for the CloudWatch agent running in containers or on on\-premises servers\. Both AMD64 and ARM64 architectures are supported on Linux instances, and AMD64 is supported on Windows Server instances\.

## IAM permissions<a name="CloudWatch-Agent-open-telemetry-iam"></a>

To be able to publish OpenTelemetry metrics and traces, the CloudWatch agent needs extra IAM permissions in addition to those permissions listed in the **CloudWatchAgentServerPolicy** managed policy\. On servers where you want to use the agent's OpenTelemetry support, grant the following policy to the CloudWatch agent's IAM role that is attached to the instance\. If you used the default suggested name in the documentation, then this role is called **CloudWatchAgentServerRole**\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "xray:PutTraceSegments",
                "xray:PutTelemetryRecords",
                "xray:GetSamplingRules",
                "xray:GetSamplingTargets",
                "xray:GetSamplingStatisticSummaries",
                "ssm:GetParameters"
            ],
            "Resource": "*"
        }
    ]
}
```

## CloudWatch agent OpenTelemetry configuration<a name="CloudWatch-Agent-open-telemetry-configuration"></a>

Amazon provides a default configuration file for the embedded OpenTelemetry Collector\. This configuration enables collecting OpenTelemetry metrics and traces through a default port\. If the default configuration works for you, you don't need to do any more configuration steps\. To see the the contents of this default configuration files, see [ config\.yaml](https://github.com/aws-observability/aws-otel-collector/blob/main/config.yaml) on Github\.

If you want to customize the configuration file, see the [ AWS Distro for OpenTelemetry](https://aws-otel.github.io/docs/introduction) documentation\. If you do this, once you have made the custom configuration file, you can either upload it to Parameter Store or copy it to the file system of every server where you want to use it\. For more information about uploading it to Parameter Store, see [ Uploading the CloudWatch Agent Configuration File to Systems Manager Parameter Store](CloudWatch-Agent-Configuration-File-Details.md#Upload-CloudWatch-Agent-Configuration-To-Parameter-Store)\.

## Use the command line to manage the CloudWatch agent with OpenTelemetry support<a name="CloudWatch-Agent-open-telemetry-processes"></a>

The CloudWatch agent with embedded OpenTelemetry support runs as two processes: the CloudWatch agent process and the OpenTelemetry Collector process\. When you start the CloudWatch agent, each process determines separately whether it can start successfully\.

You can run the `amazon-cloudwatch-agent-ctl` script to managed the OpenTelemetry Collector process\. To configure and start the embedded OpenTelemetry Collector, run one of the the following commands:

Linux:

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a fetch-config -o configuration-file -s
```

Windows Server:

```
& "C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent-ctl.ps1" -a fetch-config -o configuration-file -s
```

In this command, `-a fetch-config` loads the configuration for the OpenTelemetry Collector\. The configuration can be the default configuration or a custom configuration from either Parameter Store or a local file\. The `-s` parameter restarts the agent with this configuration\.

The value of *configuration\-file* can be `default` to use the default built\-in configuration\. To use a customized configuration in Parameter Store, you specify `ssm:your-configuration-parameter-name`\. If you store the configuration in a local file instead, specify it as `file:your-configuration-file.yaml`\. 

For example, the following configures and starts the agent's OpenTelemetry process with the default built\-in configuration for the OpenTelemetry Collector\.

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a fetch-config -o default -s
```

**Important**  
Use the `-o` option to configure the agent's OpenTelemetry Collector process, and use the `-c` option when you are configuring the CloudWatch agent process\. The following command is an example that starts both the embedded OpenTelemetry Collector and the CloudWatch agent in Linux with each’s default built\-in configuration:  

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a fetch-config -o default -c default -s
```

### Use the command line to stop and start the OpenTelemetry Collector in the CloudWatch agent<a name="CloudWatch-Agent-open-telemetry-stop"></a>

**Stop only the OpenTelemetry Collector process**

To stop the OpenTelemetry process in the agent but let the CloudWatch agent process keep running, you use commands to tell the agent to remove the OpenTelemetry configuration, and then restart the agent with no configuration for the OpenTelemetry Collector\.

In the following command, `-a remove-config -o` tells the CloudWatch agent to remove the configuration file for the OpenTelemetry Collector, and `-s` restarts the OpenTelemetry Collector without using any configuration, which effectively stops it\.

Linux:

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a remove-config -o configuration-file -s
```

Windows Server:

```
& "C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent-ctl.ps1" -a remove-config -o configuration-file -s
```

In this command, for *configuration\-file* you can specify `all` to remove whatever configuration was currently applied\. For example, the following command removes the configuration and stops the embedded OpenTelemtry Collector in Linux:

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a remove-config -o all -s
```

**Stop both agent processes**

You can use the `-a stop` parameter to stop both the embedded OpenTelemetry Collector process and the CloudWatch agent process if they are running\.

Linux:

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a stop
```

Windows Server:

```
& "C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent-ctl.ps1" -a stop
```

**Start both agent processes**

You can use the `-a start` parameter to start both the embedded OpenTelemetry Collector process and the CloudWatch agent process if they have already been configured\. If you have never configured the OpenTelemetry Collector, using this parameter will not start it\. If you have never started the CloudWatch agent process before, this command will start it with the default configuration\.

Linux:

```
sudo /usr/bin/amazon-cloudwatch-agent-ctl -a start
```

Windows Server:

```
& "C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent-ctl.ps1" -a start
```

## Use Systems Manager to manage the CloudWatch agent with the embedded OpenTelemetry Collector<a name="CloudWatch-Agent-open-telemetry-stop"></a>

You can also use Systems Manager to manage the CloudWatch agent with the OpenTelemetry Collector\.

**To configure and start the CloudWatch agent using Run Command**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AmazonCloudWatch\-ManageAgent**\.

1. In the **Targets** area, choose the instance where you installed the CloudWatch agent\.

1. In the **Action** list, choose **configure**\.

1. In the **Optional OpenTelemetry Configuration Source** list, choose **ssm** if you want to use your custom configuration, or choose **default** to use the default configuration\.

1. In the **Optional OpenTelemetry Configuration Location** box, enter the name of the OpenTelemetry configuration parameter that you created and saved to Systems Manager Parameter Store, as explained in the previous sections\.

1. \(Optional\) If you want to configure and start the CloudWatch agent process at the same time, do the following:

   1. In the **Optional Configuration Source** list, choose **ssm** if you want to use your custom configuration, or choose **default** to use the default configuration\.

   1. In the **Optional Configuration Location** box, enter the name of the CloudWatch agent configuration parameter that you created and saved to Systems Manager Parameter Store\.

1. In the **Optional Restart** list, choose **yes** to start the agent after you have finished these steps\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully started\. 

**To remove the configuration and stop the OpenTelemetry Collector process**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose **AmazonCloudWatch\-ManageAgent**\.

1. In the **Targets** area, choose the instance where you installed the CloudWatch agent\.

1. In the **Action** list, choose **configure \(remove\)**\.

1. In the **Optional OpenTelemetry Configuration Source** list, choose **all**\.

1. In the **Optional Restart** list, choose **yes** to start the agent after you have finished these steps\.

1. Choose **Run**\.

1. Optionally, in the **Targets and outputs** areas, select the button next to an instance name and choose **View output**\. Systems Manager should show that the agent was successfully started\. 

## Generating OpenTelemetry metrics and traces<a name="CloudWatch-Agent-open-telemetry-generate"></a>

AWS provides OpenTelemetry SDKs and a Java auto instrumentation agent for your applications to use to generate OpenTelemetry metrics and traces and feed them to an OpenTelemetry Collector\. For more information, see the following:
+ [ Getting Started with the Java SDK on Traces and Metrics Instrumentation](https://aws-otel.github.io/docs/getting-started/java-sdk)
+ [ Tracing with the AWS Distro for OpenTelemetry Go SDK](https://aws-otel.github.io/docs/getting-started/go-sdk)
+ [ Getting Started with the Python SDK](https://aws-otel.github.io/docs/getting-started/python-sdk)
+ [ Getting Started with the JavaScript SDK on Traces and Metrics Instrumentation](https://aws-otel.github.io/docs/getting-started/javascript-sdk)