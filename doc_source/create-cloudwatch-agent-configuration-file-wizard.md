# Create the CloudWatch Agent Configuration File with the Wizard<a name="create-cloudwatch-agent-configuration-file-wizard"></a>

The agent configuration file wizard, `amazon-cloudwatch-agent-config-wizard`, asks a series of questions, including the following:
+ Are you installing the agent on an Amazon EC2 instance or an on\-premises server?
+ Is the server running Linux or Windows Server?
+ Do you want the agent to also send log files to CloudWatch Logs? If so, do you have an existing CloudWatch Logs agent configuration file? If yes, the CloudWatch agent can use this file to determine the logs to collect from the server\.
+ If you are going to collect metrics from the server, do you want to monitor one of the default sets of metrics, or customize the list of metrics that you collect?
+ Do you want to collect custom metrics from your applications or services, using StatsD or collectd?
+ Are you migrating from an existing SSM Agent?

The wizard can autodetect the credentials and AWS Region to use, if you have the AWS credentials and configuration files in place before you start the wizard\. For more information about these files, see [ Configuration and Credential Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html) in the *AWS Systems Manager User Guide*\.

The wizard looks for an `AmazonCloudWatchAgent` section such as this in the credentials file:

```
[AmazonCloudWatchAgent]
aws_access_key_id = my_secret_key
aws_secret_access_key = my_access_key
```

If this section exists, the wizard uses these credentials for the CloudWatch agent\.

For `my_access_key` and `my_secret_key`, use the keys from the IAM user that has the permissions to write to Systems Manager Parameter Store\. For more information about the IAM users needed for the CloudWatch agent, see [Create IAM Users to Use with CloudWatch Agent on On\-premises Servers](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-users)\.

In the configuration file, you can specify what region the agent sends metrics to, if it is different than the region in the `[default]` section\. The default is to publish the metrics to region where the Amazon EC2 instance is located\. If the metrics should be published to a different region, specify the region here\. In the following example, the metrics are published to the `us-west-1` region\.

```
[AmazonCloudWatchAgent]
region=us-west-1
```

## CloudWatch Agent Predefined Metric Sets<a name="cloudwatch-agent-preset-metrics"></a>

The wizard is configured with pre\-defined sets of metrics, with different detail levels\. These sets of metrics are shown in the following tables\. For more information about these metrics, see [Metrics Collected by the CloudWatch Agent](metrics-collected-by-CloudWatch-agent.md)\. 

**Amazon EC2 instances running Linux**


| Detail Level | Metrics Included | 
| --- | --- | 
|  Basic |  **Mem:** mem\_used\_percent **Swap:** swap\_used\_percent  | 
|  Standard |  **CPU:** cpu\_usage\_idle, cpu\_usage\_iowait, cpu\_usage\_user, cpu\_usage\_system**Disk:** disk\_used\_percent, disk\_inodes\_free, diskio\_io\_time**Mem:** mem\_used\_percent**Swap:** swap\_used\_percent  | 
|  Advanced |  **CPU:** cpu\_usage\_idle, cpu\_usage\_iowait, cpu\_usage\_user, cpu\_usage\_system **Disk:** disk\_used\_percent, disk\_inodes\_free **Diskio:** diskio\_io\_time, diskio\_write\_bytes, diskio\_read\_bytes, diskio\_writes, diskio\_reads **Mem:** mem\_used\_percent **Netstat:** netstat\_tcp\_established, netstat\_tcp\_time\_wait **Swap:** swap\_used\_percent  | 

**On\-premises servers running Linux**


| Detail Level | Metrics Included | 
| --- | --- | 
|  Basic |  **Disk:** disk\_used\_percent**Diskio:** diskio\_write\_bytes, diskio\_read\_bytes, diskio\_writes, diskio\_reads **Mem:** mem\_used\_percent **Net:** net\_bytes\_sent, net\_bytes\_recv, net\_packets\_sent, net\_packets\_recv**Swap:** swap\_used\_percent  | 
|  Standard |  **CPU:** cpu\_usage\_idle, cpu\_usage\_iowait**Disk:** disk\_used\_percent, disk\_inodes\_free**Diskio:** diskio\_io\_time, diskio\_write\_bytes, diskio\_read\_bytes, diskio\_writes, diskio\_reads **Mem:** mem\_used\_percent **Net:** net\_bytes\_sent, net\_bytes\_recv, net\_packets\_sent, net\_packets\_recv**Swap:** swap\_used\_percent  | 
|  Advanced |  **CPU:** cpu\_usage\_idle, cpu\_usage\_guest, cpu\_usage\_iowait, cpu\_usage\_steal, cpu\_usage\_user, cpu\_usage\_system**Disk:** disk\_used\_percent, disk\_inodes\_free**Diskio:** diskio\_io\_time, diskio\_write\_bytes, diskio\_read\_bytes, diskio\_writes, diskio\_reads  **Mem:** mem\_used\_percent **Net:** net\_bytes\_sent, net\_bytes\_recv, net\_packets\_sent, net\_packets\_recv**Netstat:** netstat\_tcp\_established, netstat\_tcp\_time\_wait, **Swap:** swap\_used\_percent  | 

