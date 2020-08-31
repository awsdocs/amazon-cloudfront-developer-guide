# Logging and Monitoring in Amazon CloudFront<a name="logging-and-monitoring"></a>

Monitoring is an important part of maintaining the availability and performance of CloudFront and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides several tools for monitoring your CloudFront resources and activity, and responding to potential incidents:

**Amazon CloudWatch Alarms**  
Using CloudWatch alarms, you watch a single metric over a time period that you specify\. If the metric exceeds a given threshold, a notification is sent to an Amazon SNS topic or AWS Auto Scaling policy\. CloudWatch alarms do not invoke actions when a metric is in a particular state\. Rather the state must have changed and been maintained for a specified number of periods\. For more information, see [Monitoring CloudFront with Amazon CloudWatch](monitoring-using-cloudwatch.md)\.

**AWS CloudTrail Logs**  
CloudTrail provides a record of actions taken by a user, role, or an AWS service in CloudFront\. Using the information collected by CloudTrail, you can determine the request that was made to CloudFront, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, see [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](logging_using_cloudtrail.md)\.

**CloudFront Access Logs**  
Server access logs provide detailed records about requests that are made to a distribution\. Server access logs are useful for many applications\. For example, access log information can be useful in security and access audits\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**CloudFront Console Reports**  
The CloudFront console includes a variety of reports, including the cache statistics report, the popular objects report, and the top referrers report\. Most CloudFront console reports are based on the data in CloudFront access logs, which contain detailed information about every user request that CloudFront receives\. However, you don't need to enable access logs to view the reports\. For more information, see [CloudFront Reports in the Console](reports.md)\.