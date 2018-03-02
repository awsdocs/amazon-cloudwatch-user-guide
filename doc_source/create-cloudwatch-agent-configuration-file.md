# Create the CloudWatch Agent Configuration File<a name="create-cloudwatch-agent-configuration-file"></a>

Whether you are installing the CloudWatch agent on an Amazon EC2 instance or an on\-premises server, you must create the CloudWatch agent configuration file before starting the agent\. 

The agent configuration file is a JSON file that specifies the metrics and logs that the agent is to collect\. You can create it by using the wizard, or by creating it yourself from scratch\. You could also use the wizard to initially create the configuration file, and then modify it manually\.

If you create or modify it manually the process is more complex, but you have more control over the metrics collected, and can specify metrics not used by the wizard\.

Any time you change the agent configuration file, you must then restart the agent to have the changes take effect\.

After you have created a configuration file, you can store it in Systems Manager Parameter Store\. This enables you to use the same agent configuration on other servers\.


+ [Create the CloudWatch Agent Configuration File with the Wizard](create-cloudwatch-agent-configuration-file-wizard.md)
+ [Manually Create or Edit the CloudWatch Agent Configuration File](CloudWatch-Agent-Configuration-File-Details.md)