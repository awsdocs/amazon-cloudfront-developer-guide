# Getting metrics using the CloudWatch API<a name="programming-cloudwatch-metrics"></a>

You can use the Amazon CloudWatch API or CLI to get the CloudFront metrics in programs or applications that you build\. You can use the raw data to build your own custom dashboards, your own alarming tools, and so on\. To get the CloudFront metrics from the CloudWatch API, you must use the US East \(N\. Virginia\) Region \(us\-east\-1\)\. You also need to know certain values and types for each metric\.

**Topics**
+ [Values for all CloudFront metrics](#cloudfront-metrics-global-values)
+ [Values for individual CloudFront metrics](#cloudfront-metrics-values)

## Values for all CloudFront metrics<a name="cloudfront-metrics-global-values"></a>

The following values apply to all CloudFront metrics:

**`Namespace`**  
The value for `Namespace` is always `AWS/CloudFront`\.

**Dimensions**  
Each CloudFront metric has the following two dimensions:    
**`DistributionId`**  
The value for `DistributionId` is the ID of the CloudFront distribution for which you want to get metrics\.  
**`Region`**  
The value for `Region` is always `Global`, because CloudFront is a global service\.

**Note**  
To get the CloudFront metrics from the CloudWatch API, you must use the US East \(N\. Virginia\) Region \(us\-east\-1\)\.

## Values for individual CloudFront metrics<a name="cloudfront-metrics-values"></a>

Use information from the following list to get details about specific CloudFront metrics from the CloudWatch API\. Some of these metrics are available only when you have enabled additional metrics for the distribution\.

**Note**  
Only one statistic, `Average` or `Sum`, is applicable for each metric\. The following list specifies which statistic is applicable to that metric\.

**4xx error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `4xx`\.  
+ Metric name: `4xxErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**401 error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `401`\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `401ErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**403 error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `403`\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `403ErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**404 error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `404`\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `404ErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**5xx error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `5xx`\.  
+ Metric name: `5xxErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**502 error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `502`\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `502ErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**503 error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `503`\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `503ErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**504 error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `504`\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `504ErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**Bytes downloaded**  
The total number of bytes downloaded by viewers for `GET`, `HEAD`, and `OPTIONS` requests\.  
+ Metric name: `BytesDownloaded`
+ Valid statistic: `Sum`
+ Unit: `None`

**Bytes uploaded**  
The total number of bytes that viewers uploaded to your origin with CloudFront, using `POST` and `PUT` requests\.  
+ Metric name: `BytesUploaded`
+ Valid statistic: `Sum`
+ Unit: `None`

**Cache hit rate**  
The percentage of all cacheable requests for which CloudFront served the content from its cache\. HTTP `POST` and `PUT` requests, and errors, are not considered cacheable requests\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `CacheHitRate`
+ Valid statistic: `Average`
+ Unit: `Percent`

**Origin latency**  
The total time spent, in milliseconds, from when CloudFront receives a request to when it starts providing a response to the network \(not the viewer\), for requests that are served from the origin, not the CloudFront cache\. This is also known as *first byte latency*, or *time\-to\-first\-byte*\. To get this metric, you must first [enable additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional)\.  
+ Metric name: `OriginLatency`
+ Valid statistic: `Percentile`
+ Unit: `Milliseconds`
To get a `Percentile` statistic from the CloudWatch API, use the `ExtendedStatistics` parameter, not `Statistics`\. For more information, see [GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html) in the *Amazon CloudWatch API Reference*, or the reference documentation for the [AWS SDKs](https://docs.aws.amazon.com/#sdks)\.

**Requests**  
The total number of viewer requests received by CloudFront, for all HTTP methods and for both HTTP and HTTPS requests\.  
+ Metric name: `Requests`
+ Valid statistic: `Sum`
+ Unit: `None`

**Total error rate**  
The percentage of all viewer requests for which the response’s HTTP status code is `4xx` or `5xx`\.  
+ Metric name: `TotalErrorRate`
+ Valid statistic: `Average`
+ Unit: `Percent`