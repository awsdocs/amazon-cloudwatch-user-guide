# Prometheus Metrics Collected by the CloudWatch Agent<a name="ContainerInsights-Prometheus-metrics"></a>


****  

|  | 
| --- |
| Support for Prometheus metrics is in beta\. The beta is open to all AWS accounts and you do not need to request access\. Features may be added or changed before announcing General Availability\. Don’t hesitate to contact us with any feedback or let us know if you would like to be informed when updates are made by emailing us at [containerinsightsfeedback@amazon\.com](mailto:containerinsightsfeedback@amazon.com) | 

The CloudWatch agent with Prometheus support automatically collects metrics from several services and workloads\. The metrics that are collected by default are listed in the following sections\. You can also configure the agent to collect more metrics from these services, and to collect Prometheus metrics from other applications and services\. For more information about collecting additional metrics, see [ CloudWatch Agent Configuration for Prometheus](ContainerInsights-Prometheus-Setup-configure.md#ContainerInsights-Prometheus-Setup-cw-agent-config)\.

All Prometheus metrics are collected in the **ContainerInsights/Prometheus** namespace\. 

**Topics**
+ [Prometheus Metrics for App Mesh](#ContainerInsights-Prometheus-metrics-appmesh)
+ [Prometheus Metrics for NGINX](#ContainerInsights-Prometheus-metrics-nginx)
+ [Prometheus Metrics for memcached](#ContainerInsights-Prometheus-metrics-memcached)
+ [Prometheus Metrics for Java/JMX](#ContainerInsights-Prometheus-metrics-jmx)
+ [Prometheus Metrics for HAProxy](#ContainerInsights-Prometheus-metrics-haproxy)

## Prometheus Metrics for App Mesh<a name="ContainerInsights-Prometheus-metrics-appmesh"></a>

The following metrics are automatically collected from App Mesh\.

CloudWatch Container Insights can also collect App Mesh Envoy Access Logs\. For more information, see [\(Optional\) Enable App Mesh Envoy Access Logs](ContainerInsights-Prometheus-Sample-Workloads-appmesh-envoy.md)\. 


| Metric Name | Dimensions | 
| --- | --- | 
|  `envoy_http_downstream_rq_total` |  ClusterName, Namespace  | 
|  `envoy_http_downstream_rq_xx` |  ClusterName, Namespace ClusterName, Namespace, envoy\_http\_conn\_manager\_prefix, envoy\_response\_code\_class  | 
|  `envoy_cluster_upstream_cx_rx_bytes_total` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_cx_tx_bytes_total` |  ClusterName, Namespace  | 
|  `envoy_cluster_membership_healthy` |  ClusterName, Namespace  | 
|  `envoy_cluster_membership_total` |  ClusterName, Namespace  | 
|  `envoy_server_memory_heap_size` |  ClusterName, Namespace  | 
|  `envoy_server_memory_allocated` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_cx_connect_timeout` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_pending_failure_eject` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_pending_overflow` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_timeout` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_try_per_timeout` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_rx_reset` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_cx_destroy_local_with_active_rq` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_cx_destroy_remote_active_rq` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_maintenance_mode` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_flow_control_paused_reading_total` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_flow_control_resumed_reading_total` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_flow_control_backed_up_total` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_flow_control_drained_total` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_retry` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_retry_success` |  ClusterName, Namespace  | 
|  `envoy_cluster_upstream_rq_retry_overflow` |  ClusterName, Namespace  | 
|  `envoy_server_live` |  ClusterName, Namespace  | 
|  `envoy_server_uptime` |  ClusterName, Namespace  | 

## Prometheus Metrics for NGINX<a name="ContainerInsights-Prometheus-metrics-nginx"></a>

The following metrics are automatically collected from NGINX\.


| Metric Name | Dimensions | 
| --- | --- | 
|  `nginx_ingress_controller_nginx_process_cpu_seconds_total` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_success` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_requests` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_nginx_process_connections` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_nginx_process_connections_total` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_nginx_process_resident_memory_bytes` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_config_last_reload_successful` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_requests` |  ClusterName, Namespace, Service, status  | 

## Prometheus Metrics for memcached<a name="ContainerInsights-Prometheus-metrics-memcached"></a>

The following metrics are automatically collected from memcached\.


| Metric Name | Dimensions | 
| --- | --- | 
|  `memcached_current_items` |  ClusterName, Namespace, Service  | 
|  `memcached_current_connections` |  ClusterName, Namespace, Service  | 
|  `memcached_limit_bytes` |  ClusterName, Namespace, Service  | 
|  `memcached_current_bytes` |  ClusterName, Namespace, Service  | 
|  `memcached_written_bytes_total` |  ClusterName, Namespace, Service  | 
|  `memcached_read_bytes_total` |  ClusterName, Namespace, Service  | 
|  `memcached_items_evicted_total` |  ClusterName, Namespace, Service  | 
|  `memcached_items_reclaimed_total` |  ClusterName, Namespace, Service  | 
|  `memcached_commands_total` |  ClusterName, Namespace, Service ClusterName, Namespace, Service, command ClusterName, Namespace, Service, status, command  | 

## Prometheus Metrics for Java/JMX<a name="ContainerInsights-Prometheus-metrics-jmx"></a>

Container Insights can collect the following predefined Prometheus metrics from the Java Virtual Machine \(JVM\), Java, and Tomcat \(Catalina\) using the JMX Exporter\. For more information, see [ prometheus/jmx\_exporter](https://github.com/prometheus/jmx_exporter) on Github\.

**Java/JMX**


| Metric Name | Dimensions | 
| --- | --- | 
|  `jvm_classes_loaded` |  ClusterName, Namespace  | 
|  `jvm_threads_current` |  ClusterName, Namespace  | 
|  `jvm_threads_daemon` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_totalswapspacesize` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_systemcpuload` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_processcpuload` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_freeswapspacesize` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_totalphysicalmemorysize` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_freephysicalmemorysize` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_openfiledescriptorcount` |  ClusterName, Namespace  | 
|  `java_lang_operatingsystem_availableprocessors` |  ClusterName, Namespace  | 
|  `jvm_memory_bytes_used` |  ClusterName, Namespace, area  | 
|  `jvm_memory_pool_bytes_used` |  ClusterName, Namespace, pool  | 

**Note**  
The values of the `area` dimension can be `heap` or `nonheap`\.  
The values of the `pool` dimension can be `Tenured Gen`, `Compress Class Space`, `Survivor Space`, `Eden Space`, `Code Cache`, or `Metaspace`\.

**Tomcat/JMX**

In addition to the Java/JMX metrics in the previous table, the following metrics are also collected for the Tomcat workload\.


| Metric Name | Dimensions | 
| --- | --- | 
|  `catalina_manager_activesessions` |  ClusterName, Namespace  | 
|  `catalina_manager_rejectedsessions` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_bytesreceived` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_bytessent` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_requestcount` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_errorcount` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_processingtime` |  ClusterName, Namespace  | 

## Prometheus Metrics for HAProxy<a name="ContainerInsights-Prometheus-metrics-haproxy"></a>

The following metrics are automatically collected from HAProxy\.


| Metric Name | Dimensions | 
| --- | --- | 
|  `haproxy_backend_bytes_in_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_backend_bytes_out_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_backend_connection_errors_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_backend_connections_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_backend_current_sessions` |  ClusterName, Namespace, Service  | 
|  `haproxy_backend_http_responses_total` |  ClusterName, Namespace, Service, code, backend  | 
|  `haproxy_backend_up` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_bytes_in_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_bytes_out_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_connections_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_current_sessions` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_http_requests_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_http_responses_total` |  ClusterName, Namespace, Service, code, frontend  | 
|  `haproxy_frontend_request_errors_total` |  ClusterName, Namespace, Service  | 
|  `haproxy_frontend_requests_denied_total` |  ClusterName, Namespace, Service  | 

**Note**  
The values of the `code` dimension can be `1xx`, `2xx`, `3xx`, `4xx`, `5xx`, or `other`\.  
The values of the `backend` dimension can be `http-default-backend`, `http-shared-backend`, or `httpsback-shared-backend`\.  
The values of the `frontend` dimension can be `httpfront-default-backend`, `httpfront-shared-frontend`, or `httpfronts`\.