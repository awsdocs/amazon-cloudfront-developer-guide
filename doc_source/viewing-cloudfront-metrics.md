# Viewing CloudFront and edge function metrics<a name="viewing-cloudfront-metrics"></a>

You can view operational metrics about your CloudFront distributions and [edge functions](https://aws.amazon.com/cloudfront/features/#Edge_Computing) in the CloudFront console\. To view these metrics, see the [Monitoring page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring)\. To view graphs about the activity for a specific CloudFront distribution or edge function, choose one, and then choose **View distribution metrics** or **View metrics**\.

**Topics**
+ [Viewing the default CloudFront distribution metrics](#monitoring-console.distributions)
+ [Turning on additional CloudFront distribution metrics](#monitoring-console.distributions-additional)
+ [Viewing the default Lambda@Edge function metrics](#monitoring-console.lambda-at-edge)
+ [Viewing the default CloudFront Functions metrics](#monitoring-console.cloudfront-functions)

## Viewing the default CloudFront distribution metrics<a name="monitoring-console.distributions"></a>

The following default metrics are included for all CloudFront distributions, at no additional cost:

**Requests**  
The total number of viewer requests received by CloudFront, for all HTTP methods and for both HTTP and HTTPS requests\.

**Bytes downloaded**  
The total number of bytes downloaded by viewers for `GET`, `HEAD`, and `OPTIONS` requests\.

**Bytes uploaded**  
The total number of bytes that viewers uploaded to your origin with CloudFront, using `POST` and `PUT` requests\.

**4xx error rate**  
The percentage of all viewer requests for which the response's HTTP status code is `4xx`\.

**5xx error rate**  
The percentage of all viewer requests for which the response's HTTP status code is `5xx`\.

**Total error rate**  
The percentage of all viewer requests for which the response's HTTP status code is `4xx` or `5xx`\.

These metrics are shown in graphs for each CloudFront distribution on the [Monitoring page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring)\. On each graph, the totals are displayed at 1\-minute granularity\. In addition to viewing the graphs, you can also [download metrics reports as CSV files](cloudwatch-csv.md)\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh rate\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

## Turning on additional CloudFront distribution metrics<a name="monitoring-console.distributions-additional"></a>

In addition to the default metrics, you can turn on additional metrics for an additional cost\. For more information about the cost, see [Estimating cost for the additional CloudFront metrics](#monitoring-console.distributions-additional-pricing)\.

These additional metrics must be turned on for each distribution separately:

**Cache hit rate**  
The percentage of all cacheable requests for which CloudFront served the content from its cache\. HTTP `POST` and `PUT` requests, and errors, are not considered cacheable requests\.

**Origin latency**  
The total time spent from when CloudFront receives a request to when it starts providing a response to the network \(not the viewer\), for requests that are served from the origin, not the CloudFront cache\. This is also known as *first byte latency*, or *time\-to\-first\-byte*\.

**Error rate by status code**  
The percentage of all viewer requests for which the response's HTTP status code is a particular code in the `4xx` or `5xx` range\. This metric is available for all of the following error codes: `401`, `403`, `404`, `502`, `503`, and `504`\.

### Turning on additional metrics<a name="enable-metrics"></a>

You can turn on additional metrics in the CloudFront console, with AWS CloudFormation, with the AWS Command Line Interface \(AWS CLI\), or with the CloudFront API\.

------
#### [ Console ]

**To turn on additional metrics \(console\)**

1. Sign in to the AWS Management Console and open the [Monitoring page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring)\.

1. Choose the distribution to turn on additional metrics for, and then choose **View distribution metrics**\.

1. Choose **Manage additional metrics**\.

1. In the **Manage additional metrics** window, turn on **Enabled**\. After you turn on the additional metrics, you can close the **Manage additional metrics** window\.

After you turn on the additional metrics, they are shown in graphs\. On each graph, the totals are displayed at 1\-minute granularity\. In addition to viewing the graphs, you can also [download metrics reports as CSV files](cloudwatch-csv.md)\.

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\.
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh rate\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view CloudFront graphs in the CloudWatch console, choose **Add to dashboard**\.

------
#### [ AWS CloudFormation ]

To turn on additional metrics with AWS CloudFormation, use the `AWS::CloudFront::MonitoringSubscription` resource type\. The following example shows the AWS CloudFormation template syntax, in YAML format, for enabling additional metrics\.

```
Type: AWS::CloudFront::MonitoringSubscription
Properties: 
  DistributionId: EDFDVBD6EXAMPLE
  MonitoringSubscription:
    RealtimeMetricsSubscriptionConfig:
      RealtimeMetricsSubscriptionStatus: Enabled
```

------
#### [ CLI ]

To manage additional metrics with the AWS Command Line Interface \(AWS CLI\), use one of the following commands:

**To turn on additional metrics for a distribution \(CLI\)**
+ Use the create\-monitoring\-subscription command, as in the following example\. Replace *EDFDVBD6EXAMPLE* with the ID of the distribution that you are enabling additional metrics for\.

  ```
  aws cloudfront create-monitoring-subscription --distribution-id EDFDVBD6EXAMPLE --monitoring-subscription RealtimeMetricsSubscriptionConfig={RealtimeMetricsSubscriptionStatus=Enabled}
  ```

**To see whether additional metrics are turned on for a distribution \(CLI\)**
+ Use the get\-monitoring\-subscription command, as in the following example\. Replace *EDFDVBD6EXAMPLE* with the ID of the distribution that you are checking\.

  ```
  aws cloudfront get-monitoring-subscription --distribution-id EDFDVBD6EXAMPLE
  ```

**To turn off additional metrics for a distribution \(CLI\)**
+ Use the delete\-monitoring\-subscription command, as in the following example\. Replace *EDFDVBD6EXAMPLE* with the ID of the distribution that you are turning off additional metrics for\.

  ```
  aws cloudfront delete-monitoring-subscription --distribution-id EDFDVBD6EXAMPLE
  ```

------
#### [ API ]

To manage additional metrics with the CloudFront API, use one of the following API operations\.
+ To turn on additional metrics for a distribution, use [CreateMonitoringSubscription](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateMonitoringSubscription.html)\.
+ To see whether additional metrics are turned on for a distribution, use [GetMonitoringSubscription](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetMonitoringSubscription.html)\.
+ To turn off additional metrics for a distribution, use [DeleteMonitoringSubscription](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteMonitoringSubscription.html)\.

For more information about these API calls, see the API reference documentation for your AWS SDK or other API client\.

------

### Estimating cost for the additional CloudFront metrics<a name="monitoring-console.distributions-additional-pricing"></a>

When you turn on additional metrics for a distribution, CloudFront sends up to 8 metrics to CloudWatch in the US East \(N\. Virginia\) Region\. CloudWatch charges a low, fixed rate for each metric\. This rate is charged only once per month, per metric \(up to 8 metrics per distribution\)\. This is a fixed rate, so your cost remains the same regardless of the number of requests or responses that the CloudFront distribution receives or sends\. For the per\-metric rate, see the [Amazon CloudWatch pricing page](https://aws.amazon.com/cloudwatch/pricing/) and the [CloudWatch pricing calculator](https://aws.amazon.com/cloudwatch/pricing/#Pricing_calculator)\. Additional API charges apply when you retrieve the metrics with the CloudWatch API\.

## Viewing the default Lambda@Edge function metrics<a name="monitoring-console.lambda-at-edge"></a>

You can use CloudWatch metrics to monitor, in real time, problems with your Lambda@Edge functions\. There's no additional charge for these metrics\.

When you attach a Lambda@Edge function to a cache behavior in a CloudFront distribution, Lambda begins sending metrics to CloudWatch automatically\. Metrics are available for all Lambda Regions, but to view metrics in the CloudWatch console or get the metric data from the CloudWatch API, you must use the US East \(N\. Virginia\) Region \(`us-east-1`\)\. The metric group name is formatted as: `AWS/CloudFront/distribution-ID`, where *distribution\-ID* is the ID of the CloudFront distribution that the Lambda@Edge function is associated with\. For more information about CloudWatch metrics, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

The following default metrics are shown in graphs for each Lambda@Edge function on the [Monitoring page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring):
+ `5xx` error rate for Lambda@Edge
+ Lambda execution errors
+ Lambda invalid responses
+ Lambda throttles

The graphs include the number of invocations, errors, throttles, and so on\. On each graph, the totals are displayed at 1\-minute granularity, grouped by AWS Region\.

If you see a spike in errors that you want to investigate, you can choose a function and then view log files by AWS Region, until you determine which function is causing the problems and in which AWS Region\. For more information about troubleshooting Lambda@Edge errors, see:
+ [How to determine the type of failure](lambda-edge-testing-debugging.md#lambda-edge-testing-debugging-failure-type)
+ [Four Steps for Debugging your Content Delivery on AWS](https://aws.amazon.com/blogs/networking-and-content-delivery/four-steps-for-debugging-your-content-delivery-on-aws/)

You can customize the graphs by doing the following:
+ To change the time range for the information displayed in the graphs, choose 1h \(1 hour\), 3h \(3 hours\), or another range, or specify a custom range\. 
+ To change how often CloudFront updates the information in the graph, choose the down arrow next to the refresh icon, and then choose a refresh rate\. The default refresh rate is 1 minute, but you can choose 10 seconds, 2 minutes, or other options\.

To view the graphs in the CloudWatch console, choose **Add to dashboard**\. You must use the US East \(N\. Virginia\) Region \(us\-east\-1\) to view the graphs in the CloudWatch console\.

## Viewing the default CloudFront Functions metrics<a name="monitoring-console.cloudfront-functions"></a>

CloudFront Functions sends operational metrics to Amazon CloudWatch so that you can monitor your functions\. Viewing these metrics can help you troubleshoot, track, and debug issues\. CloudFront Functions publishes the following metrics to CloudWatch:
+ **Invocations** \(`FunctionInvocations`\) – The number of times the function was started \(invoked\) in a given time period\.
+ **Validation errors** \(`FunctionValidationErrors`\) – The number of validation errors produced by the function in a given time period\. Validation errors occur when the function runs successfully but returns invalid data \(an invalid [event object](functions-event-structure.md)\)\.
+ **Execution errors** \(`FunctionExecutionErrors`\) – The number of execution errors that occurred in a given time period\. Execution errors occur when the function fails to complete successfully\.
+ **Compute utilization** \(`FunctionComputeUtilization`\) – The amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a value of 35 means that the function completed in 35% of the maximum allowed time\. This metric is a number between 0 and 100\.
+ **Throttles** \(`FunctionThrottles`\) – The number of times that the function was throttled in a given time period\. Functions can be throttled for the following reasons:
  + The function continuously exceeds the maximum time allowed for execution
  + The function results in compilation errors
  + There is an unusually high number of requests per second

To view these metrics in the CloudFront console, go to the [Monitoring page](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring)\. To view graphs for a specific function, choose **Functions**, select the function, and then choose **View function metrics**\.

All of these metrics are published to CloudWatch in the US East \(N\. Virginia\) Region \(`us-east-1`\), in the CloudFront namespace\. You can also view these metrics in the CloudWatch console\. In the CloudWatch console, you can view the metrics per function or per function per distribution\.

You can also use CloudWatch to set alarms based on these metrics\. For example, you can set an alarm based on the execution time metric, which represents the percentage of available time that your function took to run\. When the execution time reaches a certain value for a certain amount of time—for example, greater than 70% of available time for 15 continuous minutes—the alarm is triggered\. You specify the alarms value and its time unit when you create the alarm\.

**Note**  
CloudFront Functions sends metrics to CloudWatch only for functions in the `LIVE` stage that run in response to production requests and responses\. When you [test a function](test-function.md), CloudFront doesn't send any metrics to CloudWatch\. The test output contains information about errors, compute utilization, and function logs \(`console.log()` statements\), but this information is not sent to CloudWatch\.

For information about how to get these metrics with the CloudWatch API, see [Getting metrics using the CloudWatch API](programming-cloudwatch-metrics.md)\.