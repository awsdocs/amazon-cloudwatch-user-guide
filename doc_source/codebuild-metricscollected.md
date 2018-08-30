# AWS CodeBuild Metrics<a name="codebuild-metricscollected"></a>

AWS CodeBuild sends metrics to CloudWatch\. For more information, see [Monitoring AWS CodeBuild](http://docs.aws.amazon.com/codebuild/latest/userguide/monitoring-builds.html) in the *AWS CodeBuild User Guide*\.

## AWS CodeBuild CloudWatch Metrics<a name="cloudwatch_metrics-codebuild"></a>

 The following metrics can be tracked per AWS account, or per AWS build project\. 


****  

|   Metric   |   Description   | 
| --- | --- | 
|   Builds   |   Measures the number of builds triggered\.   Units: Count   Valid CloudWatch statistics: Sum   | 
|   Duration   |   Measures the duration of all builds over time\.   Units: Seconds   Valid CloudWatch statistics: Average \(recommended\), Maximum, Minimum   | 
|   SucceededBuilds   |   Measures the number of successful builds\.   Units: Count   Valid CloudWatch statistics: Sum   | 
|   FailedBuilds   |   Measures the number of builds that failed because of client error or because of a timeout\.   Units: Count   Valid CloudWatch statistics: Sum   | 

## AWS CodeBuild CloudWatch Dimensions<a name="codebuild-cloudwatch-dimensions"></a>

 `ProjectName ` is the only AWS CodeBuild metrics dimension\. If it is specified, then the metrics are for that project\. If it is not specified, then the metrics are for the current AWS account\. 


****  

|   Dimension   |   Description   | 
| --- | --- | 
|   ProjectName   |   If specified, then the metrics are for that project\. If not specified, then the metrics are for the AWS account\.   | 