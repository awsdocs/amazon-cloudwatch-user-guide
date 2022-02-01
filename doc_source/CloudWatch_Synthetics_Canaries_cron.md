# Scheduling canary runs using cron<a name="CloudWatch_Synthetics_Canaries_cron"></a>

Using a cron expression gives you flexibility when you schedule a canary\. Cron expressions contain five or six fields in the order listed in the following table\. The fields are separated by spaces\. The syntax differs depending on whether you are using the CloudWatch console to create the canary, or the AWS CLI or AWS SDKs\. When you use the console, you specify only the first five fields\. When you use the AWS CLI or AWS SDKs, you specify all six fields, and you must specify `*` for the `Year` field\.


| **Field** | **Allowed values** | **Allowed special characters** | 
| --- | --- | --- | 
|  Minutes  |  0\-59  |  , \- \* /  | 
|  Hours  |  0\-23  |  , \- \* /  | 
|  Day\-of\-month  |  1\-31  |  , \- \* ? / L W  | 
|  Month  |  1\-12 or JAN\-DEC  |  , \- \* /  | 
|  Day\-of\-week  |  1\-7 or SUN\-SAT  |  , \- \* ? L \#  | 
|  Year  |  \*   |    | 

**Special characters**
+ The **,** \(comma\) includes multiple values in the expression for a field\. For example, in the Month field, JAN,FEB,MAR would include January, February, and March\.
+ The **\-** \(dash\) special character specifies ranges\. In the Day field, 1\-15 would include days 1 through 15 of the specified month\.
+ The **\*** \(asterisk\) special character includes all values in the field\. In the Hours field, **\*** includes every hour\. You cannot use **\*** in both the Day\-of\-month and Day\-of\-week fields in the same expression\. If you use it in one, you must use **?** in the other\.
+ The **/** \(forward slash\) specifies increments\. In the Minutes field, you can enter 1/10 to specify every tenth minute, starting from the first minute of the hour \(for example, the eleventh, twenty\-first, and thirty\-first minute, and so on\)\.
+ The **?** \(question mark\) specifies one or another\. If you enter **7** in the Day\-of\-month field and you don't care what day of the week the seventh is, you can enter **?** in the Day\-of\-week field\.
+ The **L** wildcard in the Day\-of\-month or Day\-of\-week fields specifies the last day of the month or week\.
+ The **W** wildcard in the Day\-of\-month field specifies a weekday\. In the Day\-of\-month field, **3W** specifies the weekday closest to the third day of the month\.
+ The **\#** wildcard in the Day\-of\-week field specifies a certain instance of the specified day of the week within a month\. For example, 3\#2 is the second Tuesday of the month\. The 3 refers to Tuesday because it is the third day of each week, and the 2 refers to the second day of that type within the month\.

**Limitations**
+ You can't specify the Day\-of\-month and Day\-of\-week fields in the same cron expression\. If you specify a value or `*` \(asterisk\) in one of the fields, you must use a **?** \(question mark\) in the other\.
+ Cron expressions that lead to rates faster than one minute are not supported\.
+ You can't set a canary to wait for more than a year before running, so you can specify only `*` in the `Year` field\.

**Examples**  
You can refer to the following sample cron strings when you create a canary\. The following examples are the correct syntax for using the AWS CLI or AWS SDKs to create or update a canary\. If you are using the CloudWatch console, omit the final `*` in each example\.


| Expression | Meaning | 
| --- | --- | 
|  `0 10 * * ? *`  |  Run at 10:00 am \(UTC\) every day  | 
|  `15 12 * * ? *`  |  Run at 12:15 am \(UTC\) every day  | 
|  `0 18 ? * MON-FRI *`  |  Run at 6:00 am \(UTC\) every Monday through Friday  | 
|  `0 8 1 * ? *`  |  Run at 8:00 am \(UTC\) on the first day of each month  | 
|  `0/10 * ? * MON-SAT *`  |  Run every 10 minutes Monday through Saturday of each week  | 
|  `0/5 8-17 ? * MON-FRI *`  |  Run every five minutes Monday through Friday between 8:00 am and 5:55 pm \(UTC\)   | 