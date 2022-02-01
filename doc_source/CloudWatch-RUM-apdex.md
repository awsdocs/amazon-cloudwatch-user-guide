# How CloudWatch RUM sets Apdex scores<a name="CloudWatch-RUM-apdex"></a>

Apdex \(Application Performance Index\) is an open standard that defines a method to report, benchmark, and rate application response time\. An Apdex score helps you understand and identify the impact on application performance over time\.

The Apdex score indicates the end users' level of satisfaction Scores range from 0 \(least satisfied\) to 1 \(most satisfied\)\. The scores are based on application performance only\. Users are not asked to rate the application\.

Each individual Apdex score falls into one of three thresholds\. Based on the Apdex threshold and actual application response time, there are three kinds of performance, as follows:
+ **Satisfied**– The actual application response time is less than or equal to the Apdex threshold\. For CloudWatch RUM, this threshold is 2000 ms or less\.
+ **Tolerable**– The actual application response time is greater than the Apdex threshold, but less than or equal to four times the Apdex threshold\. For CloudWatch RUM, this range is 2000–8000 ms\.
+ **Frustrating**– The actual application response time is greater than four times the Apdex threshold\. For CloudWatch RUM, this range is over 8000 ms\.

The total 0\-1 Apdex score is calculated using the following formula:

`(positive scores + tolerable scores/2)/total scores * 100`