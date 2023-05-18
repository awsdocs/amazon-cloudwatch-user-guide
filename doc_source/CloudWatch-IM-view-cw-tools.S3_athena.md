# Using Amazon Athena to query internet measurements in Amazon S3 log files<a name="CloudWatch-IM-view-cw-tools.S3_athena"></a>

By using Amazon Athena, you can query and view the internet measurements that Amazon CloudWatch Internet Monitor publishes to an Amazon S3 bucket\. It's an option in Internet Monitor to have Internet Monitor publish internet measurements for your application to an S3 bucket for internet\-facing traffic for your monitored city\-networks \(client locations and ASNs, typically internet service providers or ISPs\)\. Regardless of whether you choose to publish measurements to S3, Internet Monitor automatically publishes internet measurements to CloudWatch Logs every five minutes for the top 500 \(by traffic volume\) city\-networks for each monitor\. 

This chapter includes steps for how to create a table in Athena for internet measurements located in an S3 log file, and then provides [example queries](#CloudWatch-IM-view-cw-tools.S3_athena.athena-sample-queries) to see different views of the measurements\. For example, you can query for your top 10 impacted city\-networks by latency impact\.  

## Using Amazon Athena to create a table for internet measurements in Internet Monitor<a name="CloudWatch-IM-view-cw-tools.S3_athena.athena-queries"></a>

To get started using Athena with your Internet Monitor S3 log files, you first create a table for the internet measurements\.

Follow the steps in this procedure to create a table in Athena based on the S3 log files\. Then, you can run Athena queries on the table, such as [these example internet measurements queries](#CloudWatch-IM-view-cw-tools.S3_athena.athena-sample-queries), to get information about your measurements\.

**To create an Athena table**

1. Open the Athena console at [https://console.aws.amazon.com/athena/](https://console.aws.amazon.com/athena/)\.

1. In the Athena query editor, enter a query statement to generate a table with Internet Monitor internet measurements\. Replace the value for the LOCATION parameter with the location of S3 bucket where your Internet Monitor internet measurements are stored\. 

   ```
   CREATE EXTERNAL TABLE internet_measurements (
       version INT,
       timestamp INT,
       clientlocation STRING,
       servicelocation STRING,
       percentageoftotaltraffic DOUBLE,
       bytesin INT,
       bytesout INT,
       clientconnectioncount INT,
       internethealth STRING,
       trafficinsights STRING
   )
   PARTITIONED BY (year STRING, month STRING, day STRING)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   LOCATION
   's3://bucket_name/bucket_prefix/AWSLogs/account_id/internetmonitor/AWS_Region/'
   TBLPROPERTIES ('skip.header.line.count' = '1');
   ```

1. Enter a statement to create a partition to read the data\. For example, the following query creates a single partition for a specified date and location:

   ```
   ALTER TABLE internet_measurements
   ADD PARTITION (year = 'YYYY', month = 'MM', day = 'dd')
   LOCATION
   's3://bucket_name/bucket_prefix/AWSLogs/account_id/internetmonitor/AWS_Region/YYYY/MM/DD';
   ```

1. Choose **Run**\.

**Example Athena statements for internet measurements**

The following is an example of a statement to generate a table:

```
CREATE EXTERNAL TABLE internet_measurements (
    version INT,
    timestamp INT,
    clientlocation STRING,
    servicelocation STRING,
    percentageoftotaltraffic DOUBLE,
    bytesin INT,
    bytesout INT,
    clientconnectioncount INT,
    internethealth STRING,
    trafficinsights STRING
)
PARTITIONED BY (year STRING, month STRING, day STRING)
ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
LOCATION 's3://internet-measurements/TestMonitor/AWSLogs/1111222233332/internetmonitor/us-east-2/'
TBLPROPERTIES ('skip.header.line.count' = '1');
```

The following is an example of a statement to create a partition to read the data:

```
ALTER TABLE internet_measurements
ADD PARTITION (year = '2023', month = '04', day = '07')
LOCATION 's3://internet-measurements/TestMonitor/AWSLogs/1111222233332/internetmonitor/us-east-2/2023/04/07/'
```

## Sample Amazon Athena queries to use with internet measurements in Internet Monitor<a name="CloudWatch-IM-view-cw-tools.S3_athena.athena-sample-queries"></a>

This section includes example queries that you can use with Amazon Athena to get information about your application's internet measurements published to Amazon S3\.

**Query your top 10 impacted \(by total percentage of traffic\) client locations and ASNs**

Run this Athena query to return your top 10 impacted \(by total percentage of traffic\) city\-networks—that is, client locations and ASNs, typically internet service providers\. 

```
SELECT json_extract_scalar(clientLocation, '$.city') as city,
    json_extract_scalar(clientLocation, '$.networkname') as networkName,
    sum(percentageoftotaltraffic) as percentageoftotaltraffic
FROM internet_measurements
GROUP BY json_extract_scalar(clientLocation, '$.city'),
    json_extract_scalar(clientLocation, '$.networkname')
ORDER BY percentageoftotaltraffic desc
limit 10
```

**Query your top 10 impacted \(by availability\) client locations and ASNs **

Run this Athena query to return your top 10 impacted \(by total percentage of traffic\) city\-networks—that is, client locations and ASNs, typically internet service providers\. 

```
SELECT json_extract_scalar(clientLocation, '$.city') as city,
    json_extract_scalar(clientLocation, '$.networkname') as networkName,
    sum(
        cast(
            json_extract_scalar(
                internetHealth,
                '$.availability.percentageoftotaltrafficimpacted'
            )
        as double ) 
    ) as percentageOfTotalTrafficImpacted
FROM internet_measurements
GROUP BY json_extract_scalar(clientLocation, '$.city'),
    json_extract_scalar(clientLocation, '$.networkname')
ORDER BY percentageOfTotalTrafficImpacted desc
limit 10
```

**Query your top 10 impacted \(by latency\) client locations and ASNs **

Run this Athena query to return your top 10 impacted \(by latency impact\) city\-networks—that is, client locations and ASNs, typically internet service providers\. 

```
SELECT json_extract_scalar(clientLocation, '$.city') as city,
    json_extract_scalar(clientLocation, '$.networkname') as networkName,
    sum(
        cast(
            json_extract_scalar(
                internetHealth,
                '$.performance.percentageoftotaltrafficimpacted'
            )
        as double ) 
    ) as percentageOfTotalTrafficImpacted
FROM internet_measurements
GROUP BY json_extract_scalar(clientLocation, '$.city'),
    json_extract_scalar(clientLocation, '$.networkname')
ORDER BY percentageOfTotalTrafficImpacted desc
limit 10
```

**Query traffic highlights for your client locations and ASNs **

Run this Athena query to return traffic highlights, including availability score, performance score, and time to first byte for your city\-networks—that is, client locations and ASNs, typically internet service providers\. \.

```
SELECT json_extract_scalar(clientLocation, '$.city') as city,
    json_extract_scalar(clientLocation, '$.subdivision') as subdivision,
    json_extract_scalar(clientLocation, '$.country') as country,
    avg(cast(json_extract_scalar(internetHealth, '$.availability.experiencescore') as double)) as availabilityScore,
    avg(cast(json_extract_scalar(internetHealth, '$.performance.experiencescore') as double)) performanceScore,
    avg(cast(json_extract_scalar(trafficinsights, '$.timetofirstbyte.currentexperience.value') as double)) as averageTTFB,
    sum(bytesIn) as bytesIn,
    sum(bytesOut) as bytesOut,
    sum(bytesIn + bytesOut) as totalBytes
FROM internet_measurements
where json_extract_scalar(clientLocation, '$.city') != 'N/A'
GROUP BY 
json_extract_scalar(clientLocation, '$.city'),
    json_extract_scalar(clientLocation, '$.subdivision'),
    json_extract_scalar(clientLocation, '$.country')
ORDER BY totalBytes desc
limit 100
```

For more information about using Athena, see the [Amazon Athena User Guide](https://docs.aws.amazon.com/athena/latest/ug/)\.