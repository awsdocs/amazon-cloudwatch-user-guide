# Deploying the CloudWatch agent and the X\-Ray daemon on Amazon EC2<a name="deploy_servicelens_CloudWatch_agent_deploy_EC2"></a>

Standard installations of the CloudWatch agent and the X\-Ray daemon are sufficient to enable ServiceLens on Amazon EC2, with the addition of the following CloudWatch agent configuration section example\. For more information about installing the agent, see [Installing the CloudWatch agent](install-CloudWatch-Agent-on-EC2-Instance.md)\. For more information about the CloudWatch agent configuration file, see [ Manually create or edit the CloudWatch agent configuration file](CloudWatch-Agent-Configuration-File-Details.md)\.

When you configure the CloudWatch agent, include this section in your configuration file:

```
{
  "logs": {
    "metrics_collected": {
      "emf": {}
    }
  }
}
```

For more information about installing the X\-Ray daemon, see [X\-Ray Daemon Configuration](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon-configuration.html)\.