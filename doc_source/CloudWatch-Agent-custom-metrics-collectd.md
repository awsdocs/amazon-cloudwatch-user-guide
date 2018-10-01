# Retrieve Custom Metrics with collectd<a name="CloudWatch-Agent-custom-metrics-collectd"></a>

You can retrieve custom metrics from your applications or services using the CloudWatch agent with the collectd protocol\. collectd is supported only on Linux servers\. You use collectd software to send the metrics to the CloudWatch agent\.

collectd software is not installed automatically on every server\. For more information about collectd and downloading collectd software, see the [Download page for collectd\.](https://collectd.org/download.shtml) 

Basic configuration for collecting these custom metrics with the CloudWatch agent is to add a **"collectd": \{\}** line to the **metrics\_collected** section of the agent configuration file\. You can add this line manually\. If you use the wizard to create the configuration file, it is done for you\. For more information, see [Create the CloudWatch Agent Configuration File](create-cloudwatch-agent-configuration-file.md)\.

Optional parameters are also available\. If you are using collectd and you do not use `/etc/collectd/auth_file` as your **collectd\_auth\_file**, you must set some of these options\. 
+ **service\_address:** The service address to which the CloudWatch agent should listen\. The format is `"udp://ip:port`\. The default is `udp://127.0.0.1:25826`
+ **name\_prefix:** A prefix to attach to the beginning of the name of each collectd metric\. The default is `collectd_`\. The maximum length is 255 characters\.
+ **collectd\_security\_level:** Sets the security level for network communication\. The default is **Encrypt**\.

  **Encrypt** specifies that only encrypted data is accepted\. **Sign** specifies that only signed and encrypted data is accepted\. **None** specifies that all data is accepted\. If you specify a value for **collectd\_auth\_file**, encrypted data is decrypted if possible\.

  For more information, see [Client setup](https://collectd.org/wiki/index.php/Networking_introduction#Client_setup) and [Possible interactions](https://collectd.org/wiki/index.php/Networking_introduction#Possible_interactions) in the collectd Wiki\.
+ **collectd\_auth\_file** Sets a file in which user names are mapped to passwords\. These passwords are used to verify signatures and to decrypt encrypted network packets\. If given, signed data is verified and encrypted packets are decrypted\. Otherwise, signed data is accepted without checking the signature and encrypted data cannot be decrypted\.

  The default is `/etc/collectd/auth_file`

   If **collectd\_security\_level** is set to **None**, this is optional\. If you set **collectd\_security\_level** to `Encrypt` or **Sign**, you must specify **collectd\_auth\_file**\.

  For the format of the auth file, each line is a user name followed by a colon and any number of spaces followed by the password\. For example:

  `user1: user1_password`

  `user2: user2_password`
+ **collectd\_types\_db:** A list of one or more files that contain the dataset descriptions\. The list must be surrounded by brackets, even if there is just one entry in the list\. Each entry in the list must be surrounded by double quotes\. If there are multiple entries, separate them with commas\. The default is `["/user/share/collectd/types.db"]`\. For more information, see [https://collectd.org/documentation/manpages/types.db.5.shtml](https://collectd.org/documentation/manpages/types.db.5.shtml)\.
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