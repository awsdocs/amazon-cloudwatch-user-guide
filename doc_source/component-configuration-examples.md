# Component configuration examples<a name="component-configuration-examples"></a>

The following examples show component configurations in JSON format for relevant services\.

**Topics**
+ [Amazon Elastic Compute Cloud \(EC2\) instance](component-configuration-examples-ec2.md)
+ [Amazon Relational Database Service instance](component-configuration-examples-rds.md)
+ [Amazon Relational Database Service \(RDS\) Aurora MySQL](component-configuration-examples-rds-aurora.md)
+ [Elastic Load Balancing \(ELB\)](component-configuration-examples-elb.md)
+ [Application Elastic Load Balancing](component-configuration-examples-application-elb.md)
+ [Amazon EC2 Auto Scaling \(ASG\)](component-configuration-examples-asg.md)
+ [Amazon Simple Queue Service \(SQS\)](component-configuration-examples-sqs.md)
+ [Customer\-grouped Amazon EC2 instances](component-configuration-examples-grouped-ec2.md)
+ [AWS Lambda Function](component-configuration-examples-lambda.md)
+ [Amazon DynamoDB table](component-configuration-examples-dynamo.md)
+ [SQL Always On Availability Group](component-configuration-examples-sql.md)
+ [SQL failover cluster instance](component-configuration-examples-sql-failover-cluster.md)
+ [RDS MariaDB and RDS MySQL](component-configuration-examples-mysql.md)
+ [RDS PostgreSQL](component-configuration-examples-rds-postgre-sql.md)
+ [Amazon S3 bucket](component-configuration-examples-s3.md)
+ [AWS Step Functions](component-configuration-examples-step-functions.md)
+ [API Gateway REST API stages](component-configuration-examples-api-gateway.md)
+ [Java](component-configuration-examples-java.md)
+ [RDS Oracle](component-configuration-examples-oracle.md)
+ [Amazon Elastic Container Service \(Amazon ECS\)](component-configuration-examples-ecs.md)
+ [Amazon ECS service](component-configuration-examples-ecs-service.md)
+ [Amazon ECS task](component-configuration-examples-ecs-task.md)
+ [Amazon EKS cluster](component-configuration-examples-eks-cluster.md)
+ [Kubernetes on Amazon EC2](component-configuration-examples-kubernetes-ec2.md)
+ [Amazon FSx](component-configuration-examples-fsx.md)
+ [Amazon SNS topic](component-configuration-examples-sns.md)
+ [SAP HANA on Amazon EC2](#component-configuration-examples-hana)
+ [SAP HANA High Availability on Amazon EC2](#component-configuration-examples-hana-ha)

## SAP HANA on Amazon EC2<a name="component-configuration-examples-hana"></a>

The following example shows a component configuration in JSON format for SAP HANA on Amazon EC2\.

```
{
  "subComponents": [
    {
      "subComponentType": "AWS::EC2::Instance",
      "alarmMetrics": [
        {
          "alarmMetricName": "hanadb_server_startup_time_variations_seconds",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_level_5_alerts_count",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_level_4_alerts_count",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_out_of_memory_events_count",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_max_trigger_read_ratio_percent",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_table_allocation_limit_used_percent",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_cpu_usage_percent",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_plan_cache_hit_ratio_percent",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_last_data_backup_age_days",
          "monitor": true
        }
      ],
      "logs": [
        {
          "logGroupName": "SAP_HANA_TRACE-my-resourge-group",
          "logPath": "/usr/sap/HDB/HDB00/*/trace/*.trc",
          "logType": "SAP_HANA_TRACE",
          "monitor": true,
          "encoding": "utf-8"
        },
        {
          "logGroupName": "SAP_HANA_LOGS-my-resource-group",
          "logPath": "/usr/sap/HDB/HDB00/*/trace/*.log",
          "logType": "SAP_HANA_LOGS",
          "monitor": true,
          "encoding": "utf-8"
        }
      ]
    }
  ],
  "hanaPrometheusExporter": {
    "hanaSid": "HDB",
    "hanaPort": "30013",
    "hanaSecretName": "HANA_DB_CREDS",
    "prometheusPort": "9668"
  }
}
```

## SAP HANA High Availability on Amazon EC2<a name="component-configuration-examples-hana-ha"></a>

The following example shows a component configuration in JSON format for SAP HANA High Availability on Amazon EC2\.

```
{
  "subComponents": [
    {
      "subComponentType": "AWS::EC2::Instance",
      "alarmMetrics": [
        {
          "alarmMetricName": "hanadb_server_startup_time_variations_seconds",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_level_5_alerts_count",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_level_4_alerts_count",
          "monitor": true
        },
        {
          "alarmMetricName": "hanadb_out_of_memory_events_count",
          "monitor": true
        },
        {
          "alarmMetricName": "ha_cluster_pacemaker_stonith_enabled",
          "monitor": true
        }
      ],
      "logs": [
        {
          "logGroupName": "SAP_HANA_TRACE-my-resourge-group",
          "logPath": "/usr/sap/HDB/HDB00/*/trace/*.trc",
          "logType": "SAP_HANA_TRACE",
          "monitor": true,
          "encoding": "utf-8"
        },
        {
          "logGroupName": "SAP_HANA_HIGH_AVAILABILITY-my-resource-group",
          "logPath": "/var/log/pacemaker/pacemaker.log",
          "logType": "SAP_HANA_HIGH_AVAILABILITY",
          "monitor": true,
          "encoding": "utf-8"
        }
      ]
    }
  ],
  "hanaPrometheusExporter": {
    "hanaSid": "HDB",
    "hanaPort": "30013",
    "hanaSecretName": "HANA_DB_CREDS",
    "prometheusPort": "9668"
  },
  "haClusterPrometheusExporter": {
    "prometheusPort": "9664"
  }
}
```