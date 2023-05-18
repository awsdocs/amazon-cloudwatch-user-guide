# SAP NetWeaver<a name="appinsights-metrics-sap-netweaver"></a>

CloudWatch Application Insights supports the following metrics:


| Metric | Description | 
| --- | --- | 
| sap\_alerts\_ResponseTime |  The SAP response time alert from CCMS \(RZ20\)>R3Services>Dialog>ResponseTime\.  | 
|  sap\_alerts\_ResponseTimeDialog  | The SAP response time dialog alert from CCMS \(RZ20\)>R3Services>Dialog> ResponseTimeDialog\. | 
| sap\_alerts\_ResponseTimeDialogRFC | The SAP response time alert from CCMS \(RZ20\)>R3Services> Dialog>ResponseTimeDialogRFC\. | 
| sap\_alerts\_DBRequestTime | The SAP response time alert from CCMS \(RZ20\)>R3Services>Dialog>DBRequestTime\. | 
| sap\_alerts\_FrontendResponseTime | The SAP response time alert from CCMS \(RZ20\)>R3Services > Dialog>FrontEndResponseTime\. | 
| sap\_alerts\_Database  | The SAP system has logged database\-related errors\. Alert from SM21 or CCMS \(RZ20\)>R3Syslog>Database\. | 
| sap\_alerts\_QueueTime  | The SAP queue time alert from CCMS \(RZ20\)>R3Services>Dialog>QueueTime\. | 
| sap\_alerts\_AbortedJobs | Failed background jobs in SAP system\. Alert from \(RZ20\)>R3Services > Background>AbortedJobs\. | 
| sap\_alerts\_BasisSystem | SAP system logged system\-level errors\. Alert from SM21 or CCMS \(RZ20\)>R3Syslog>BasisSystem\. | 
| sap\_alerts\_Security  | The SAP system logged security\-related messages\. Alert from SM21 or CCMS \(RZ20\)>R3Syslog>Security\. | 
| sap\_alerts\_System  | The SAP system logged security or audit\-related messages\. Alert from SM21 or CCMS \(RZ20\)>Security>System\. | 
| sap\_alerts\_LongRunners  | There are long running programs in your SAP system\. Alert from CCMS \(RZ20\)>R3Services > Dialog>LongRunners\. | 
| sap\_alerts\_SqlError  | There are SAP database client layer error logs\. Alert from CCMS\(RZ20\)>DatabaseClient>AbapSql>SqlError\. | 
| sap\_alerts\_State  | State alert from CCMS \(RZ20\)>OS Collector>State\. | 
| sap\_alerts\_Shortdumps  | Shortdumps alert from ST22 and CCMS \(RZ20\)>R3Abap>Shortdumps\. | 
| sap\_alerts\_Availability  | Availability alert for SAP application server instance from SM21, SM50, SM51, SM66, and CCMS \(RZ20\)>InstanceAsTask>Availability\. | 
| sap\_dispatcher\_queue\_high | The SAPControl Web Service function GetQueueStatistic provides the dispatcher queue high count\.  | 
| sap\_dispatcher\_queue\_max | The SAPControl Web Service function GetQueueStatistic provides the dispatcher queue max count\.  | 
| sap\_dispatcher\_queue\_now | The SAPControl Web Service function GetQueueStatistic provides the dispatcher queue now count\. | 
| sap\_dispatcher\_queue\_reads | The SAPControl Web Service function GetQueueStatistic provides the dispatcher queue reads count\.  | 
| sap\_dispatcher\_queue\_writes | The SAPControl Web Service function GetQueueStatistic provides the dispatcher queue writes count\.  | 
| sap\_enqueue\_server\_arguments\_high  | The SAPControl Web Service function EnqGetStatistic provides the enqueue arguments high\. | 
| sap\_enqueue\_server\_arguments\_max  | The SAPControl Web Service function EnqGetStatistic provides the enqueue arguments max\. | 
| sap\_enqueue\_server\_arguments\_now | The SAPControl Web Service function EnqGetStatistic provides the enqueue arguments now\. | 
| sap\_enqueue\_server\_arguments\_state | The SAPControl Web Service function EnqGetStatistic provides the enqueue arguments state\. | 
| sap\_enqueue\_server\_backup\_requests | The SAPControl Web Service function EnqGetStatistic provides the enqueue backup requests\. | 
| sap\_enqueue\_server\_cleanup\_requests | The SAPControl Web Service function EnqGetStatistic provides the enqueue cleanup requests\.  | 
| sap\_enqueue\_server\_dequeue\_all\_requests | The SAPControl Web Service function EnqGetStatistic provides the dequeue all requests\. | 
| sap\_enqueue\_server\_dequeue\_errors | The SAPControl Web Service function EnqGetStatistic provides the dequeue errors\.  | 
| sap\_enqueue\_server\_dequeue\_requests  | The SAPControl Web Service function EnqGetStatistic provides the dequeue requests\. | 
| sap\_enqueue\_server\_enqueue\_errors | The SAPControl Web Service function EnqGetStatistic provides the enqueue errors\. | 
| sap\_enqueue\_server\_enqueue\_rejects  | The SAPControl Web Service function EnqGetStatistic provides the enqueue rejects\. | 
| sap\_enqueue\_server\_enqueue\_requests | The SAPControl Web Service function EnqGetStatistic provides the enqueue requests\. | 
| sap\_enqueue\_server\_lock\_time | The SAPControl Web Service function EnqGetStatistic provides the enqueue lock time\. | 
| sap\_enqueue\_server\_lock\_wait\_time | The SAPControl Web Service function EnqGetStatistic provides the enqueue lock wait time\. | 
| sap\_enqueue\_server\_locks\_high | The SAPControl Web Service function EnqGetStatistic provides the enqueue locks high\. | 
| sap\_enqueue\_server\_locks\_max | The SAPControl Web Service function EnqGetStatistic provides the enqueue locks max\. | 
| sap\_enqueue\_server\_locks\_now | The SAPControl Web Service function EnqGetStatistic provides the enqueue locks now\. | 
| sap\_enqueue\_server\_locks\_state | The SAPControl Web Service function EnqGetStatistic provides the enqueue locks state\. | 
| sap\_enqueue\_server\_owner\_high | The SAPControl Web Service function EnqGetStatistic provides the enqueue owner high\. | 
| sap\_enqueue\_server\_owner\_max | The SAPControl Web Service function EnqGetStatistic provides the enqueue owner max\. | 
| sap\_enqueue\_server\_owner\_now | The SAPControl Web Service function EnqGetStatistic provides the enqueue owner now\. | 
| sap\_enqueue\_server\_owner\_state | The SAPControl Web Service function EnqGetStatistic provides the enqueue owner state\. | 
| sap\_enqueue\_server\_replication\_state  | The SAPControl Web Service function EnqGetStatistic provides the enqueue replication state status\. | 
| sap\_enqueue\_server\_reporting\_requests | The SAPControl Web Service function EnqGetStatistic provides the reqporting requests status\. | 
| sap\_enqueue\_server\_server\_time | The SAPControl Web Service function EnqGetStatistic provides the enqueue server time\. | 
| sap\_HA\_check\_failover\_config\_state | The SAPControl Web Service function HACheckFailoverConfig provides the SAP High Availability status\. | 
| sap\_HA\_get\_failover\_config\_HAActive | The SAPControl Web Service function HAGetFailoverConfig provides the SAP High Availability Cluster configuration and status\. | 
| sap\_start\_service\_processes  | The SAPControl Web Service function GetProcessList provides the disp\+work, IGS, gwrd, icman, message server, and enqueue server processes status\. | 