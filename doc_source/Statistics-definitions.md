# CloudWatch statistics definitions<a name="Statistics-definitions"></a>

Statistics are metric data aggregations over specified periods of time\. When you graph or retrieve the statistics for a metric, you specify the *Period* of time, such as five minutes, to use to calculate each statistical value\. For example, if the **Period** is five minutes, the **Sum** is the sum of all sample values collected during the five\-minute period, while the **Minimum** is the lowest value collected during the five\-minute period\.

CloudWatch supports the following statistics for metrics\.
+ **SampleCount** is the number of data points during the period\.
+ **Sum** is sum of the values of the all data points collected during the period\.
+ **Average** is the value of `Sum/SampleCount` during the specified period\.
+ **Minimum** is the lowest value observed during the specified period\.
+ **Maximum** is the highest value observed during the specified period\.
+ **Percentile \(p\)** indicates the relative standing of a value in a dataset\. For example, **p95** is the 95th percentile and means that 95 percent of the data within the period is lower than this value and 5 percent of the data is higher than this value\. Percentiles help you get a better understanding of the distribution of your metric data\.
+ **Trimmed mean \(TM\)** is the mean of all values that are between two specified boundaries\. Values outside of the boundaries are ignored when the mean is calculated\. You define the boundaries as one or two numbers between 0 and 100, up to 10 decimal places\. The numbers can be absolute values or percentages\. For example, **tm90** calculates the average after removing the 10% of data points with the highest values\. **TM\(2%:98%\)** calculates the average after removing the 2% lowest data points and the 2% highest data points\. **TM\(150:1000\)** calculates the average after removing all data points that are lower than or equal to 150, or higher than 1000\.
+ **Interquartile mean \(IQM\)** is the trimmed mean of the *interquartile range*, or the middle 50% of values\. It is equivalent to **TM\(25%:75%\)**\.
+ **Winsorized mean \(WM\)** is similar to trimmed mean\. However, with winsorized mean, the values that are outside the boundary are not ignored, but instead are considered to be equal to the value at the edge of the appropriate boundary\. After this normalization, the average is calculated\. You define the boundaries as one or two numbers between 0 and 100, up to 10 decimal places\. For example, **wm98** calculates the average while treating the 2% of the highest values to be equal to the value at the 98th percentile\. **WM\(10%:90%\)** calculates the average while treating the highest 10% of data points to be the value of the 90% boundary, and treating the lowest 10% of data points to be the value of the 10% boundary\.
+ **Percentile rank \(PR\)** is the percentage of values that meet a fixed threshold\. For example, **PR\(:300\)** returns the percentage of data points that have a value of 300 or less\. **PR\(100:2000\)** returns the percentage of data points that have a value between 100 and 2000\.
+ **Trimmed count \(TC\)** is the number of data points in the chosen range for a trimmed mean statistic\. For example, **tc90** returns the number of data points not including any data points that fall in the highest 10% of the values\. **TC\(0\.005:0\.030\)** returns the number of data points with values between 0\.005 \(exclusive\) and 0\.030 \(inclusive\)\.
+ **Trimmed sum \(TS\)** is the sum of the values of data points in a chosen range for a trimmed mean statistic\. It is equivalent to \(Trimmed Mean\) \* \(Trimmed count\)\. For example, **ts90** returns the sum of the data points not including any data points that fall in the highest 10% of the values\. **TS\(80%:\)** returns the sum of the data point values, not including any data points with values in the lowest 80% of the range of values\.

**Note**  
For Trimmed Mean, Trimmed Count, Trimmed Sum, and Winsorized Mean, if you define two boundaries as fixed values instead of percentages, the calculation includes values equal to the higher boundary, but does not include values equal to the lower boundary\.

## Syntax<a name="Statistics-syntax"></a>

