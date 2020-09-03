# Setting alarms to receive notifications<a name="receiving-notifications"></a>

In the CloudFront console, you can set alarms to notify you by Amazon Simple Notification Service \(Amazon SNS\) based on specific CloudFront metrics\. You can set an alarm on the [**Alarms** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/home#alarms:)\.

When you create an alarm in the console, you specify the following values:

**Metric**  
The metric for which you are creating the alarm\.

**Distribution**  
The CloudFront distribution for which you are creating the alarm\.

**Name of alarm**  
A name for the alarm\.

**Send a notification to**  
The Amazon SNS topic to send notification to if this metric triggers an alarm\.

**Whenever *<metric> <operator> <value>***  
Specify when CloudWatch should trigger an alarm and send a notification to the Amazon SNS topic\. For example, to receive a notification when the `5xx` error rate exceeds 1%, specify the following:  
**Whenever Average of 5xxErrorRate** **> 1**  
Note the following about specifying values:  
+ Enter only whole numbers without punctuation\. For example, to specify one thousand, enter **1000**\.
+ For `4xx`, `5xx`, and total error rates, the value that you specify is a percentage\.
+ For requests, bytes downloaded, and bytes uploaded, the value that you specify is units\. For example, 1073742000 bytes\.

**For at least *<number>* consecutive periods of *<time period>***  
Specify how many consecutive time periods of the specified duration the metric must meet the criteria before CloudWatch triggers an alarm\. When you choose a value, aim for an appropriate balance between a value that does not alarm for temporary or fleeting problems, but does alarm for sustained or real problems\.