# Viewing CloudFront and Lambda@Edge Metrics<a name="viewing-cloudfront-metrics"></a>

You can view operational metrics about your CloudFront distributions and Lambda@Edge functions in the CloudFront console\. To view these metrics, see the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\. To view graphs about the activity for a specific CloudFront distribution or Lambda@Edge function, choose one, and then choose to view the metrics\.

**Topics**
+ [Viewing the Default CloudFront Distribution Metrics](#monitoring-console.distributions)
+ [Viewing Additional CloudFront Distribution Metrics](#monitoring-console.distributions-additional)
+ [Viewing the Default Lambda@Edge Function Metrics](#monitoring-console.lambda-at-edge)

## Viewing the Default CloudFront Distribution Metrics<a name="monitoring-console.distributions"></a>

The following default metrics are included for all CloudFront distributions, at no additional cost:

**Requests**  
The total number of viewer requests received by CloudFront, for all HTTP methods and for both HTTP and HTTPS requests\.

**Bytes downloaded**  
The total number of bytes downloaded by viewers for `GET`, `HEAD`, and `OPTIONS` requests\.

**Bytes uploaded**  
The total number of bytes that viewers uploaded to your origin with CloudFront, using `POST` and `PUT` requests\.

**4xx error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `4xx`\.

**5xx error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `5xx`\.

**Total error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `4xx` or `5xx`\.

These metrics are shown in graphs for each CloudFront distribution on the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\. On each graph, the totals are displayed at 1\-minute granularity\. In addition to viewing the graphs, you can also [download metrics reports as CSV files](cloudwatch-csv.md)\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh interval\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

## Viewing Additional CloudFront Distribution Metrics<a name="monitoring-console.distributions-additional"></a>

In addition to the default metrics, you can enable additional metrics for an additional cost\. For more information about the cost, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing/)\. The following additional metrics must be enabled for each distribution separately:

**Cache hit rate**  
The percentage of all cacheable requests for which CloudFront served the content from its cache\. HTTP `POST` and `PUT` requests, and errors, are not considered cacheable requests\.

**Origin latency**  
The total time spent from when CloudFront receives a request to when it starts providing a response to the network \(not the viewer\), for requests that are served from the origin, not the CloudFront cache\. This is also known as *first byte latency*, or *time\-to\-first\-byte*\.

**Error rate by status code**  
The percentage of all viewer requests for which the response’s HTTP status code is a particular code in the `4xx` or `5xx` range\. This metric is available for all of the following error codes: `401`, `403`, `404`, `502`, `503`, and `504`\.

**To enable additional metrics**

1. Sign in to the AWS Management Console and open the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\.

1. Choose the distribution to enable additional metrics for, and then choose **View distribution metrics**\.

1. Choose **Enable additional metrics**\.

1. In the **Enable additional metrics** window, choose **Enable**, and then choose **Save**\.

After you enable the additional metrics, they are shown in graphs\. On each graph, the totals are displayed at 1\-minute granularity\. In addition to viewing the graphs, you can also [download metrics reports as CSV files](cloudwatch-csv.md)\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh interval\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

## Viewing the Default Lambda@Edge Function Metrics<a name="monitoring-console.lambda-at-edge"></a>

The following default metrics are included for all Lambda@Edge functions, at no additional cost:
+ 5xx error rate for Lambda@Edge
+ Lambda execution errors
+ Lambda invalid responses
+ Lambda throttles

These metrics are shown in graphs for each Lambda@Edge function on the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\. The graphs include the number of invocations, errors, throttles, and so on\. On each graph, the totals are displayed at 1\-minute granularity, grouped by AWS Region\.

If you see a spike in errors that you want to investigate, you can choose a function and then view log files by AWS Region, until you determine which function is causing the problems and in which AWS Region\. For more information about troubleshooting Lambda@Edge errors, see:
+  [How to Determine the Type of Failure](lambda-edge-testing-debugging.md#lambda-edge-testing-debugging-failure-type)
+ [Four Steps for Debugging your Content Delivery on AWS](https://aws-blogs-prod.amazon.com/networking-and-content-delivery/four-steps-for-debugging-your-content-delivery-on-aws/)

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh interval\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view the graphs in the CloudWatch console, choose **Add to dashboard**\.