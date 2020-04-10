# Create the CloudWatch Agent Configuration File with the Wizard<a name="create-cloudwatch-agent-configuration-file-wizard"></a>

The agent configuration file wizard, `amazon-cloudwatch-agent-config-wizard`, asks a series of questions, including the following:
+ Are you installing the agent on an Amazon EC2 instance or an on\-premises server?
+ Is the server running Linux or Windows Server?
+ Do you want the agent to also send log files to CloudWatch Logs? If so, do you have an existing CloudWatch Logs agent configuration file? If yes, the CloudWatch agent can use this file to determine the logs to collect from the server\.
+ If you're going to collect metrics from the server, do you want to monitor one of the default sets of metrics or customize the list of metrics that you collect?
+ Do you want to collect custom metrics from your applications or services, using `StatsD` or `collectd`?
+ Are you migrating from an existing SSM Agent?

The wizard can autodetect the credentials and AWS Region to use if you have the AWS credentials and configuration files in place before you start the wizard\. For more information about these files, see [ Configuration and Credential Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html) in the *AWS Systems Manager User Guide*\.

In the AWS credentials file, the wizard checks for default credentials and also looks for an `AmazonCloudWatchAgent` section such as the following:

```
[AmazonCloudWatchAgent]
aws_access_key_id = my_access_key
aws_secret_access_key = my_secret_key
```

The wizard displays the default credentials, the credentials from the `AmazonCloudWatchAgent`, and an `Others` option\. You can select which credentials to use\. If you choose `Others`, you can input credentials\.

