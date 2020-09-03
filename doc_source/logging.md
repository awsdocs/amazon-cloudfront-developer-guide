# CloudFront logging<a name="logging"></a>

Amazon CloudFront provides different kinds of logging\. You can log the requests that come to your CloudFront distributions, or you can log the CloudFront service activity in your AWS account\.

## Logging requests<a name="logging-requests"></a>

CloudFront provides the following ways to log the requests that come to your distributions\.

**Standard logs \(access logs\)**  
CloudFront standard logs provide detailed records about every request that’s made to a distribution\. These logs are useful for many scenarios, including security and access audits\.  
CloudFront standard logs are delivered to the Amazon S3 bucket of your choice\. CloudFront doesn’t charge for standard logs, though you incur Amazon S3 charges for storing and accessing the log files\.  
For more information, see [Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**Real\-time logs**  
CloudFront real\-time logs provide information about requests made to a distribution, in real time \(log records are delivered within seconds of receiving the requests\)\. You can choose the *sampling rate* for your real\-time logs—that is, the percentage of requests for which you want to receive real\-time log records\. You can also choose the specific fields that you want to receive in the log records\.  
CloudFront real\-time logs are delivered to the data stream of your choice in Amazon Kinesis Data Streams\. CloudFront charges for real\-time logs, in addition to the charges you incur for using Kinesis Data Streams\.  
For more information, see [Real\-time logs](real-time-logs.md)\.

## Logging service activity<a name="logging-service-activity"></a>

You can use AWS CloudTrail to log the CloudFront service activity in your AWS account\. CloudTrail provides a record of actions taken by a user, role, or AWS service in CloudFront\. Using the information collected by CloudTrail, you can determine the request that was made to CloudFront, the IP address from which the request was made, who made the request, when it was made, and additional details\.

For more information, see [Capturing API Requests with CloudTrail](logging_using_cloudtrail.md)\.

**Topics**
+ [Logging requests](#logging-requests)
+ [Logging service activity](#logging-service-activity)
+ [Using Standard Logs \(Access Logs\)](AccessLogs.md)
+ [Real\-time logs](real-time-logs.md)
+ [Capturing API Requests with CloudTrail](logging_using_cloudtrail.md)