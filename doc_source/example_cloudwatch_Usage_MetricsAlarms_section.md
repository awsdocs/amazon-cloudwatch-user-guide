# Manage CloudWatch metrics and alarms using an AWS SDK<a name="example_cloudwatch_Usage_MetricsAlarms_section"></a>

The following code example shows how to:
+ Create an alarm to watch a CloudWatch metric\.
+ Put data into a metric and trigger the alarm\.
+ Get data from the alarm\.
+ Delete the alarm\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/cloudwatch#code-examples)\. 
Create a class that wraps CloudWatch operations\.  

```
from datetime import datetime, timedelta
import logging
from pprint import pprint
import random
import time
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


class CloudWatchWrapper:
    """Encapsulates Amazon CloudWatch functions."""
    def __init__(self, cloudwatch_resource):
        """
        :param cloudwatch_resource: A Boto3 CloudWatch resource.
        """
        self.cloudwatch_resource = cloudwatch_resource

    def put_metric_data_set(self, namespace, name, timestamp, unit, data_set):
        """
        Sends a set of data to CloudWatch for a metric. All of the data in the set
        have the same timestamp and unit.

        :param namespace: The namespace of the metric.
        :param name: The name of the metric.
        :param timestamp: The UTC timestamp for the metric.
        :param unit: The unit of the metric.
        :param data_set: The set of data to send. This set is a dictionary that
                         contains a list of values and a list of corresponding counts.
                         The value and count lists must be the same length.
        """
        try:
            metric = self.cloudwatch_resource.Metric(namespace, name)
            metric.put_data(
                Namespace=namespace,
                MetricData=[{
                    'MetricName': name,
                    'Timestamp': timestamp,
                    'Values': data_set['values'],
                    'Counts': data_set['counts'],
                    'Unit': unit}])
            logger.info("Put data set for metric %s.%s.", namespace, name)
        except ClientError:
            logger.exception("Couldn't put data set for metric %s.%s.", namespace, name)
            raise

    def create_metric_alarm(
            self, metric_namespace, metric_name, alarm_name, stat_type, period,
            eval_periods, threshold, comparison_op):
        """
        Creates an alarm that watches a metric.

        :param metric_namespace: The namespace of the metric.
        :param metric_name: The name of the metric.
        :param alarm_name: The name of the alarm.
        :param stat_type: The type of statistic the alarm watches.
        :param period: The period in which metric data are grouped to calculate
                       statistics.
        :param eval_periods: The number of periods that the metric must be over the
                             alarm threshold before the alarm is set into an alarmed
                             state.
        :param threshold: The threshold value to compare against the metric statistic.
        :param comparison_op: The comparison operation used to compare the threshold
                              against the metric.
        :return: The newly created alarm.
        """
        try:
            metric = self.cloudwatch_resource.Metric(metric_namespace, metric_name)
            alarm = metric.put_alarm(
                AlarmName=alarm_name,
                Statistic=stat_type,
                Period=period,
                EvaluationPeriods=eval_periods,
                Threshold=threshold,
                ComparisonOperator=comparison_op)
            logger.info(
                "Added alarm %s to track metric %s.%s.", alarm_name, metric_namespace,
                metric_name)
        except ClientError:
            logger.exception(
                "Couldn't add alarm %s to metric %s.%s", alarm_name, metric_namespace,
                metric_name)
            raise
        else:
            return alarm

    def put_metric_data(self, namespace, name, value, unit):
        """
        Sends a single data value to CloudWatch for a metric. This metric is given
        a timestamp of the current UTC time.

        :param namespace: The namespace of the metric.
        :param name: The name of the metric.
        :param value: The value of the metric.
        :param unit: The unit of the metric.
        """
        try:
            metric = self.cloudwatch_resource.Metric(namespace, name)
            metric.put_data(
                Namespace=namespace,
                MetricData=[{
                    'MetricName': name,
                    'Value': value,
                    'Unit': unit
                }]
            )
            logger.info("Put data for metric %s.%s", namespace, name)
        except ClientError:
            logger.exception("Couldn't put data for metric %s.%s", namespace, name)
            raise

    def get_metric_statistics(self, namespace, name, start, end, period, stat_types):
        """
        Gets statistics for a metric within a specified time span. Metrics are grouped
        into the specified period.

        :param namespace: The namespace of the metric.
        :param name: The name of the metric.
        :param start: The UTC start time of the time span to retrieve.
        :param end: The UTC end time of the time span to retrieve.
        :param period: The period, in seconds, in which to group metrics. The period
                       must match the granularity of the metric, which depends on
                       the metric's age. For example, metrics that are older than
                       three hours have a one-minute granularity, so the period must
                       be at least 60 and must be a multiple of 60.
        :param stat_types: The type of statistics to retrieve, such as average value
                           or maximum value.
        :return: The retrieved statistics for the metric.
        """
        try:
            metric = self.cloudwatch_resource.Metric(namespace, name)
            stats = metric.get_statistics(
                StartTime=start, EndTime=end, Period=period, Statistics=stat_types)
            logger.info(
                "Got %s statistics for %s.", len(stats['Datapoints']), stats['Label'])
        except ClientError:
            logger.exception("Couldn't get statistics for %s.%s.", namespace, name)
            raise
        else:
            return stats

    def get_metric_alarms(self, metric_namespace, metric_name):
        """
        Gets the alarms that are currently watching the specified metric.

        :param metric_namespace: The namespace of the metric.
        :param metric_name: The name of the metric.
        :returns: An iterator that yields the alarms.
        """
        metric = self.cloudwatch_resource.Metric(metric_namespace, metric_name)
        alarm_iter = metric.alarms.all()
        logger.info("Got alarms for metric %s.%s.", metric_namespace, metric_name)
        return alarm_iter

    def delete_metric_alarms(self, metric_namespace, metric_name):
        """
        Deletes all of the alarms that are currently watching the specified metric.

        :param metric_namespace: The namespace of the metric.
        :param metric_name: The name of the metric.
        """
        try:
            metric = self.cloudwatch_resource.Metric(metric_namespace, metric_name)
            metric.alarms.delete()
            logger.info(
                "Deleted alarms for metric %s.%s.", metric_namespace, metric_name)
        except ClientError:
            logger.exception(
                "Couldn't delete alarms for metric %s.%s.", metric_namespace,
                metric_name)
            raise
```
Use the wrapper class to put data in a metric, trigger an alarm that watches the metric, and get data from the alarm\.  