For *my\_access\_key* and *my\_secret\_key*, use the keys from the IAM user that has the permissions to write to Systems Manager Parameter Store\. For more information about the IAM users needed for the CloudWatch agent, see [Create IAM Users to Use with the CloudWatch Agent on On\-Premises Servers](create-iam-roles-for-cloudwatch-agent.md#create-iam-roles-for-cloudwatch-agent-users)\.

In the AWS configuration file, you can specify the Region that the agent sends metrics to if it's different than the `[default]` section\. The default is to publish the metrics to the Region where the Amazon EC2 instance is located\. If the metrics should be published to a different Region, specify the Region here\. In the following example, the metrics are published to the `us-west-1` Region\.

```
[AmazonCloudWatchAgent]
region = us-west-1
```

## CloudWatch Agent Predefined Metric Sets<a name="cloudwatch-agent-preset-metrics"></a>

The wizard is configured with predefined sets of metrics, with different detail levels\. These sets of metrics are shown in the following tables\. For more information about these metrics, see [Metrics Collected by the CloudWatch Agent](metrics-collected-by-CloudWatch-agent.md)\. 

**Amazon EC2 instances running Linux**


| Detail Level | Metrics Included | 
| --- | --- | 
|  **Basic** |  **Mem:** mem\_used\_percent **Disk:** disk\_used\_percent The `disk` metrics such as `disk_used_percent` have a dimension for `Partition`, which means that the number of custom metrics generated is dependent on the number of partitions associated with your instance\. The number of disk partitions you have depends on which AMI you are using and the number of Amazon EBS volumes you attach to the server\.  | 
|  **Standard** |  **CPU:** `cpu_usage_idle`, `cpu_usage_iowait`, `cpu_usage_user`, `cpu_usage_system` **Disk:** `disk_used_percent`, `disk_inodes_free` **Diskio:** `diskio_io_time` **Mem:** `mem_used_percent` **Swap:** `swap_used_percent`  | 
|  **Advanced** |  **CPU:** `cpu_usage_idle`, `cpu_usage_iowait`, `cpu_usage_user`, `cpu_usage_system` **Disk:** `disk_used_percent`, `disk_inodes_free` **Diskio:** `diskio_io_time`, `diskio_write_bytes`, `diskio_read_bytes`, `diskio_writes`, `diskio_reads` **Mem:** `mem_used_percent` **Netstat:** `netstat_tcp_established`, `netstat_tcp_time_wait` **Swap:** `swap_used_percent`  | 

**On\-premises servers running Linux**


| Detail Level | Metrics Included | 
| --- | --- | 
|  **Basic** |  **Disk:** `disk_used_percent` **Diskio:** `diskio_write_bytes`, `diskio_read_bytes`, `diskio_writes`, `diskio_reads` **Mem:** `mem_used_percent` **Net:** `net_bytes_sent`, `net_bytes_recv`, `net_packets_sent`, `net_packets_recv` **Swap:** `swap_used_percent`  | 
|  **Standard** |  **CPU:** `cpu_usage_idle`, `cpu_usage_iowait` **Disk:** `disk_used_percent`, `disk_inodes_free` **Diskio:** `diskio_io_time`, `diskio_write_bytes`, `diskio_read_bytes`, `diskio_writes`, `diskio_reads` **Mem:** `mem_used_percent` **Net:** `net_bytes_sent`, `net_bytes_recv`, `net_packets_sent`, `net_packets_recv` **Swap:** `swap_used_percent`  | 
|  **Advanced** |  **CPU:** `cpu_usage_guest`, `cpu_usage_idle`, `cpu_usage_iowait`, `cpu_usage_steal`, `cpu_usage_user`, `cpu_usage_system` **Disk:** `disk_used_percent`, `disk_inodes_free` **Diskio:** `diskio_io_time`, `diskio_write_bytes`, `diskio_read_bytes`, `diskio_writes`, `diskio_reads`  **Mem:** `mem_used_percent`  **Net:** `net_bytes_sent`, `net_bytes_recv`, `net_packets_sent`, `net_packets_recv` **Netstat:** `netstat_tcp_established`, `netstat_tcp_time_wait` **Swap:** `swap_used_percent`  | 

**Amazon EC2 instances running Windows Server**


| Detail Level | Metrics Included | 
| --- | --- | 
|  **Basic** |  **Memory:** `Memory % Committed Bytes In Use` **LogicalDisk:** `LogicalDisk % Free Space`  | 
|  **Standard** |  **Memory:** `Memory % Committed Bytes In Use` **Paging:** `Paging File % Usage` **Processor:** `Processor % Idle Time`, `Processor % Interrupt Time`, `Processor % User Time` **PhysicalDisk:** `PhysicalDisk % Disk Time` **LogicalDisk:** `LogicalDisk % Free Space`  | 
|  **Advanced** |  **Memory:** `Memory % Committed Bytes In Use` **Paging:** `Paging File % Usage` **Processor:** `Processor % Idle Time`, `Processor % Interrupt Time`, `Processor % User Time` **LogicalDisk:** `LogicalDisk % Free Space` **PhysicalDisk:** `PhysicalDisk % Disk Time`, `PhysicalDisk Disk Write Bytes/sec`, `PhysicalDisk Disk Read Bytes/sec`, `PhysicalDisk Disk Writes/sec`, `PhysicalDisk Disk Reads/sec` **TCP:** `TCPv4 Connections Established`, `TCPv6 Connections Established`  | 

**On\-premises server running Windows Server**


| Detail Level | Metrics Included | 
| --- | --- | 
|  **Basic** |  **Paging: **`Paging File % Usage` **Processor:** `Processor % Processor Time` **LogicalDisk:**`LogicalDisk % Free Space`  **PhysicalDisk:** `PhysicalDisk Disk Write Bytes/sec`, `PhysicalDisk Disk Read Bytes/sec`, `PhysicalDisk Disk Writes/sec`, `PhysicalDisk Disk Reads/sec` **Memory:** `Memory % Committed Bytes In Use` **Network Interface:** `Network Interface Bytes Sent/sec`, `Network Interface Bytes Received/sec`, `Network Interface Packets Sent/sec`, `Network Interface Packets Received/sec`  | 
|  **Standard** |  **Paging:** `Paging File % Usage` **Processor:** `Processor % Processor Time`, `Processor % Idle Time`, `Processor % Interrupt Time` **LogicalDisk:** `LogicalDisk % Free Space` **PhysicalDisk:** `PhysicalDisk % Disk Time`, `PhysicalDisk Disk Write Bytes/sec`, `PhysicalDisk Disk Read Bytes/sec`, `PhysicalDisk Disk Writes/sec`, `PhysicalDisk Disk Reads/sec` **Memory:** `Memory % Committed Bytes In Use` **Network Interface:** `Network Interface Bytes Sent/sec`, `Network Interface Bytes Received/sec`, `Network Interface Packets Sent/sec`, `Network Interface Packets Received/sec`  | 
|  **Advanced** |  **Paging:**`Paging File % Usage` **Processor:** `Processor % Processor Time`, `Processor % Idle Time`, `Processor % Interrupt Time`, `Processor % User Time` **LogicalDisk:** `LogicalDisk % Free Space` **PhysicalDisk:** `PhysicalDisk % Disk Time`, `PhysicalDisk Disk Write Bytes/sec`, `PhysicalDisk Disk Read Bytes/sec`, `PhysicalDisk Disk Writes/sec`, `PhysicalDisk Disk Reads/sec` **Memory:** `Memory % Committed Bytes In Use` **Network Interface:** `Network Interface Bytes Sent/sec`, `Network Interface Bytes Received/sec`, `Network Interface Packets Sent/sec`, `Network Interface Packets Received/sec` **TCP:** `TCPv4 Connections Established`, `TCPv6 Connections Established`  | 

## Run the CloudWatch Agent Configuration Wizard<a name="cloudwatch-agent-running-wizard"></a>

**To create the CloudWatch agent configuration file**

1. Start the CloudWatch agent configuration wizard by entering the following:

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
   ```

   On a server running Windows Server, enter the following:

   ```
   cd "C:\Program Files\Amazon\AmazonCloudWatchAgent"
   amazon-cloudwatch-agent-config-wizard.exe
   ```

1. Answer the questions to customize the configuration file for your server\.

1. If you're storing the configuration file locally, the configuration file `config.json` is stored in `/opt/aws/amazon-cloudwatch-agent/bin/` on Linux servers, and is stored in `C:\Program Files\Amazon\AmazonCloudWatchAgent` on Windows Server\. You can then copy this file to other servers where you want to install the agent\.

   If you're going to use Systems Manager to install and configure the agent, be sure to answer **Yes** when prompted whether to store the file in Systems Manager Parameter Store\. You can also choose to store the file in Parameter Store even if you aren't using the SSM Agent to install the CloudWatch agent\. To be able to store the file in Parameter Store, you must use an IAM role with sufficient permissions\. For more information, see [Create IAM Roles and Users for Use with the CloudWatch Agent](create-iam-roles-for-cloudwatch-agent.md)\.