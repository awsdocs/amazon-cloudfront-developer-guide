# Monitoring CloudFront with Amazon CloudWatch<a name="monitoring-using-cloudwatch"></a>

Amazon CloudFront is integrated with Amazon CloudWatch, and automatically publishes 10 operational metrics per distribution, which are displayed in a set of graphs in the [CloudFront console](https://console.aws.amazon.com/cloudfront/home), or accessible by using the CloudFront API or CLI\. Some of these metrics apply to CloudFront distributions, and others apply to Lambda@Edge functions\. These metrics don’t count against [CloudWatch limits](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_limits.html) and don’t incur any additional cost\.

In addition to the 10 default metrics, you can enable additional metrics for an additional cost\. The additional metrics apply to CloudFront distributions, and must be enabled for each distribution separately\. For more information about CloudWatch pricing, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing/)\.

Viewing these metrics can help you troubleshoot, track, and debug issues\. To view these metrics, see the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\. To view graphs about the activity for a specific CloudFront distribution or Lambda@Edge function, choose one, and then choose to view the metrics\.

You can also set alarms based on these metrics in the CloudFront console, or in the CloudWatch console, API, or CLI \([standard CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing/) applies\)\. For example, you can set an alarm based on the `5xxErrorRate` metric, which represents the percentage of all viewer requests for which the response’s HTTP status code is in the `5xx` range\. When the error rate reaches a certain value for a certain amount of time—for example, 5% of requests for 5 continuous minutes—the alarm is triggered\. You specify the alarm’s value and its time unit when you create the alarm\. For more information, see [Setting Alarms](receiving-notifications.md)\.

**Topics**
+ [Viewing CloudFront and Lambda@Edge Metrics](viewing-cloudfront-metrics.md)
+ [Setting Alarms to Receive Notifications](receiving-notifications.md)
+ [Downloading Data in CSV Format](cloudwatch-csv.md)
+ [Getting CloudWatch Metrics for CloudFront Programmatically](programming-cloudwatch-metrics.md)