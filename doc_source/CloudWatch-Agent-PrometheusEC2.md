# Set up and configure Prometheus metrics collection on Amazon EC2 instances<a name="CloudWatch-Agent-PrometheusEC2"></a>

The following sections explain how to install the CloudWatch agent with Prometheus monitoring on EC2 instances, and how to configure the agent to scrape additional targets\. It also provides tutorials for setting up sample workloads to use testing with Prometheus monitoring\.

For information about the operating systems supported by the CloudWatch agent, see [Collecting metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](Install-CloudWatch-Agent.md)

**VPC security group requirements**

If you are using a VPC, the following requirements apply\.
+ The ingress rules of the security groups for the Prometheus workloads must open the Prometheus ports to the CloudWatch agent for scraping the Prometheus metrics by the private IP\.
+ The egress rules of the security group for the CloudWatch agent must allow the CloudWatch agent to connect to the Prometheus workloads' port by private IP\. 

**Topics**
+ [Step 1: Install the CloudWatch agent](#CloudWatch-Agent-PrometheusEC2-install)
+ [Step 2: Scrape Prometheus sources and import metrics](#CloudWatch-Agent-PrometheusEC2-configure)
+ [Example: Set up Java/JMX sample workloads for Prometheus metric testing](#CloudWatch-Agent-Prometheus-Java)

## Step 1: Install the CloudWatch agent<a name="CloudWatch-Agent-PrometheusEC2-install"></a>

The first step is to install the CloudWatch agent on the EC2 instance\. For instructions, see [Installing the CloudWatch agent](install-CloudWatch-Agent-on-EC2-Instance.md)\.

## Step 2: Scrape Prometheus sources and import metrics<a name="CloudWatch-Agent-PrometheusEC2-configure"></a>

The CloudWatch agent with Prometheus monitoring needs two configurations to scrape the Prometheus metrics\. One is for the standard Prometheus configurations as documented in [ <scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config)in the Prometheus documentation\. The other is for the CloudWatch agent configuration\.

### Prometheus scrape configuration<a name="CloudWatch-Agent-PrometheusEC2-configure-scrape"></a>

The CloudWatch agent supports the standard Prometheus scrape configurations as documented in [ <scrape\_config>](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config) in the Prometheus documentation\. You can edit this section to update the configurations that are already in this file, and add additional Prometheus scraping targets\. A sample configuration file contains the following global configuration lines:

```
PS C:\ProgramData\Amazon\AmazonCloudWatchAgent> cat prometheus.yaml
global:
  scrape_interval: 1m
  scrape_timeout: 10s
scrape_configs:
- job_name: MY_JOB
  sample_limit: 10000
  file_sd_configs:
    - files: ["C:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\prometheus_sd_1.yaml", "C:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\prometheus_sd_2.yaml"]
```

The `global` section specifies parameters that are valid in all configuration contexts\. They also serve as defaults for other configuraiton sections\. It contains the following parameters:
+ `scrape_interval`— Defines how frequently to scrape targets\.
+ `scrape_timeout`— Defines how long to wait before a scrape request times out\.

The `scrape_configs` section specifies a set of targets and parameters that define how to scrape them\. It contains the following parameters:
+ `job_name`— The job name assigned to scraped metrics by default\.
+ `sample_limit`— Per\-scrape limit on the number of scraped samples that will be accepted\.
+ `file_sd_configs`— List of file service discovery configurations\. It reads a set of files containing a list of zero or more static configs\. The `file_sd_configs` section contains a `files` parameter which defines patterns for files from which target groups are extracted\.

The CloudWatch agent supports the following service discovery configuration types\.

**`static_config`** Allows specifying a list of targets and a common label set for them\. It is the canonical way to specify static targets in a scrape configuration\.

The following is a sample static config to scrape Prometheus metrics from a local host\. Metrics can also be scraped from other servers if the Prometheus port is open to the server where the agent runs\.

```
PS C:\ProgramData\Amazon\AmazonCloudWatchAgent> cat prometheus_sd_1.yaml
- targets:
    - 127.0.0.1:9404
  labels:
    key1: value1
    key2: value2
```

This example contains the following parameters:
+ `targets`— The targets scraped by the static config\.
+ `labels`— Labels assigned to all metrics that are scraped from the targets\.

**`ec2_sd_config`** Allows retrieving scrape targets from Amazon EC2 instances\. The following is a sample `ec2_sd_config` to scrape Prometheus metrics from a list of EC2 instances\. The Prometheus ports of these instances have to open to the server where the CloudWatch agent runs\. The IAM role for the EC2 instance where the CloudWatch agent runs must include the `ec2:DescribeInstance` permission\. For example, you could attach the managed policy **AmazonEC2ReadOnlyAccess** to the instance running the CloudWatch agent\.

```
PS C:\ProgramData\Amazon\AmazonCloudWatchAgent> cat prometheus.yaml
global:
  scrape_interval: 1m
  scrape_timeout: 10s
scrape_configs:
  - job_name: MY_JOB
    sample_limit: 10000
    ec2_sd_configs:
      - region: us-east-1
        port: 9404
        filters:
          - name: instance-id
            values:
              - i-98765432109876543
              - i-12345678901234567
```

This example contains the following parameters:
+ `region`— The AWS Region where the target EC2 instance is\. If you leave this blank, the Region from the instance metadata is used\.
+ `port`— The port to scrape metrics from\.
+ `filters`— Optional filters to use to filter the instance list\. This example filters based on EC2 instance IDs\. For more criteria that you can filter on, see [ DescribeInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstances.html)\.

### CloudWatch agent configuration for Prometheus<a name="CloudWatch-Agent-PrometheusEC2-configure-agent"></a>

The CloudWatch agent configuration file includes `prometheus` sections under both `logs` and `metrics_collected`\. It includes the following parameters\.
+ **cluster\_name**— specifies the cluster name to be added as a label in the log event\. This field is optional\. 
+ **log\_group\_name**— specifies the log group name for the scraped Prometheus metrics\.
+ **prometheus\_config\_path**— specifies the Prometheus scrape configuration file path\.
+ **emf\_processor**— specifies the embedded metric format processor configuration\. For more information about embedded metric format, see [Ingesting high\-cardinality logs and generating metrics with CloudWatch embedded metric format](CloudWatch_Embedded_Metric_Format.md)\. 

  The `emf_processor` section can contain the following parameters:
  + **metric\_declaration\_dedup**— It set to true, the de\-duplication function for the embedded metric format metrics is enabled\.
  + **metric\_namespace**— Specifies the metric namespace for the emitted CloudWatch metrics\.
  + **metric\_unit**— Specifies the metric name:metric unit map\. For information about supported metrit units, see [ MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html)\.
  + **metric\_declaration**— are sections that specify the array of logs with embedded metric format to be generated\. There are `metric_declaration` sections for each Prometheus source that the CloudWatch agent imports from by default\. These sections each include the following fields:
    + `source_labels` specifies the value of the labels that are checked by the `label_matcher` line\.
    + `label_matcher` is a regular expression that checks the value of the labels listed in `source_labels`\. The metrics that match are enabled for inclusion in the embedded metric format sent to CloudWatch\. 
    + `metric_selectors` is a regular expression that specifies the metrics to be collected and sent to CloudWatch\.
    + `dimensions` is the list of labels to be used as CloudWatch dimensions for each selected metric\.

The following is an example CloudWatch agent configuration for Prometheus\.

```
{
   "logs":{
      "metrics_collected":{
         "prometheus":{
            "cluster_name":"prometheus-cluster",
            "log_group_name":"Prometheus",
            "prometheus_config_path":"C:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\prometheus.yaml",
            "emf_processor":{
               "metric_declaration_dedup":true,
               "metric_namespace":"CWAgent-Prometheus",
               "metric_unit":{
                  "jvm_threads_current": "Count",
                  "jvm_gc_collection_seconds_sum": "Milliseconds"
               },
               "metric_declaration":[
                  {
                     "source_labels":[
                        "job", "key2"
                     ],
                     "label_matcher":"MY_JOB;^value2",
                     "dimensions":[
                        [
                           "key1", "key2"
                        ],
                        [
                           "key2"
                        ]
                     ],
                     "metric_selectors":[
                        "^jvm_threads_current$",
                        "^jvm_gc_collection_seconds_sum$"
                     ]
                  }
               ]
            }
         }
      }
   }
}
```

The previous example configures an embedded metric format section to be sent as a log event if the following conditions are met:
+ The value of the label `job` is `MY_JOB`
+ The value of the label `key2` is `value2`
+ The Prometheus metrics `jvm_threads_current` and `jvm_gc_collection_seconds_sum` contains both `job` and `key2` labels\.

The log event that is sent includes the following highlighted section\.

```
{
    "CloudWatchMetrics": [
        {
            "Metrics": [
                {
                    "Unit": "Count",
                    "Name": "jvm_threads_current"
                },
                {
                    "Unit": "Milliseconds",
                    "Name": "jvm_gc_collection_seconds_sum"
                }
            ],
            "Dimensions": [
                [
                    "key1",
                    "key2"
                ],
                [
                    "key2"
                ]
            ],
            "Namespace": "CWAgent-Prometheus"
        }
    ],
    "ClusterName": "prometheus-cluster",
    "InstanceId": "i-0e45bd06f196096c8",
    "Timestamp": "1607966368109",
    "Version": "0",
    "host": "EC2AMAZ-PDDOIUM",
    "instance": "127.0.0.1:9404",
    "jvm_threads_current": 2,
    "jvm_gc_collection_seconds_sum": 0.006000000000000002,
    "prom_metric_type": "gauge",
    ...
}
```

## Example: Set up Java/JMX sample workloads for Prometheus metric testing<a name="CloudWatch-Agent-Prometheus-Java"></a>

JMX Exporter is an official Prometheus exporter that can scrape and expose JMX mBeans as Prometheus metrics\. For more information, see [prometheus/jmx\_exporter](https://github.com/prometheus/jmx_exporter)\.

The CloudWatch agent can collect predefined Prometheus metrics from Java Virtual Machine \(JVM\), Hjava, and Tomcat \(Catalina\), from a JMX exporter on EC2 instances\.

### Step 1: Install the CloudWatch agent<a name="CloudWatch-Agent-PrometheusJava-install"></a>

The first step is to install the CloudWatch agent on the EC2 instance\. For instructions, see [Installing the CloudWatch agent](install-CloudWatch-Agent-on-EC2-Instance.md)\.

### Step 2: Start the Java/JMX workload<a name="CloudWatch-Agent-PrometheusJava-start"></a>

The next step is to start the Java/JMX workload\.

First, download the latest JMX exporter jar file from the following location: [prometheus/jmx\_exporter](https://github.com/prometheus/jmx_exporter)\.

 **Use the jar for your sample application**

The example commands in the following sections use `SampleJavaApplication-1.0-SNAPSHOT.jar` as the jar file\. Replace these parts of the commands with the jar for your application\.

#### Prepare the JMX exporter configuration<a name="CloudWatch-Agent-PrometheusJava-start-config"></a>

The `config.yaml` file is the JMX exporter configuration file\. For more information, see [Configuration](https://github.com/prometheus/jmx_exporter#Configuration) in the JMX exporter documentation\.

Here is a sample configuration for Java and Tomcat\.

```
---
lowercaseOutputName: true
lowercaseOutputLabelNames: true

rules:
- pattern: 'java.lang<type=OperatingSystem><>(FreePhysicalMemorySize|TotalPhysicalMemorySize|FreeSwapSpaceSize|TotalSwapSpaceSize|SystemCpuLoad|ProcessCpuLoad|OpenFileDescriptorCount|AvailableProcessors)'
  name: java_lang_OperatingSystem_$1
  type: GAUGE

- pattern: 'java.lang<type=Threading><>(TotalStartedThreadCount|ThreadCount)'
  name: java_lang_threading_$1
  type: GAUGE

- pattern: 'Catalina<type=GlobalRequestProcessor, name=\"(\w+-\w+)-(\d+)\"><>(\w+)'
  name: catalina_globalrequestprocessor_$3_total
  labels:
    port: "$2"
    protocol: "$1"
  help: Catalina global $3
  type: COUNTER

- pattern: 'Catalina<j2eeType=Servlet, WebModule=//([-a-zA-Z0-9+&@#/%?=~_|!:.,;]*[-a-zA-Z0-9+&@#/%=~_|]), name=([-a-zA-Z0-9+/$%~_-|!.]*), J2EEApplication=none, J2EEServer=none><>(requestCount|maxTime|processingTime|errorCount)'
  name: catalina_servlet_$3_total
  labels:
    module: "$1"
    servlet: "$2"
  help: Catalina servlet $3 total
  type: COUNTER

- pattern: 'Catalina<type=ThreadPool, name="(\w+-\w+)-(\d+)"><>(currentThreadCount|currentThreadsBusy|keepAliveCount|pollerThreadCount|connectionCount)'
  name: catalina_threadpool_$3
  labels:
    port: "$2"
    protocol: "$1"
  help: Catalina threadpool $3
  type: GAUGE

- pattern: 'Catalina<type=Manager, host=([-a-zA-Z0-9+&@#/%?=~_|!:.,;]*[-a-zA-Z0-9+&@#/%=~_|]), context=([-a-zA-Z0-9+/$%~_-|!.]*)><>(processingTime|sessionCounter|rejectedSessions|expiredSessions)'
  name: catalina_session_$3_total
  labels:
    context: "$2"
    host: "$1"
  help: Catalina session $3 total
  type: COUNTER

- pattern: ".*"
```

#### Start the Java application with the Prometheus exporter<a name="CloudWatch-Agent-PrometheusJava-start-start"></a>

Start the sample application\. This will emit Prometheus metrics to port 9404\. Be sure to replace the entry point `com.gubupt.sample.app.App` with the correct infromation for your sample java application\. 

On Linux, enter the following command\.

```
$ nohup java -javaagent:./jmx_prometheus_javaagent-0.14.0.jar=9404:./config.yaml -cp  ./SampleJavaApplication-1.0-SNAPSHOT.jar com.gubupt.sample.app.App &
```

On Windows, enter the following command\.

```
PS C:\> java -javaagent:.\jmx_prometheus_javaagent-0.14.0.jar=9404:.\config.yaml -cp  .\SampleJavaApplication-1.0-SNAPSHOT.jar com.gubupt.sample.app.App
```

#### Verify the Prometheus metrics emission<a name="CloudWatch-Agent-PrometheusJava-start-verify"></a>

Verify that Prometheus metrics are being emitted\. 

On Linux, enter the following command\.

```
$ curl localhost:9404
```

On Windows, enter the following command\.

```
PS C:\> curl  http://localhost:9404
```

Sample output on Linux:

```
StatusCode        : 200
StatusDescription : OK
Content           : # HELP jvm_classes_loaded The number of classes that are currently loaded in the JVM
                    # TYPE jvm_classes_loaded gauge
                    jvm_classes_loaded 2526.0
                    # HELP jvm_classes_loaded_total The total number of class...
RawContent        : HTTP/1.1 200 OK
                    Content-Length: 71908
                    Content-Type: text/plain; version=0.0.4; charset=utf-8
                    Date: Fri, 18 Dec 2020 16:38:10 GMT

                    # HELP jvm_classes_loaded The number of classes that are currentl...
Forms             : {}
Headers           : {[Content-Length, 71908], [Content-Type, text/plain; version=0.0.4; charset=utf-8], [Date, Fri, 18
                    Dec 2020 16:38:10 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : System.__ComObject
RawContentLength  : 71908
```

### Step 3: Configure the CloudWatch agent to scrape Prometheus metrics<a name="CloudWatch-Agent-PrometheusJava-agent"></a>

Next, set up the Prometheus scrape configuration in the CloudWatch agent configuration file\.

**To set up the Prometheus scrape configuration for the Java/JMX example**

1. Set up the configuration for `file_sd_config` and `static_config`\.

   On Linux, enter the following command\.

   ```
   $ cat /opt/aws/amazon-cloudwatch-agent/var/prometheus.yaml
   global:
     scrape_interval: 1m
     scrape_timeout: 10s
   scrape_configs:
     - job_name: jmx
       sample_limit: 10000
       file_sd_configs:
         - files: [ "/opt/aws/amazon-cloudwatch-agent/var/prometheus_file_sd.yaml" ]
   ```

   On Windows, enter the following command\.

   ```
   PS C:\ProgramData\Amazon\AmazonCloudWatchAgent> cat prometheus.yaml
   global:
     scrape_interval: 1m
     scrape_timeout: 10s
   scrape_configs:
     - job_name: jmx
       sample_limit: 10000
       file_sd_configs:
         - files: [ "C:\\ProgramData\\Amazon\\AmazonCloudWatchAgent\\prometheus_file_sd.yaml" ]
   ```

1. Set up the scrape targets configuration\.

   On Linux, enter the following command\.

   ```
   $ cat /opt/aws/amazon-cloudwatch-agent/var/prometheus_file_sd.yaml
   - targets:
     - 127.0.0.1:9404
     labels:
       application: sample_java_app
       os: linux
   ```

   On Windows, enter the following command\.

   ```
   PS C:\ProgramData\Amazon\AmazonCloudWatchAgent> cat prometheus_file_sd.yaml
   - targets:
     - 127.0.0.1:9404
     labels:
       application: sample_java_app
       os: windows
   ```

1. Set up the Prometheus scrape configuration by `ec2_sc_config`\. Replace *your\-ec2\-instance\-id* with the correct EC2 instance ID\.

   On Linux, enter the following command\.

   ```
   $ cat .\prometheus.yaml
   global:
     scrape_interval: 1m
     scrape_timeout: 10s
   scrape_configs:
     - job_name: jmx
       sample_limit: 10000
       ec2_sd_configs:
         - region: us-east-1
           port: 9404
           filters:
             - name: instance-id
               values:
                 - your-ec2-instance-id
   ```

   On Windows, enter the following command\.

   ```
   PS C:\ProgramData\Amazon\AmazonCloudWatchAgent> cat prometheus_file_sd.yaml
   - targets:
     - 127.0.0.1:9404
     labels:
       application: sample_java_app
       os: windows
   ```

1. Set up the CloudWatch agent configuration\. First, navigate to the correct directory\. On Linux, it is `/opt/aws/amazon-cloudwatch-agent/var/cwagent-config.json`\. On Windows, it is `C:\ProgramData\Amazon\AmazonCloudWatchAgent\cwagent-config.json`\.

   The following is a sample configuration with Java/JHX Prometheus metrics defined\. Be sure to replace *path\-to\-Prometheus\-Scrape\-Configuration\-file* with the correct path\.

   ```
   {
     "agent": {
       "region": "us-east-1"
     },
     "logs": {
       "metrics_collected": {
         "prometheus": {
           "cluster_name": "my-cluster",
           "log_group_name": "prometheus-test",
           "prometheus_config_path": "path-to-Prometheus-Scrape-Configuration-file",
           "emf_processor": {
             "metric_declaration_dedup": true,
             "metric_namespace": "PrometheusTest",
             "metric_unit":{
               "jvm_threads_current": "Count",
               "jvm_classes_loaded": "Count",
               "java_lang_operatingsystem_freephysicalmemorysize": "Bytes",
               "catalina_manager_activesessions": "Count",
               "jvm_gc_collection_seconds_sum": "Seconds",
               "catalina_globalrequestprocessor_bytesreceived": "Bytes",
               "jvm_memory_bytes_used": "Bytes",
               "jvm_memory_pool_bytes_used": "Bytes"
             },
             "metric_declaration": [
               {
                 "source_labels": ["job"],
                 "label_matcher": "^jmx$",
                 "dimensions": [["instance"]],
                 "metric_selectors": [
                   "^jvm_threads_current$",
                   "^jvm_classes_loaded$",
                   "^java_lang_operatingsystem_freephysicalmemorysize$",
                   "^catalina_manager_activesessions$",
                   "^jvm_gc_collection_seconds_sum$",
                   "^catalina_globalrequestprocessor_bytesreceived$"
                 ]
               },
               {
                 "source_labels": ["job"],
                 "label_matcher": "^jmx$",
                 "dimensions": [["area"]],
                 "metric_selectors": [
                   "^jvm_memory_bytes_used$"
                 ]
               },
               {
                 "source_labels": ["job"],
                 "label_matcher": "^jmx$",
                 "dimensions": [["pool"]],
                 "metric_selectors": [
                   "^jvm_memory_pool_bytes_used$"
                 ]
               }
             ]
           }
         }
       },
       "force_flush_interval": 5
     }
   }
   ```

1. Restart the CloudWatch agent by entering one of the following commands\.

   On Linux, enter the following command\.

   ```
   sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/var/cwagent-config.json
   ```

   On Windows, enter the following command\.

   ```
   & "C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent-ctl.ps1" -a fetch-config -m ec2 -s -c file:C:\ProgramData\Amazon\AmazonCloudWatchAgent\cwagent-config.json
   ```

### Viewing the Prometheus metrics and logs<a name="CloudWatch-Agent-PrometheusJava-view"></a>

You can now view the Java/JMX metrics being collected\.

**To view the metrics for your sample Java/JMX workload**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the Region where your cluster is running, choose **Metrics** in the left navigation pane\. Find the **PrometheusTest** namespace to see the metrics\.

1. To see the CloudWatch Logs events, choose **Log groups** in the navigation pane\. The events are in the log group **prometheus\-test**\.
