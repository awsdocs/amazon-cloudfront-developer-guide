# Reports, metrics, and logs<a name="reports-and-monitoring"></a>

CloudFront provides several options for reporting, monitoring, and logging your CloudFront resources:
+ You can view and download reports to see usage and activity for your CloudFront distributions, including billing reports, cache statistics, popular content, and top referrers\.
+ You can monitor and track CloudFront, including your [edge computing functions](https://aws.amazon.com/cloudfront/features/#Edge_Computing), directly in the CloudFront console or by using Amazon CloudWatch\. CloudFront sends various metrics to CloudWatch for distributions and edge functions, both Lambda@Edge and CloudFront Functions\.
+ You can view logs for the viewer requests that your CloudFront distributions receive with standard logs or real\-time logs\. In addition to viewer request logs, you can use CloudWatch Logs to get logs for your edge functions, both Lambda@Edge and CloudFront Functions\. You can also use AWS CloudTrail to get logs of the CloudFront API activity in your AWS account\.
+ You can track configuration changes to your CloudFront resources using AWS Config\.

For more information about each of these options, see the following topics\.

**Topics**
+ [AWS billing and usage reports for CloudFront](reports-billing.md)
+ [CloudFront reports in the console](reports.md)
+ [Monitoring CloudFront metrics with Amazon CloudWatch](monitoring-using-cloudwatch.md)
+ [CloudFront and edge function logging](logging.md)
+ [Tracking configuration changes with AWS Config](TrackingChanges.md)