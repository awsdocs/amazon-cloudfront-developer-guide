# Viewing CloudFront and Lambda@Edge metrics<a name="viewing-cloudfront-metrics"></a>

You can view operational metrics about your CloudFront distributions and Lambda@Edge functions in the CloudFront console\. To view these metrics, see the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\. To view graphs about the activity for a specific CloudFront distribution or Lambda@Edge function, choose one, and then choose to view the metrics\.

**Topics**
+ [Viewing the default CloudFront distribution metrics](#monitoring-console.distributions)
+ [Viewing additional CloudFront distribution metrics](#monitoring-console.distributions-additional)
+ [Viewing the default Lambda@Edge function metrics](#monitoring-console.lambda-at-edge)

## Viewing the default CloudFront distribution metrics<a name="monitoring-console.distributions"></a>

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
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh rate\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

## Viewing additional CloudFront distribution metrics<a name="monitoring-console.distributions-additional"></a>

In addition to the default metrics, you can enable additional metrics for an additional cost\. For more information about the cost, see [Estimating cost for the additional CloudFront metrics](#monitoring-console.distributions-additional-pricing)\.

These additional metrics must be enabled for each distribution separately:

**Cache hit rate**  
The percentage of all cacheable requests for which CloudFront served the content from its cache\. HTTP `POST` and `PUT` requests, and errors, are not considered cacheable requests\.

**Origin latency**  
The total time spent from when CloudFront receives a request to when it starts providing a response to the network \(not the viewer\), for requests that are served from the origin, not the CloudFront cache\. This is also known as *first byte latency*, or *time\-to\-first\-byte*\.

**Error rate by status code**  
The percentage of all viewer requests for which the response’s HTTP status code is a particular code in the `4xx` or `5xx` range\. This metric is available for all of the following error codes: `401`, `403`, `404`, `502`, `503`, and `504`\.

### Enabling additional metrics<a name="enable-metrics"></a>

You can enable additional metrics in the CloudFront console, with the AWS Command Line Interface \(AWS CLI\), or with the CloudFront API\.

#### Enabling additional metrics \(console\)<a name="enable-metrics-console"></a>

**To enable additional metrics**

1. Sign in to the AWS Management Console and open the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring)\.

1. Choose the distribution to enable additional metrics for, and then choose **View distribution metrics**\.

1. Choose **Enable additional metrics**\.

1. In the **Enable additional metrics** window, choose **Enable**, and then choose **Save**\.

After you enable the additional metrics, they are shown in graphs\. On each graph, the totals are displayed at 1\-minute granularity\. In addition to viewing the graphs, you can also [download metrics reports as CSV files](cloudwatch-csv.md)\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh rate\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

#### Enabling additional metrics \(AWS CLI\)<a name="enable-metrics-cli"></a>

To manage additional metrics with the AWS Command Line Interface \(AWS CLI\), use one of the following commands\.

**To enable additional metrics for a distribution**
+ Use the create\-monitoring\-subscription command, as in the following example\. Replace *EDFDVBD6EXAMPLE* with the ID of the distribution that you are enabling additional metrics for\.

  ```
  aws cloudfront create-monitoring-subscription --distribution-id EDFDVBD6EXAMPLE --monitoring-subscription RealtimeMetricsSubscriptionConfig={RealtimeMetricsSubscriptionStatus=Enabled}
  ```

**To see whether additional metrics are enabled for a distribution**
+ Use the get\-monitoring\-subscription command, as in the following example\. Replace *EDFDVBD6EXAMPLE* with the ID of the distribution that you are checking\.

  ```
  aws cloudfront get-monitoring-subscription --distribution-id EDFDVBD6EXAMPLE
  ```

**To disable additional metrics for a distribution**
+ Use the delete\-monitoring\-subscription command, as in the following example\. Replace *EDFDVBD6EXAMPLE* with the ID of the distribution that you are disabling additional metrics for\.

  ```
  aws cloudfront delete-monitoring-subscription --distribution-id EDFDVBD6EXAMPLE
  ```

#### Enabling additional metrics \(API\)<a name="enable-metrics-api"></a>

To manage additional metrics with the CloudFront API, use one of the following API operations\.
+ To enable additional metrics for a distribution, use [CreateMonitoringSubscription](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateMonitoringSubscription.html)\.
+ To see whether additional metrics are enabled for a distribution, use [GetMonitoringSubscription](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetMonitoringSubscription.html)\.
+ To disable additional metrics for a distribution, use [DeleteMonitoringSubscription](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteMonitoringSubscription.html)\.

For more information about these API calls, see the API reference documentation for your AWS SDK or other API client\.

### Estimating cost for the additional CloudFront metrics<a name="monitoring-console.distributions-additional-pricing"></a>

When you enable additional metrics for a distribution, CloudFront sends up to 8 metrics to CloudWatch in the US East \(N\. Virginia\) Region\. CloudWatch charges a low, fixed rate for each metric\. This rate is charged only once per month, per metric \(up to 8 metrics per distribution\)\. This is a fixed rate, so your cost remains the same regardless of the number of requests or responses that the CloudFront distribution receives or sends\. For the per\-metric rate, see the [Amazon CloudWatch pricing page](http://aws.amazon.com/cloudwatch/pricing/) and the [CloudWatch pricing calculator](http://aws.amazon.com/cloudwatch/pricing/#Pricing_calculator)\. Additional API charges apply when you retrieve the metrics with the CloudWatch API\.

## Viewing the default Lambda@Edge function metrics<a name="monitoring-console.lambda-at-edge"></a>

The following default metrics are shown in graphs for each Lambda@Edge function on the [**Monitoring** page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v2/home?#/monitoring):
+ `5xx` error rate for Lambda@Edge
+ Lambda execution errors
+ Lambda invalid responses
+ Lambda throttles

The graphs include the number of invocations, errors, throttles, and so on\. On each graph, the totals are displayed at 1\-minute granularity, grouped by AWS Region\.

If you see a spike in errors that you want to investigate, you can choose a function and then view log files by AWS Region, until you determine which function is causing the problems and in which AWS Region\. For more information about troubleshooting Lambda@Edge errors, see:
+  [How to Determine the Type of Failure](lambda-edge-testing-debugging.md#lambda-edge-testing-debugging-failure-type)
+ [Four Steps for Debugging your Content Delivery on AWS](http://aws.amazon.com/blogs/networking-and-content-delivery/four-steps-for-debugging-your-content-delivery-on-aws/)

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh rate\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view the graphs in the CloudWatch console, choose **Add to dashboard**\. You must use the US East \(N\. Virginia\) Region \(us\-east\-1\) to view the graphs in the CloudWatch console\.