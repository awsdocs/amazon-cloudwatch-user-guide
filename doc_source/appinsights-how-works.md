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

The following table provides the default quotas for CloudWatch Application Insights\. Unless otherwise noted, each quota is per AWS Region\. Please contact [AWS Support](https://console.aws.amazon.com/support/home#/case/create?issueType=technical) to request an increase in your service quota\. Many services contain quotas that cannot be changed\. For more information about the quotas for a specific service, see the documentation for that service\. 


| Resource  | Default quota | 
| --- | --- | 
|  API requests  |  All API actions are throttled to 5 TPS  | 
| Applications  |  10 per account  | 
| Log Streams  |  5 per resource  | 
| Observations per problem  |  20 per dashboard 40 per DescribeProblemObservations action  | 
| Metrics |  30 per resource  | 
| Resources  |  30 per application  | 

## AWS Systems Manager \(SSM\) packages used by CloudWatch Application Insights<a name="appinsights-ssm-packages"></a>

The packages listed in this section are used by Application Insights and can be independently managed and deployed with AWS Systems Manager Distributor\. For more information about SSM Distributor, see [AWS Systems Manager Distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html) in the *AWS Systems Manager User Guide*\.

**Topics**
+ [`AWSObservabilityExporter-JMXExporterInstallAndConfigure`](#configure-java)

### `AWSObservabilityExporter-JMXExporterInstallAndConfigure`<a name="configure-java"></a>

You can retrieve workload\-specific Java metrics from [Prometheus JMX exporter](https://prometheus.io/docs/instrumenting/exporters/#third-party-exporters) for Application Insights to configure and monitor alarms\. In the Application Insights console, on the **Manage monitoring** page, select **JAVA application** from the **Application tier** dropdown\. Then under **JAVA Prometheus exporter configuration**, select your **Collection method** and **JMX port number**\. 

To use [AWS Systems Manager Distributor](https://docs.aws.amazon.com/systems-manager/latest/userguide/distributor.html) to package, install, and configure the AWS\-provided Prometheus JMX exporter package independently of Application Insights, complete the following steps\.

**Prerequisites for using the Prometheus JMX exporter SSM package**
+ SSM agent version 2\.3\.1550\.0 or later installed
+ The JAVA\_HOME environment variable is set

**Install and configure the `AWSObservabilityExporter-JMXExporterInstallAndConfigure` package**  
The `AWSObservabilityExporter-JMXExporterInstallAndConfigure` package is an SSM Distributor package that you can use to install and configure [Prometheus JMX Exporter](https://github.com/prometheus/jmx_exporter)\. When Java metrics are sent by the Prometheus JMX exporter, the CloudWatch agent can be configured to retrieve the metrics for the CloudWatch service\.

1. Based on your preferences, prepare the [Prometheus JMX exporter YAML configuration file](https://github.com/prometheus/jmx_exporter#configuration) located in the Prometheus GitHub repository using the example configuration and option descriptions to guide you\.

1. Copy the Prometheus JMX exporter YAML configuration file encoded as Base64 to a new SSM parameter in [SSM Parameter Store](https://console.aws.amazon.com/systems-manager/parameters)\.

1. Navigate to the [SSM Distributor](https://console.aws.amazon.com/systems-manager/distributor) console and open the **Third party** tab\. Select **AWSObservabilityExporter\-JMXExporterInstallAndConfigure** and choose **Install one time**\.

1. Update the SSM parameter you created in the first step by replacing "Additional Arguments" with the following:

   ```
   {
     "SSM_EXPORTER_CONFIGURATION": "ssm:<SSM_PARAMETER_STORE_NAME>",
     "SSM_EXPOSITION_PORT": "9404"
   }
   ```
**Note**  
Port 9404 is the default port used to send Prometheus JMX metrics; however, you can update it\.

**Example: Configure CloudWatch agent to retrieve Java metrics**

1. Install the Prometheus JMX exporter as described in the previous procedure and verify that it is correctly installed on your instance by checking the port status\.

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
   + The metrics retrieval \(also know as "scraping"\) jobs \(`jmx` and `sap`\), which include the job name, maximum time series returned at a time, and service discovery file path\. 

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