```
def usage_demo():
    print('-'*88)
    print("Welcome to the Amazon CloudWatch metrics and alarms demo!")
    print('-'*88)

    logging.basicConfig(level=logging.INFO, format='%(levelname)s: %(message)s')

    cw_wrapper = CloudWatchWrapper(boto3.resource('cloudwatch'))

    minutes = 20
    metric_namespace = 'doc-example-metric'
    metric_name = 'page_views'
    start = datetime.utcnow() - timedelta(minutes=minutes)
    print(f"Putting data into metric {metric_namespace}.{metric_name} spanning the "
          f"last {minutes} minutes.")
    for offset in range(0, minutes):
        stamp = start + timedelta(minutes=offset)
        cw_wrapper.put_metric_data_set(
            metric_namespace, metric_name, stamp, 'Count', {
                'values': [
                    random.randint(bound, bound * 2)
                    for bound in range(offset + 1, offset + 11)],
                'counts': [random.randint(1, offset + 1) for _ in range(10)]
            })

    alarm_name = 'high_page_views'
    period = 60
    eval_periods = 2
    print(f"Creating alarm {alarm_name} for metric {metric_name}.")
    alarm = cw_wrapper.create_metric_alarm(
        metric_namespace, metric_name, alarm_name, 'Maximum', period, eval_periods,
        100, 'GreaterThanThreshold')
    print(f"Alarm ARN is {alarm.alarm_arn}.")
    print(f"Current alarm state is: {alarm.state_value}.")

    print(f"Sending data to trigger the alarm. This requires data over the threshold "
          f"for {eval_periods} periods of {period} seconds each.")
    while alarm.state_value == 'INSUFFICIENT_DATA':
        print("Sending data for the metric.")
        cw_wrapper.put_metric_data(
            metric_namespace, metric_name, random.randint(100, 200), 'Count')
        alarm.load()
        print(f"Current alarm state is: {alarm.state_value}.")
        if alarm.state_value == 'INSUFFICIENT_DATA':
            print(f"Waiting for {period} seconds...")
            time.sleep(period)
        else:
            print("Wait for a minute for eventual consistency of metric data.")
            time.sleep(period)
            if alarm.state_value == 'OK':
                alarm.load()
                print(f"Current alarm state is: {alarm.state_value}.")

    print(f"Getting data for metric {metric_namespace}.{metric_name} during timespan "
          f"of {start} to {datetime.utcnow()} (times are UTC).")
    stats = cw_wrapper.get_metric_statistics(
        metric_namespace, metric_name, start, datetime.utcnow(), 60,
        ['Average', 'Minimum', 'Maximum'])
    print(f"Got {len(stats['Datapoints'])} data points for metric "
          f"{metric_namespace}.{metric_name}.")
    pprint(sorted(stats['Datapoints'], key=lambda x: x['Timestamp']))

    print(f"Getting alarms for metric {metric_name}.")
    alarms = cw_wrapper.get_metric_alarms(metric_namespace, metric_name)
    for alarm in alarms:
        print(f"Alarm {alarm.name} is currently in state {alarm.state_value}.")

    print(f"Deleting alarms for metric {metric_name}.")
    cw_wrapper.delete_metric_alarms(metric_namespace, metric_name)

    print("Thanks for watching!")
    print('-'*88)
```
+ For API details, see the following topics in *AWS SDK for Python \(Boto3\) API Reference*\.
  + [DeleteAlarms](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/DeleteAlarms)
  + [DescribeAlarmsForMetric](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/DescribeAlarmsForMetric)
  + [DisableAlarmActions](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/DisableAlarmActions)
  + [EnableAlarmActions](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/EnableAlarmActions)
  + [GetMetricStatistics](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/GetMetricStatistics)
  + [ListMetrics](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/ListMetrics)
  + [PutMetricAlarm](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/PutMetricAlarm)
  + [PutMetricData](https://docs.aws.amazon.com/goto/boto3/monitoring-2010-08-01/PutMetricData)

------

For a complete list of AWS SDK developer guides and code examples, see [Using CloudWatch with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.