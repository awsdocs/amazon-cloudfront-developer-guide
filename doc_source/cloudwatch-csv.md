# Downloading Data in CSV Format<a name="cloudwatch-csv"></a>

You can download the CloudFront CloudWatch monitoring information in CSV format\. This section explains how to download the report and describes several values in the report\.<a name="cloudwatch-csv-procedure"></a>

**To download the CloudFront monitoring information in CSV format**

1. While viewing the CloudFront metrics, choose **CSV**\.

1. In the **Opening *file name*** dialog box, choose whether to open or save the file\.

## Information About the Report<a name="cloudwatch-csv-header"></a>

The first few rows of the report include the following information:

**Version**  
The CloudFront reporting version\.

**Report**  
The name of the report\.

**DistributionID**  
The ID of the distribution that you ran the report for\.

**StartDateUTC**  
The beginning of the date range for which you ran the report, in Coordinated Universal Time \(UTC\)\.

**EndDateUTC**  
The end of the date range for which you ran the report, in Coordinated Universal Time \(UTC\)\.

**GeneratedTimeUTC**  
The date and time on which you ran the report, in Coordinated Universal Time \(UTC\)\.

**Granularity**  
The time period for each row in the report, for example, ONE\_MINUTE\. 

## Data in the CloudWatch Metrics Report<a name="cloudwatch-csv-data"></a>

The report includes the following values:

**DistributionID**  
The ID of the distribution that you ran the report for\.

**FriendlyName**  
An alternate domain name \(CNAME\) for the distribution, if any\. If a distribution has no alternate domain names, the list includes an origin domain name for the distribution\.

**TimeBucket**  
The hour or the day that data applies to, in Coordinated Universal Time \(UTC\)\.

**Requests**  
The total number of requests for all HTTP status codes \(for example, 200 or 404\) and all methods \(for example, GET, HEAD, or POST\) during the time period\.

**BytesDownloaded**  
The number of bytes that viewers downloaded for the specified distribution during the time period\.

**BytesUploaded**  
The number of bytes that viewers uploaded to your origin for the specified distribution during the time period\.

**TotalErrorRatePct**  
Requests for which the HTTP status code was a 4xx or 5xx error for the specified distribution during the time period\.

**4xxErrorRatePct**  
Requests for which the HTTP status code was a 4xx error for the specified distribution during the time period\.

**5xxErrorRatePct**  
Requests for which the HTTP status code was a 5xx error for the specified distribution during the time period\.