# How Amazon CloudWatch Application Insights works<a name="appinsights-how-works"></a>

**Topics**
+ [How Application Insights monitors applications](#appinsights-how-works-sub)
+ [Data retention](#appinsights-retention)
+ [Quotas](#appinsights-limits)
+ [SSM packages used by Application Insights](#appinsights-ssm-packages)

## How Application Insights monitors applications<a name="appinsights-how-works-sub"></a>

Application Insights monitors applications as follows\.

**Application discovery and configuration**  
The first time an application is added to CloudWatch Application Insights it scans the application components to recommend key metrics, logs, and other data sources to monitor for your application\. You can then configure your application based on these recommendations\. 

**Data preprocessing**  
CloudWatch Application Insights continuously analyzes the data sources being monitored across the application resources to discover metric anomalies and log errors \(observations\)\. 

**Intelligent problem detection**  
The CloudWatch Application Insights engine detects problems in your application by correlating observations using classification algorithms and built\-in rules\. To assist in troubleshooting, it creates automated CloudWatch dashboards, which include contextual information about the problems\. 

**Alert and action**  
When CloudWatch Application Insights detects a problem with your application, it generates CloudWatch Events to notify you of the problem\. See [Application Insights CloudWatch Events and notifications for detected problems](appinsights-cloudwatch-events.md) for more information about how to set up these Events\. 

**Example scenario**

You have an ASP \.NET application that is backed by a SQL Server database\. Suddenly, your database begins to malfunction because of high memory pressure\. This leads to application performance degradation and possibly HTTP 500 errors in your web servers and load balancer\.

With CloudWatch Application Insights and its intelligent analytics, you can identify the application layer that is causing the problem by checking the dynamically created dashboard that shows the related metrics and log file snippets\. In this case, the problem might be at the SQL database layer\.

## Data retention<a name="appinsights-retention"></a>

CloudWatch Application Insights retains problems for 55 days and observations for 60 days\.

## Quotas<a name="appinsights-limits"></a>

For default quotas for CloudWatch Application Insights, see [Amazon CloudWatch Application Insights endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/applicationinsights.html)\. Unless otherwise noted, each quota is per AWS Region\. Contact [AWS Support](https://console.aws.amazon.com/support/home#/case/create?issueType=technical) to request an increase in your service quota\. Many services contain quotas that cannot be changed\. For more information about the quotas for a specific service, see the documentation for that service\. 

## AWS Systems Manager \(SSM\) packages used by CloudWatch Application Insights<a name="appinsights-ssm-packages"></a>

The packages listed in this section are used by Application Insights, and can be independently managed and deployed with AWS Systems Manager Distributor\. For more information about SSM Distributor, see [AWS Systems Manager Distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html) in the *AWS Systems Manager User Guide*\.

**Topics**
+ [`AWSObservabilityExporter-JMXExporterInstallAndConfigure`](#configure-java)
+ [`AWSObservabilityExporter-SAP-HANADBExporterInstallAndConfigure`](#appinsights-ssm-sap-prometheus)
+ [`AWSObservabilityExporter-HAClusterExporterInstallAndConfigure`](#appinsights-ssm-sap-prometheus-ha)

### `AWSObservabilityExporter-JMXExporterInstallAndConfigure`<a name="configure-java"></a>

You can retrieve workload\-specific Java metrics from [Prometheus JMX exporter](https://prometheus.io/docs/instrumenting/exporters/#third-party-exporters) for Application Insights to configure and monitor alarms\. In the Application Insights console, on the **Manage monitoring** page, select **JAVA application** from the **Application tier** dropdown\. Then under **JAVA Prometheus exporter configuration**, select your **Collection method** and **JMX port number**\. 

To use [AWS Systems Manager Distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html) to package, install, and configure the AWS\-provided Prometheus JMX exporter package independently of Application Insights, complete the following steps\.

**Prerequisites for using the Prometheus JMX exporter SSM package**
+ SSM agent version 2\.3\.1550\.0 or later installed
+ The JAVA\_HOME environment variable is set

**Install and configure the `AWSObservabilityExporter-JMXExporterInstallAndConfigure` package**  
The `AWSObservabilityExporter-JMXExporterInstallAndConfigure` package is an SSM Distributor package that you can use to install and configure [Prometheus JMX Exporter](https://github.com/prometheus/jmx_exporter)\. When Java metrics are sent by the Prometheus JMX exporter, the CloudWatch agent can be configured to retrieve the metrics for the CloudWatch service\.

1. Based on your preferences, prepare the [Prometheus JMX exporter YAML configuration file](https://github.com/prometheus/jmx_exporter#configuration) located in the Prometheus GitHub repository\. Use the example configuration and option descriptions to guide you\.

1. Copy the Prometheus JMX exporter YAML configuration file encoded as Base64 to a new SSM parameter in [SSM Parameter Store](https://console.aws.amazon.com/systems-manager/parameters)\.

1. Navigate to the [SSM Distributor](https://console.aws.amazon.com/systems-manager/distributor) console and open the **Third party** tab\. Select **AWSObservabilityExporter\-JMXExporterInstallAndConfigure** and choose **Install one time**\.

1. Update the SSM parameter you created in the first step by replacing "Additional Arguments" with the following:

   ```
   {
     "SSM_EXPORTER_CONFIGURATION": "{{ssm:<SSM_PARAMETER_STORE_NAME>}}",
     "SSM_EXPOSITION_PORT": "9404"
   }
   ```
**Note**  
Port 9404 is the default port used to send Prometheus JMX metrics\. You can update this port\.

**Example: Configure CloudWatch agent to retrieve Java metrics**

1. Install the Prometheus JMX exporter, as described in the previous procedure\. Then verify that it is correctly installed on your instance by checking the port status\.

   Successful installation on Windows instance example

   ```
   PS C:\> curl http://localhost:9404 (http://localhost:9404/)
   StatusCode : 200
   StatusDescription : OK
   Content : # HELP jvm_info JVM version info
   ```

   Successful installation on Linux instance example

   ```
   $ curl localhost:9404
   # HELP jmx_config_reload_failure_total Number of times configuration have failed to be reloaded.
   # TYPE jmx_config_reload_failure_total counter
   jmx_config_reload_failure_total 0.0
   ```

1. Create the Prometheus service discovery YAML file\. The following example service discovery file performs the following:
   + Specifies the Prometheus JMX exporter host port as `localhost: 9404`\.
   + Attaches labels \(`Application`, `ComponentName`, and `InstanceId`\) to the metrics, which can be set as CloudWatch metric dimensions\.

   ```
   $ cat prometheus_sd_jmx.yaml 
   - targets:
     - 127.0.0.1:9404
     labels:
       Application: myApp
       ComponentName: arn:aws:elasticloadbalancing:us-east-1:123456789012:loadbalancer/app/sampl-Appli-MMZW8E3GH4H2/aac36d7fea2a6e5b
       InstanceId: i-12345678901234567
   ```

1. Create the Prometheus JMX exporter configuration YAML file\. The following example configuration file specifies the following:
   + The metrics retrieval job interval and timeout period\.
   + The metrics retrieval jobs \(`jmx` and `sap`\), also known as scraping, which include the job name, maximum time series returned at a time, and service discovery file path\. 

   ```
   $ cat prometheus.yaml 
   global:
     scrape_interval: 1m
     scrape_timeout: 10s
   scrape_configs:
     - job_name: jmx
       sample_limit: 10000
       file_sd_configs:
         - files: ["/tmp/prometheus_sd_jmx.yaml"]
     - job_name: sap
       sample_limit: 10000
       file_sd_configs:
         - files: ["/tmp/prometheus_sd_sap.yaml"]
   ```

1. Verify that the CloudWatch agent is installed on your Amazon EC2 instance and that the version is 1\.247346\.1b249759 or later\. To install the CloudWatch agent on your EC2 instance, see [Installing the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html)\. To verify the version, see [Finding information about CloudWatch agent versions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/troubleshooting-CloudWatch-Agent.html#CloudWatch-Agent-troubleshooting-agent-version)\.

1. Configure the CloudWatch agent\. For more information about how to configure the CloudWatch agent configuration file, see [Manually create or edit the CloudWatch agent configuration file](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html)\. The following example CloudWatch agent configuration file performs the following:
   + Specifies the Prometheus JMX exporter configuration file path\.
   + Specifies the target log group to which to publish EMF metric logs\.
   + Specifies two sets of dimensions for each metric name\.
   + Sends 8 \(4 metric names \* 2 sets of dimensions per metric name\) CloudWatch metrics\.

   ```
   {
      "logs":{
         "logs_collected":{
            ....
         },
         "metrics_collected":{
            "prometheus":{
               "cluster_name":"prometheus-test-cluster",
               "log_group_name":"prometheus-test",
               "prometheus_config_path":"/tmp/prometheus.yaml",
               "emf_processor":{
                  "metric_declaration_dedup":true,
                  "metric_namespace":"CWAgent",
                  "metric_unit":{
                     "jvm_threads_current":"Count",
                     "jvm_gc_collection_seconds_sum":"Second",
                     "jvm_memory_bytes_used":"Bytes"
                  },
                  "metric_declaration":[
                     {
                        "source_labels":[
                           "job"
                        ],
                        "label_matcher":"^jmx$",
                        "dimensions":[
                           [
                              "InstanceId",
                              "ComponentName"
                           ],
                           [
                              "ComponentName"
                           ]
                        ],
                        "metric_selectors":[
                           "^java_lang_threading_threadcount$",
                           "^java_lang_memory_heapmemoryusage_used$",
                           "^java_lang_memory_heapmemoryusage_committed$"
                        ]
                     }
                  ]
               }
            }
         }
      },
      "metrics":{
         ....
      }
   }
   ```

### `AWSObservabilityExporter-SAP-HANADBExporterInstallAndConfigure`<a name="appinsights-ssm-sap-prometheus"></a>

You can retrieve workload\-specific SAP HANA metrics from [Prometheus HANA database exporter](https://prometheus.io/docs/instrumenting/exporters/#third-party-exporters) for Application Insights to configure and monitor alarms\. For more information, see [Set up your SAP HANA database for monitoring](appinsights-tutorial-sap-hana.md#appinsights-tutorial-sap-hana-set-up) in this guide\.

To use [AWS Systems Manager Distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html) to package, install, and configure the AWS\-provided Prometheus HANA database exporter package independently of Application Insights, complete the following steps\.

**Prerequisites for using the Prometheus HANA database exporter SSM package**
+ SSM agent version 2\.3\.1550\.0 or later installed
+ SAP HANA database
+ Linux operating system \(SUSE Linux, RedHat Linux\)
+ A secret with SAP HANA database monitoring credentials, using AWS Secrets Manager\. Create a secret using the key/value pairs format, specify the key username, and enter the database user for the value\. Add a second key password, and then enter the password for the value\. For more information about how to create secrets, see [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\. The secret must be formatted as follows:

  ```
  {
    "username": "<database_user>",
    "password": "<database_password>"
  }
  ```

**Install and configure the `AWSObservabilityExporter-SAP-HANADBExporterInstallAndConfigure` package**  
The `AWSObservabilityExporter-SAP-HANADBExporterInstallAndConfigure` package is an SSM Distributor package that you can use to install and configure [Prometheus HANA database Exporter](https://github.com/prometheus/jmx_exporter)\. When HANA database metrics are sent by the Prometheus HANA database exporter, the CloudWatch agent can be configured to retrieve the metrics for the CloudWatch service\.

1. Create an SSM parameter in [SSM Parameter Store](https://console.aws.amazon.com/systems-manager/parameters) to store the Exporter configurations\. The following is an example parameter value\.

   ```
   {\"exposition_port\":9668,\"multi_tenant\":true,\"timeout\":600,\"hana\":{\"host\":\"localhost\",\"port\":30013,\"aws_secret_name\":\"HANA_DB_CREDS\",\"scale_out_mode\":true}}
   ```
**Note**  
In this example, the export runs only on the Amazon EC2 instance with the active `SYSTEM` database, and it will remain idle on the other EC2 instances in order to avoid duplicate metrics\. The exporter can retrieve all of the database tenant information from the `SYSTEM` database\.

1. Create an SSM parameter in [SSM Parameter Store](https://console.aws.amazon.com/systems-manager/parameters) to store the Exporter metrics queries\. The package can accept more than one metrics parameter\. Each parameter must have a valid JSON object format\. The following is an example parameter value:

   ```
   {\"SELECT MAX(TIMESTAMP) TIMESTAMP, HOST, MEASURED_ELEMENT_NAME CORE, SUM(MAP(CAPTION, 'User Time', TO_NUMBER(VALUE), 0)) USER_PCT, SUM(MAP(CAPTION, 'System Time', TO_NUMBER(VALUE), 0)) SYSTEM_PCT, SUM(MAP(CAPTION, 'Wait Time', TO_NUMBER(VALUE), 0)) WAITIO_PCT, SUM(MAP(CAPTION, 'Idle Time', 0, TO_NUMBER(VALUE))) BUSY_PCT, SUM(MAP(CAPTION, 'Idle Time', TO_NUMBER(VALUE), 0)) IDLE_PCT FROM sys.M_HOST_AGENT_METRICS WHERE MEASURED_ELEMENT_TYPE = 'Processor' GROUP BY HOST, MEASURED_ELEMENT_NAME;\":{\"enabled\":true,\"metrics\":[{\"name\":\"hanadb_cpu_user\",\"description\":\"Percentage of CPU time spent by HANA DB in user space, over the last minute (in seconds)\",\"labels\":[\"HOST\",\"CORE\"],\"value\":\"USER_PCT\",\"unit\":\"percent\",\"type\":\"gauge\"},{\"name\":\"hanadb_cpu_system\",\"description\":\"Percentage of CPU time spent by HANA DB in Kernel space, over the last minute (in seconds)\",\"labels\":[\"HOST\",\"CORE\"],\"value\":\"SYSTEM_PCT\",\"unit\":\"percent\",\"type\":\"gauge\"},{\"name\":\"hanadb_cpu_waitio\",\"description\":\"Percentage of CPU time spent by HANA DB in IO mode, over the last minute (in seconds)\",\"labels\":[\"HOST\",\"CORE\"],\"value\":\"WAITIO_PCT\",\"unit\":\"percent\",\"type\":\"gauge\"},{\"name\":\"hanadb_cpu_busy\",\"description\":\"Percentage of CPU time spent by HANA DB, over the last minute (in seconds)\",\"labels\":[\"HOST\",\"CORE\"],\"value\":\"BUSY_PCT\",\"unit\":\"percent\",\"type\":\"gauge\"},{\"name\":\"hanadb_cpu_idle\",\"description\":\"Percentage of CPU time not spent by HANA DB, over the last minute (in seconds)\",\"labels\":[\"HOST\",\"CORE\"],\"value\":\"IDLE_PCT\",\"unit\":\"percent\",\"type\":\"gauge\"}]}}
   ```

   For more information about metrics queries, see the [https://github.com/SUSE/hanadb_exporter/blob/master/metrics.json](https://github.com/SUSE/hanadb_exporter/blob/master/metrics.json) repo on GitHub\.

1. Navigate to the [SSM Distributor](https://console.aws.amazon.com/systems-manager/distributor) console and open the **Owned by Amazon** tab\. Select **AWSObservabilityExporter\-SAP\-HANADBExporterInstallAndConfigure\*** and choose **Install one time**\.

1. Update the SSM parameter you created in the first step by replacing "Additional Arguments" with the following:

   ```
   {
     "SSM_EXPORTER_CONFIG": "{{ssm:<*SSM_CONFIGURATIONS_PARAMETER_STORE_NAME>*}}",
     "SSM_SID": "<SAP_DATABASE_SID>",
     "SSM_EXPORTER_METRICS_1": "{{ssm:<SSM_FIRST_METRICS_PARAMETER_STORE_NAME>}}",
     "SSM_EXPORTER_METRICS_2": "{{ssm:<SSM_SECOND_METRICS_PARAMETER_STORE_NAME>}}"
   }
   ```

1. Select the Amazon EC2 instances with SAP HANA database, and choose **Run**\.

### `AWSObservabilityExporter-HAClusterExporterInstallAndConfigure`<a name="appinsights-ssm-sap-prometheus-ha"></a>

You can retrieve workload\-specific High Availability \(HA\) cluster metrics from [Prometheus HANA cluster exporter](https://prometheus.io/docs/instrumenting/exporters/#third-party-exporters) for Application Insights to configure and monitor alarms for an SAP HANA database High Availability setup\. For more information, see [Set up your SAP HANA database for monitoring](appinsights-tutorial-sap-hana.md#appinsights-tutorial-sap-hana-set-up) in this guide\.

To use [AWS Systems Manager Distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html) to package, install, and configure the AWS\-provided Prometheus HA cluster exporter package independently of Application Insights, complete the following steps\.

**Prerequisites for using the Prometheus HA cluster exporter SSM package**
+ SSM agent version 2\.3\.1550\.0 or later installed
+ HA cluster for Pacemaker, Corosync, SBD, and DRBD
+ Linux operating system \(SUSE Linux, RedHat Linux\)

**Install and configure the `AWSObservabilityExporter-HAClusterExporterInstallAndConfigure` package**  
The `AWSObservabilityExporter-HAClusterExporterInstallAndConfigure` package is an SSM Distributor package that you can use to install and configure Prometheus HA Cluster Exporter\. When cluster metrics are sent by the Prometheus HANA database exporter, the CloudWatch agent can be configured to retrieve the metrics for the CloudWatch service\.

1. Create an SSM parameter in [SSM Parameter Store](https://console.aws.amazon.com/systems-manager/parameters) to store the Exporter configurations in JSON format\. The following is an example parameter value\.

   ```
   {\"port\":\"9664\",\"address\":\"0.0.0.0\",\"log-level\":\"info\",\"crm-mon-path\":\"/usr/sbin/crm_mon\",\"cibadmin-path\":\"/usr/sbin/cibadmin\",\"corosync-cfgtoolpath-path\":\"/usr/sbin/corosync-cfgtool\",\"corosync-quorumtool-path\":\"/usr/sbin/corosync-quorumtool\",\"sbd-path\":\"/usr/sbin/sbd\",\"sbd-config-path\":\"/etc/sysconfig/sbd\",\"drbdsetup-path\":\"/sbin/drbdsetup\",\"enable-timestamps\":false}
   ```

   For more information about the exporter configurations, see the [https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/ha_cluster_exporter.yaml](https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/ha_cluster_exporter.yaml) repo on GitHub\.

1. Navigate to the [SSM Distributor](https://console.aws.amazon.com/systems-manager/distributor) console and open the **Owned by Amazon** tab\. Select **AWSObservabilityExporter\-HAClusterExporterInstallAndConfigure\*** and choose **Install one time**\.

1. Update the SSM parameter you created in the first step by replacing "Additional Arguments" with the following:

   ```
   {
     "SSM_EXPORTER_CONFIG": "{{ssm:<*SSM_CONFIGURATIONS_PARAMETER_STORE_NAME>*}}"
   }
   ```

1. Select the Amazon EC2 instances with SAP HANA database, and choose **Run**\.