# Monitoring CloudFront metrics with Amazon CloudWatch<a name="monitoring-using-cloudwatch"></a>

Amazon CloudFront is integrated with Amazon CloudWatch and automatically publishes operational metrics for distributions and [edge functions](https://aws.amazon.com/cloudfront/features/#Edge_Computing) \(both [Lambda@Edge and CloudFront Functions](edge-functions.md)\)\. Many of these metrics are displayed in a set of graphs in the [CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home), and are also accessible by using the CloudFront API or CLI\. All of these metrics are available in the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home) or through the CloudWatch API or CLI\. The CloudFront metrics don't count against [CloudWatch quotas \(formerly known as limits\)](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_limits.html) and don't incur any additional cost\.

In addition to the default metrics for CloudFront distributions, you can turn on additional metrics for an additional cost\. The additional metrics apply to CloudFront distributions, and must be turned on for each distribution separately\. For more information about the cost, see [Estimating cost for the additional CloudFront metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional-pricing)\.

Viewing these metrics can help you troubleshoot, track, and debug issues\. To view these metrics in the CloudFront console, see the [Monitoring page](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring)\. To view graphs about the activity for a specific CloudFront distribution or edge function, choose one, and then choose **View distribution metrics** or **View metrics**\.

You can also set alarms based on these metrics in the CloudFront console, or in the CloudWatch console, API, or CLI \([standard CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/) applies\)\. For example, you can set an alarm based on the `5xxErrorRate` metric, which represents the percentage of all viewer requests for which the response's HTTP status code is in the range of `500` to `599`, inclusive\. When the error rate reaches a certain value for a certain amount of time, for example, 5% of requests for 5 continuous minutes, the alarm is triggered\. You specify the alarm's value and its time unit when you create the alarm\. For more information, see [Creating alarms](receiving-notifications.md)\.

**Topics**
+ [Viewing CloudFront and edge function metrics](viewing-cloudfront-metrics.md)
+ [Creating alarms for metrics](receiving-notifications.md)
+ [Downloading metrics data in CSV format](cloudwatch-csv.md)
+ [Getting metrics using the CloudWatch API](programming-cloudwatch-metrics.md)