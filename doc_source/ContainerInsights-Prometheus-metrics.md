# Prometheus metrics collected by the CloudWatch agent<a name="ContainerInsights-Prometheus-metrics"></a>

The CloudWatch agent with Prometheus support automatically collects metrics from several services and workloads\. The metrics that are collected by default are listed in the following sections\. You can also configure the agent to collect more metrics from these services, and to collect Prometheus metrics from other applications and services\. For more information about collecting additional metrics, see [ CloudWatch agent configuration for Prometheus](ContainerInsights-Prometheus-Setup-configure-ECS.md#ContainerInsights-Prometheus-Setup-cw-agent-config)\.

Prometheus metrics collected from Amazon EKS and Kubernetes clusters are in the **ContainerInsights/Prometheus** namespace\. Prometheus metrics collected from Amazon ECS clusters are in the **ECS/ContainerInsights/Prometheus** namespace\. 

**Topics**
+ [Prometheus metrics for App Mesh](#ContainerInsights-Prometheus-metrics-appmesh)
+ [Prometheus metrics for NGINX](#ContainerInsights-Prometheus-metrics-nginx)
+ [Prometheus metrics for Memcached](#ContainerInsights-Prometheus-metrics-memcached)
+ [Prometheus metrics for Java/JMX](#ContainerInsights-Prometheus-metrics-jmx)
+ [Prometheus metrics for HAProxy](#ContainerInsights-Prometheus-metrics-haproxy)

## Prometheus metrics for App Mesh<a name="ContainerInsights-Prometheus-metrics-appmesh"></a>

The following metrics are automatically collected from App Mesh \.

CloudWatch Container Insights can also collect App Mesh Envoy Access Logs\. For more information, see [\(Optional\) Enable App Mesh Envoy access logs](ContainerInsights-Prometheus-Sample-Workloads-appmesh-envoy.md)\. 

**Prometheus metrics for App Mesh on Amazon EKS and Kubernetes clusters**


| Metric name | Dimensions | 
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

**Prometheus metrics for App Mesh on Amazon ECS clusters**


| Metric name | Dimensions | 
| --- | --- | 
|  `envoy_http_downstream_rq_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_http_downstream_rq_xx` |  ClusterName, TaskDefinitionFamily | 
|  `envoy_cluster_upstream_cx_rx_bytes_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_cx_tx_bytes_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_membership_healthy` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_membership_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_server_memory_heap_size` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_server_memory_allocated` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_cx_connect_timeout` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_pending_failure_eject` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_pending_overflow` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_timeout` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_try_per_timeout` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_rx_reset` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_cx_destroy_local_with_active_rq` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_cx_destroy_remote_active_rq` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_maintenance_mode` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_flow_control_paused_reading_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_flow_control_resumed_reading_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_flow_control_backed_up_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_flow_control_drained_total` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_retry` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_retry_success` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_cluster_upstream_rq_retry_overflow` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_server_live` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_server_uptime` |  ClusterName, TaskDefinitionFamily  | 
|  `envoy_http_downstream_rq_xx` |  ClusterName, TaskDefinitionFamily, envoy\_http\_conn\_manager\_prefix, envoy\_response\_code\_class ClusterName, TaskDefinitionFamily, envoy\_response\_code\_class | 

**Note**  
`TaskDefinitionFamily` is the Kubernetes namespace of the mesh\.  
The value of `envoy_http_conn_manager_prefix` can be `ingress`, `egress`, or `admin`\.   
The value of `envoy_response_code_class` can be `1` \(stands for `1xx`\), `2` stands for `2xx`\), `3` stands for `3xx`\), `4` stands for `4xx`\), or `5` stands for `5xx`\)\. 

## Prometheus metrics for NGINX<a name="ContainerInsights-Prometheus-metrics-nginx"></a>

The following metrics are automatically collected from NGINX on Amazon EKS and Kubernetes clusters\.


| Metric name | Dimensions | 
| --- | --- | 
|  `nginx_ingress_controller_nginx_process_cpu_seconds_total` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_success` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_requests` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_nginx_process_connections` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_nginx_process_connections_total` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_nginx_process_resident_memory_bytes` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_config_last_reload_successful` |  ClusterName, Namespace, Service  | 
|  `nginx_ingress_controller_requests` |  ClusterName, Namespace, Service, status  | 

## Prometheus metrics for Memcached<a name="ContainerInsights-Prometheus-metrics-memcached"></a>

The following metrics are automatically collected from Memcached on Amazon EKS and Kubernetes clusters\.


| Metric name | Dimensions | 
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

## Prometheus metrics for Java/JMX<a name="ContainerInsights-Prometheus-metrics-jmx"></a>

**Metrics collected on Amazon EKS and Kubernetes clusters**

On Amazon EKS and Kubernetes clusters, Container Insights can collect the following predefined Prometheus metrics from the Java Virtual Machine \(JVM\), Java, and Tomcat \(Catalina\) using the JMX Exporter\. For more information, see [ prometheus/jmx\_exporter](https://github.com/prometheus/jmx_exporter) on Github\.

**Java/JMX on Amazon EKS and Kubernetes clusters**


| Metric name | Dimensions | 
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

**Tomcat/JMX on Amazon EKS and Kubernetes clusters**

In addition to the Java/JMX metrics in the previous table, the following metrics are also collected for the Tomcat workload\.


| Metric name | Dimensions | 
| --- | --- | 
|  `catalina_manager_activesessions` |  ClusterName, Namespace  | 
|  `catalina_manager_rejectedsessions` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_bytesreceived` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_bytessent` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_requestcount` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_errorcount` |  ClusterName, Namespace  | 
|  `catalina_globalrequestprocessor_processingtime` |  ClusterName, Namespace  | 

**Java/JMX on Amazon ECS clusters**


| Metric name | Dimensions | 
| --- | --- | 
|  `jvm_classes_loaded` |  ClusterName, TaskDefinitionFamily  | 
|  `jvm_threads_current` |  ClusterName, TaskDefinitionFamily  | 
|  `jvm_threads_daemon` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_totalswapspacesize` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_systemcpuload` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_processcpuload` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_freeswapspacesize` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_totalphysicalmemorysize` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_freephysicalmemorysize` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_openfiledescriptorcount` |  ClusterName, TaskDefinitionFamily  | 
|  `java_lang_operatingsystem_availableprocessors` |  ClusterName, TaskDefinitionFamily  | 
|  `jvm_memory_bytes_used` |  ClusterName, TaskDefinitionFamily, area  | 
|  `jvm_memory_pool_bytes_used` |  ClusterName, TaskDefinitionFamily, pool  | 

**Note**  
The values of the `area` dimension can be `heap` or `nonheap`\.  
The values of the `pool` dimension can be `Tenured Gen`, `Compress Class Space`, `Survivor Space`, `Eden Space`, `Code Cache`, or `Metaspace`\.

**Tomcat/JMX on Amazon ECS clusters**

In addition to the Java/JMX metrics in the previous table, the following metrics are also collected for the Tomcat workload on Amazon ECS clusters\.


| Metric name | Dimensions | 
| --- | --- | 
|  `catalina_manager_activesessions` |  ClusterName, TaskDefinitionFamily  | 
|  `catalina_manager_rejectedsessions` |  ClusterName, TaskDefinitionFamily  | 
|  `catalina_globalrequestprocessor_bytesreceived` |  ClusterName, TaskDefinitionFamily  | 
|  `catalina_globalrequestprocessor_bytessent` |  ClusterName, TaskDefinitionFamily  | 
|  `catalina_globalrequestprocessor_requestcount` |  ClusterName, TaskDefinitionFamily  | 
|  `catalina_globalrequestprocessor_errorcount` |  ClusterName, TaskDefinitionFamily  | 
|  `catalina_globalrequestprocessor_processingtime` |  ClusterName, TaskDefinitionFamily  | 

## Prometheus metrics for HAProxy<a name="ContainerInsights-Prometheus-metrics-haproxy"></a>

The following metrics are automatically collected from HAProxy on Amazon EKS and Kubernetes clusters\.

The metrics collected depend on which version of HAProxy Ingress that you are using\. For more information about HAProxy Ingress and its versions, see [ haproxy\-ingress](https://artifacthub.io/packages/helm/haproxy-ingress/haproxy-ingress)\.


| Metric name | Dimensions | Availability | 
| --- | --- | --- | 
|  `haproxy_backend_bytes_in_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_backend_bytes_out_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_backend_connection_errors_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_backend_connections_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_backend_current_sessions` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_backend_http_responses_total` |  ClusterName, Namespace, Service, code, backend  | All versions of HAProxy Ingress | 
|  `haproxy_backend_status` |  ClusterName, Namespace, Service  |  Only in versions 0\.10 or later of HAProxy Ingress  | 
|  `haproxy_backend_up` |  ClusterName, Namespace, Service  |  Only in versions of HAProxy Ingress earlier than 0\.10  | 
|  `haproxy_frontend_bytes_in_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_bytes_out_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_connections_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_current_sessions` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_http_requests_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_http_responses_total` |  ClusterName, Namespace, Service, code, frontend  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_request_errors_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 
|  `haproxy_frontend_requests_denied_total` |  ClusterName, Namespace, Service  | All versions of HAProxy Ingress | 

**Note**  
The values of the `code` dimension can be `1xx`, `2xx`, `3xx`, `4xx`, `5xx`, or `other`\.  
The values of the `backend` dimension can be:  
`http-default-backend`, `http-shared-backend`, or `httpsback-shared-backend` for HAProxy Ingress version 0\.0\.27 or earlier\.
`_default_backend` for HAProxy Ingress versions later than 0\.0\.27\.
The values of the `frontend` dimension can be:  
`httpfront-default-backend`, `httpfront-shared-frontend`, or `httpfronts` for HAProxy Ingress version 0\.0\.27 or earlier\.
`_front_http` or `_front_https` for HAProxy Ingress versions later than 0\.0\.27\.