For Trimmed Mean, Trimmed Count, Trimmed Sum, and Winsorized Mean, the following syntax rules apply:
+ Using parentheses with one or two numbers with percent signs defines the boundaries to use as the values in the data set that fall in between the two percentiles that you specify\. For example, **TM\(10%:90%\)** uses only the values between the 10th and 90th percentiles\. **TM\(:95%\)** uses the values from the lowest end of the data set up to the 95th percentile, ignoring the 5% of data points with the highest values\. 
+ Using parenthesis with one or two numbers without percent signs defines the boundaries to use as the values in the data set that fall in between the explicit values that you specify\. For example, **TC\(80:500\)** uses only the values that are between 80 \(exclusive\) and 500 \(inclusive\)\. **TC\(:0\.5\)** uses only the values that equal 0\.5 or are lower\. 
+ Using one number without parentheses calculates using percentages, ignoring data points that are higher than the specified percentile\. For example, **tm99** calculates the mean while ignoring the 1% of the data points with the highest value\. It is the same as **TM\(:99%\)**\. 
+ Trimmed mean, Trimmed Count, Trimmed Sum, and Winsorized Mean can all be abbreviated using uppercase letters when specifying a range, such as **TM\(5%:95%\)**, **TM\(100:200\)**, or **TM\(:95%\)**\. They can only be abbreviated using lowercase letters when you specifying only one number, such as **tm99**\.

## Statistics use cases<a name="Statistics-usecases"></a>
+ **Trimmed mean** is most useful for metrics with a large sample size, such as webpage latency\. For example, **tm99** disregards extreme high outliers that could result from network problems or human errors, to give a more accurate number for the average latency of typical requests\. Similarly, **TM\(10%:\)** disregards the lowest 10% of latency values, such as those resulting from cache hits\. And **TM\(10%:99%\)** excludes both of these types of outliers\.
+ It is a good idea to keep watch on trimmed count whenever you are using trimmed mean, to make sure that the number of values being used in your trimmed mean calculations are enough to be statistically significant\.
+ Percentile rank enables you to put values into "bins" of ranges, and you can use this to manually create a histogram\. To do this, break your values down into various bins, such as **PR\(:1\)**, **PR\(1:5\)**, **PR\(5:10\)**, and **PR\(10:\)**\. Put each of these bins into a visualization as bar charts, and you have a histogram\.

## Percentiles versus trimmed mean<a name="Percentile-versus-Trimmed-Mean"></a>

A percentile such as **p99** and a trimmed mean such as **tm99** measure similar, but not identical values\. Both **p99** and **tm99** ignore the 1% of the data points with the highest values, which are considered outliers\. After that, **p99** is the **maximum value of the remaining 99%, while **tm99** is the *average* of the remaining 99%\. If you are looking at the latency of web requests, **p99** tells you the worst customer experience, ignoring outliers, while **tm99** tells you the average customer experience, ignoring outliers\.

Trimmed mean is a good latency statistic to watch if you are looking to optimize your customer experience\. For alarming on latency, we recomend to use a percentile statistic, to receive alerts early if there is an issue that leads to a partial loss of service\.

## Requirements to use percentiles, trimmed mean, and some other statistics<a name="Percentiles-trimmedmean-requirements"></a>

CloudWatch needs raw data points to calculate the following statistics:
+ Percentiles
+ Trimmed mean
+ Interquartile mean
+ Winsorized mean
+ Trimmed sum
+ Trimmed count
+ Percentile rank

If you publish data for a custom statistics using a statistic set instead of raw data, you can retrieve these types of statistics for this data only if one of the following conditions is true:
+ The SampleCount value of the statistic set is 1 and Min, Max, and Sum are all equal\.
+ The Min and Max are equal, and Sum is equal to Min multiplied by SampleCount\.

The following AWS services include metrics that support these types of statistics\.
+ API Gateway
+ Application Load Balancer
+ Amazon EC2
+ Elastic Load Balancing
+ Kinesis
+ Amazon RDS

Additionally, these type of statistics are not available for metrics when any of the metric values are negative numbers\.