**Amazon EC2 instances running Windows Server**


| Detail Level | Metrics Included | 
| --- | --- | 
|  Basic |  **Memory:** Memory % Committed Bytes In Use **Paging:** Paging File % Usage  | 
|  Standard |  **Memory:** Memory % Committed Bytes In Use **Paging:** Paging File % Usage **Processor:** Processor % Idle Time, Processor % Interrupt Time, Processor % User Time,  **PhysicalDisk:** PhysicalDisk % Disk Time**LogicalDisk:** LogicalDisk % Free Space  | 
|  Advanced |  **Memory:** Memory % Committed Bytes In Use **Paging:** Paging File % Usage **Processor:** Processor % Idle Time, Processor % Interrupt Time, Processor % User Time **LogicalDisk:** LogicalDisk % Free Space  **PhysicalDisk:** PhysicalDisk % Disk Time, PhysicalDisk Disk Write Bytes/sec, PhysicalDisk Disk Read Bytes/sec, PhysicalDisk Disk Writes/sec, PhysicalDisk Disk Reads/sec **TCP:** TCPv4 Connections Established, TCPv6 Connections Established   | 

**On\-premises server running Windows Server**


| Detail Level | Metrics Included | 
| --- | --- | 
|  Basic |  **Processor:** Processor % Processor Time **Paging:**Paging File % Usage **LogicalDisk:** LogicalDisk % Free Space **PhysicalDisk:** PhysicalDisk Disk Write Bytes/sec, PhysicalDisk Disk Read Bytes/sec, PhysicalDisk Disk Writes/sec, PhysicalDisk Disk Reads/sec**Memory:** Memory % Committed Bytes In Use**Network Interface:** Network Interface Bytes Sent/sec, Network Interface Bytes Received/sec, Network Interface Packets Sent/sec, Network Interface Packets Received/sec  | 
|  Standard |  **Paging:** Paging File % Usage**Processor:** Processor\_% Processor Time, Processor % Idle Time Processor % Interrupt Time**LogicalDisk:** LogicalDisk % Free Space**PhysicalDisk:** PhysicalDisk % Disk Time, PhysicalDisk Disk Write Bytes/sec, PhysicalDisk Disk Read Bytes/sec, PhysicalDisk Disk Writes/sec, PhysicalDisk Disk Reads/sec**Memory:** Memory % Committed Bytes In Use**Network Interface:** Network Interface Bytes Sent/sec, Network Interface Bytes Received/sec, Network Interface Packets Sent/sec, Network Interface Packets Received/sec   | 
|  Advanced |  **Paging:**Paging File % Usage**Processor:** Processor % Processor Time, Processor % Idle Time, Processor % Interrupt Time, Processor % User Time**LogicalDisk:** LogicalDisk % Free Space**PhysicalDisk:** PhysicalDisk % Disk Time, PhysicalDisk Disk Write Bytes/sec, PhysicalDisk Disk Read Bytes/sec, PhysicalDisk Disk Writes/sec, PhysicalDisk Disk Reads/sec**Memory:** Memory % Committed Bytes In Use**Network Interface:** Network Interface Bytes Sent/sec, Network Interface Bytes Received/sec, Network Interface Packets Sent/sec, Network Interface Packets Received/sec**TCP:** TCPv4 Connections Established, TCPv6 Connections Established  | 

## Running the CloudWatch Agent Configuration Wizard<a name="cloudwatch-agent-running-wizard"></a>

**To create the CloudWatch agent configuration file**

1. Start the CloudWatch agent configuration wizard by typing the following:

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
   ```

   On a server running Windows Server, type the following:

   ```
   cd "C:\Program Files\Amazon\AmazonCloudWatchAgent"
   amazon-cloudwatch-agent-config-wizard.exe
   ```

1. Answer the questions to customize the configuration file for your server\.

1. If you are going to use Systems Manager to install and configure the agent, be sure to answer **Yes** when prompted whether to store the file in Systems Manager Parameter Store\. You can also choose to store the file in Parameter Store even if you aren't using the SSM Agent to install the CloudWatch agent\. To be able to store the file in Parameter Store, you must use an IAM role with sufficient permissions\. For more information, see [Create IAM Roles and Users for Use With CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)\.

   If you are storing the configuration file locally, the configuration file `config.json` is stored in the current working directory\. You then specify this file location when you start the agent\.