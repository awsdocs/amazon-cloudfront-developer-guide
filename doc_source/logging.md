# CloudFront and edge function logging<a name="logging"></a>

Amazon CloudFront provides different kinds of logging\. You can log the viewer requests that come to your CloudFront distributions, or you can log the CloudFront service activity \(API activity\) in your AWS account\. You can also get logs from your [edge computing](https://aws.amazon.com/cloudfront/features/#Edge_Computing) functions\.

## Logging requests<a name="logging-requests"></a>

CloudFront provides the following ways to log the requests that come to your distributions\.

**Standard logs \(access logs\)**  
CloudFront standard logs provide detailed records about every request that's made to a distribution\. These logs are useful for many scenarios, including security and access audits\.  
CloudFront standard logs are delivered to the Amazon S3 bucket of your choice\. CloudFront doesn't charge for standard logs, though you incur Amazon S3 charges for storing and accessing the log files\.  
For more information, see [Using standard logs \(access logs\)](AccessLogs.md)\.

**Real\-time logs**  
CloudFront real\-time logs provide information about requests made to a distribution, in real time \(log records are delivered within seconds of receiving the requests\)\. You can choose the *sampling rate* for your real\-time logs—that is, the percentage of requests for which you want to receive real\-time log records\. You can also choose the specific fields that you want to receive in the log records\.  
CloudFront real\-time logs are delivered to the data stream of your choice in Amazon Kinesis Data Streams\. CloudFront charges for real\-time logs, in addition to the charges you incur for using Kinesis Data Streams\.  
For more information, see [Real\-time logs](real-time-logs.md)\.

## Logging edge functions<a name="logging-edge-functions"></a>

You can use Amazon CloudWatch Logs to get logs for your [edge functions](edge-functions.md), both Lambda@Edge and CloudFront Functions\. You can access the logs using the CloudWatch console or the CloudWatch Logs API\. For more information, see [Edge function logs](edge-functions-logs.md)\.

## Logging service activity<a name="logging-service-activity"></a>

You can use AWS CloudTrail to log the CloudFront service activity \(API activity\) in your AWS account\. CloudTrail provides a record of API actions taken by a user, role, or AWS service in CloudFront\. Using the information collected by CloudTrail, you can determine the API request that was made to CloudFront, the IP address from which the request was made, who made the request, when it was made, and additional details\.

For more information, see [Capturing API requests with CloudTrail](logging_using_cloudtrail.md)\.

**Topics**
+ [Logging requests](#logging-requests)
+ [Logging edge functions](#logging-edge-functions)
+ [Logging service activity](#logging-service-activity)
+ [Using standard logs \(access logs\)](AccessLogs.md)
+ [Real\-time logs](real-time-logs.md)
+ [Edge function logs](edge-functions-logs.md)
+ [Capturing API requests with CloudTrail](logging_using_cloudtrail.md)