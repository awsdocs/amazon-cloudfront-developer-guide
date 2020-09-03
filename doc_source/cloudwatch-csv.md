# Downloading data in CSV format<a name="cloudwatch-csv"></a>

You can download the CloudWatch metrics data for a CloudFront distribution in CSV format\. You can download the data when you **View distribution metrics** for a particular distribution in the [CloudFront console](https://console.aws.amazon.com/cloudfront/home)\.

## Information about the report<a name="cloudwatch-csv-header"></a>

The first few rows of the report include the following information:

**Version**  
The CloudFront reporting version\.

**Report**  
The name of the report\.

**DistributionID**  
The ID of the distribution for which you ran the report\.

**StartDateUTC**  
The beginning of the date range for which you ran the report, in Coordinated Universal Time \(UTC\)\.

**EndDateUTC**  
The end of the date range for which you ran the report, in Coordinated Universal Time \(UTC\)\.

**GeneratedTimeUTC**  
The date and time on which you ran the report, in Coordinated Universal Time \(UTC\)\.

**Granularity**  
The time period for each row in the report, for example, `ONE_MINUTE`\.

## Data in the metrics report<a name="cloudwatch-csv-data"></a>

The report includes the following values:

**DistributionID**  
The ID of the distribution for which you ran the report\.

**FriendlyName**  
An alternate domain name \(CNAME\) for the distribution, if any\. If a distribution has no alternate domain names, the list includes an origin domain name for the distribution\.

**TimeBucket**  
The hour or the day that the data applies to, in Coordinated Universal Time \(UTC\)\.

**Requests**  
The total number of requests for all HTTP status codes \(for example, `200`, `404`, and so on\) and all methods \(for example, `GET`, `HEAD`, `POST`, and so on\) during the time period\.

**BytesDownloaded**  
The number of bytes that viewers downloaded for the specified distribution during the time period\.

**BytesUploaded**  
The number of bytes that viewers uploaded for the specified distribution during the time period\.

**TotalErrorRatePct**  
The percentage of requests for which the HTTP status code was a `4xx` or `5xx` error for the specified distribution during the time period\.

**4xxErrorRatePct**  
The percentage of requests for which the HTTP status code was a `4xx` error for the specified distribution during the time period\.

**5xxErrorRatePct**  
The percentage of requests for which the HTTP status code was a `5xx` error for the specified distribution during the time period\.

If you have [enabled additional metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions-additional) for your distribution, then the report also includes the following additional values:

**401ErrorRatePct**  
The percentage of requests for which the HTTP status code was a `401` error for the specified distribution during the time period\.

**403ErrorRatePct**  
The percentage of requests for which the HTTP status code was a `403` error for the specified distribution during the time period\.

**404ErrorRatePct**  
The percentage of requests for which the HTTP status code was a `404` error for the specified distribution during the time period\.

**502ErrorRatePct**  
The percentage of requests for which the HTTP status code was a `502` error for the specified distribution during the time period\.

**503ErrorRatePct**  
The percentage of requests for which the HTTP status code was a `503` error for the specified distribution during the time period\.

**504ErrorRatePct**  
The percentage of requests for which the HTTP status code was a `504` error for the specified distribution during the time period\.

**OriginLatency**  
The total time spent, in milliseconds, from when CloudFront received a request to when it started providing a response to the network \(not the viewer\), for requests that were served from the origin, not the CloudFront cache\. This is also known as *first byte latency*, or *time\-to\-first\-byte*\.

**CacheHitRate**  
The percentage of all cacheable requests for which CloudFront served the content from its cache\. HTTP `POST` and `PUT` requests, and errors, are not considered cacheable requests\.