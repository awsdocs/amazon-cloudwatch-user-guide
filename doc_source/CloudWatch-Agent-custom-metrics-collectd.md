# Retrieve custom metrics with collectd<a name="CloudWatch-Agent-custom-metrics-collectd"></a>

You can retrieve additional metrics from your applications or services using the CloudWatch agent with the collectd protocol, which is supported only on Linux servers\. collectd is a popular open\-source solution with plugins that can gather system statistics for a wide variety of applications\. By combining the system metrics that the CloudWatch agent can already collect with the additional metrics from collectd, you can better monitor, analyze, and troubleshoot your systems and applications\. For more information about collectd, see [collectd \- The system statistics collection daemon](https://collectd.org/)\.

You use the collectd software to send the metrics to the CloudWatch agent\. For the collectd metrics, the CloudWatch agent acts as the server while the collectd plugin acts as the client\.

The collectd software is not installed automatically on every server\. On a server running Amazon Linux 2, follow these steps to install collectd

```
sudo amazon-linux-extras install collectd
```

For information about installing collectd on other systems, see the [Download page for collectd\.](https://collectd.org/download.shtml) 

To collect these custom metrics, add a **"collectd": \{\}** line to the **metrics\_collected** section of the agent configuration file\. You can add this line manually\. If you use the wizard to create the configuration file, it is done for you\. For more information, see [Create the CloudWatch agent configuration file](create-cloudwatch-agent-configuration-file.md)\.

Optional parameters are also available\. If you are using collectd and you do not use `/etc/collectd/auth_file` as your **collectd\_auth\_file**, you must set some of these options\. 
+ **service\_address:** The service address to which the CloudWatch agent should listen\. The format is `"udp://ip:port`\. The default is `udp://127.0.0.1:25826`\.
+ **name\_prefix:** A prefix to attach to the beginning of the name of each collectd metric\. The default is `collectd_`\. The maximum length is 255 characters\.
+ **collectd\_security\_level:** Sets the security level for network communication\. The default is **encrypt**\.

  **encrypt** specifies that only encrypted data is accepted\. **sign** specifies that only signed and encrypted data is accepted\. **none** specifies that all data is accepted\. If you specify a value for **collectd\_auth\_file**, encrypted data is decrypted if possible\.

  For more information, see [Client setup](https://collectd.org/wiki/index.php/Networking_introduction#Client_setup) and [Possible interactions](https://collectd.org/wiki/index.php/Networking_introduction#Possible_interactions) in the collectd Wiki\.
+ **collectd\_auth\_file** Sets a file in which user names are mapped to passwords\. These passwords are used to verify signatures and to decrypt encrypted network packets\. If given, signed data is verified and encrypted packets are decrypted\. Otherwise, signed data is accepted without checking the signature and encrypted data cannot be decrypted\.

  The default is `/etc/collectd/auth_file`\.

   If **collectd\_security\_level** is set to **none**, this is optional\. If you set **collectd\_security\_level** to `encrypt` or **sign**, you must specify **collectd\_auth\_file**\.

  For the format of the auth file, each line is a user name followed by a colon and any number of spaces followed by the password\. For example:

  `user1: user1_password`

  `user2: user2_password`
+ **collectd\_typesdb:** A list of one or more files that contain the dataset descriptions\. The list must be surrounded by brackets, even if there is just one entry in the list\. Each entry in the list must be surrounded by double quotes\. If there are multiple entries, separate them with commas\. The default on Linux servers is `["/usr/share/collectd/types.db"]`\. The default on macOs computers depends on the version of collectd\. For example, `["/usr/local/Cellar/collectd/5.12.0/share/collectd/types.db"]`\.

  For more information, see [https://collectd.org/documentation/manpages/types.db.5.shtml](https://collectd.org/documentation/manpages/types.db.5.shtml)\.
+ **metrics\_aggregation\_interval:** How often in seconds CloudWatch aggregates metrics into single data points\. The default is 60 seconds\. The range is 0 to 172,000\. Setting it to 0 disables the aggregation of collectd metrics\.

The following is an example of the collectd section of the agent configuration file\.

```
{
   "metrics":{
      "metrics_collected":{
         "collectd":{
            "name_prefix":"My_collectd_metrics_",
            "metrics_aggregation_interval":120
         }
      }
   }
}
```

## Viewing collectd metrics imported by the CloudWatch agent<a name="CloudWatch-view-collectd-metrics"></a>

After importing collectd metrics into CloudWatch, you can view these metrics as time series graphs, and create alarms that can watch these metrics and notify you if they breach a threshold that you specify\. The following procedure shows how to view collectd metrics as a time series graph\. For more information about setting alarms, see [Using Amazon CloudWatch alarms](AlarmThatSendsEmail.md)\.

**To view collectd metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose the namespace for the metrics collected by the agent\. By default, this is **CWAgent**, but you may have specified a different namespace in the CloudWatch agent configuration file\.

1. Choose a metric dimension \(for example, **Per\-Instance Metrics**\)\.

1. The **All metrics** tab displays all metrics for that dimension in the namespace\. You can do the following:

   1. To graph a metric, select the check box next to the metric\. To select all metrics, select the check box in the heading row of the table\.

   1. To sort the table, use the column heading\.

   1. To filter by resource, choose the resource ID and then choose **Add to search**\.

   1. To filter by metric, choose the metric name and then choose **Add to search**\.

1. \(Optional\) To add this graph to a CloudWatch dashboard, choose **Actions**, **Add to dashboard**\.