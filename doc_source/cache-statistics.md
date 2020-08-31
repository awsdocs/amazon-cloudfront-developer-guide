# CloudFront Cache Statistics Reports<a name="cache-statistics"></a>

You can use the Amazon CloudFront console to display a graphical representation of statistics related to CloudFront edge locations\. Data for these statistics are drawn from the same source as CloudFront access logs\. You can display charts for a specified date range in the last 60 days, with data points every hour or every day\. You can usually view data about requests that CloudFront received as recently as an hour ago, but data can occasionally be delayed by as much as 24 hours\. 

**Note**  
You don't need to enable access logging to view cache statistics\.<a name="cache-statistics-procedure"></a>

**To display CloudFront cache statistics**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation pane, click **Cache Statistics**\.

1. In the **CloudFront Cache Statistics Reports** pane, for **Start Date** and **End Date**, select the date range for which you want to display cache statistics charts\. Available ranges depend on the value that you select for **Granularity**:
   + **Daily** – To display charts with one data point per day, select any date range in the previous 60 days\.
   + **Hourly** – To display charts with one data point every hour, select any date range of up to 14 days within the previous 60 days\.

   Dates and times are in Coordinated Universal Time \(UTC\)\.

1. For **Granularity**, specify whether to display one data point per day or one data point per hour in the charts\. If you specify a date range greater than 14 days, the option to specify one data point per hour is not available\.

1. For **Viewer Location**, choose the continent from which viewer requests originated, or choose **All Locations**\. Cache statistics charts include data for requests that CloudFront received from the specified location\.

1. In the **Distribution** list, select the distributions for which you want to display data in the usage charts:
   + **An individual web distribution** – The charts display data for the selected CloudFront web distribution\. The **Distribution** list displays the distribution ID and alternate domain names \(CNAMEs\) for the distribution, if any\. If a distribution has no alternate domain names, the list includes origin domain names for the distribution\.
   + **All Web Distributions** – The charts display summed data for all web distributions that are associated with the current AWS account, excluding web distributions that you have deleted\.

1. Click **Update**\.

1. To view data for a daily or hourly data point within a chart, move your mouse pointer over the data point\.

1. For charts that show data transferred, note that you can change the vertical scale to gigabytes, megabytes, or kilobytes for each chart\.

**Topics**
+ [Downloading Data in CSV Format](#cache-statistics-csv)
+ [How Cache Statistics Charts Are Related to Data in the CloudFront Access Logs](#cache-statistics-data)

## Downloading Data in CSV Format<a name="cache-statistics-csv"></a>

You can download the Cache Statistics report in CSV format\. This section explains how to download the report and describes the values in the report\.<a name="cache-statistics-csv-procedure"></a>

**To download the Cache Statistics report in CSV format**

1. While viewing the Cache Statistics report, click **CSV**\.

1. In the **Opening *file name*** dialog box, choose whether to open or save the file\.

### Information About the Report<a name="cache-statistics-csv-header"></a>

The first few rows of the report include the following information:

**Version**  
The version of the format for this CSV file\.

**Report**  
The name of the report\.

**DistributionID**  
The ID of the distribution that you ran the report for, or `ALL` if you ran the report for all distributions\.

**StartDateUTC**  
The beginning of the date range for which you ran the report, in Coordinated Universal Time \(UTC\)\.

**EndDateUTC**  
The end of the date range for which you ran the report, in Coordinated Universal Time \(UTC\)\.

**GeneratedTimeUTC**  
The date and time on which you ran the report, in Coordinated Universal Time \(UTC\)\.

**Granularity**  
Whether each row in the report represents one hour or one day\. 

**ViewerLocation**  
The continent that viewer requests originated from, or `ALL`, if you chose to download the report for all locations\.

### Data in the Cache Statistics Report<a name="cache-statistics-csv-data"></a>

The report includes the following values:

**DistributionID**  
The ID of the distribution that you ran the report for, or `ALL` if you ran the report for all distributions\.

**FriendlyName**  
An alternate domain name \(CNAME\) for the distribution, if any\. If a distribution has no alternate domain names, the list includes an origin domain name for the distribution\.

**ViewerLocation**  
The continent that viewer requests originated from, or `ALL`, if you chose to download the report for all locations\.

**TimeBucket**  
The hour or the day that data applies to, in Coordinated Universal Time \(UTC\)\.

**RequestCount**  
The total number of requests for all HTTP status codes \(for example, 200 or 404\) and all methods \(for example, GET, HEAD, or POST\)\.

**HitCount**  
The number of viewer requests for which the object is served from a CloudFront edge cache\.

**MissCount**  
The number of viewer requests for which the object isn't currently in an edge cache, so CloudFront must get the object from your origin\.

**ErrorCount**  
The number of viewer requests that resulted in an error, so CloudFront didn't serve the object\.

**IncompleteDownloadCount**  
The number of viewer requests for which the viewer started but didn't finish downloading the object\.

**HTTP2xx**  
The number of viewer requests for which the HTTP status code was a 2xx value \(succeeded\)\.

**HTTP3xx**  
The number of viewer requests for which the HTTP status code was a 3xx value \(additional action is required\)\.

**HTTP4xx**  
The number of viewer requests for which the HTTP status code was a 4xx value \(client error\)\.

**HTTP5xx**  
The number of viewer requests for which the HTTP status code was a 5xx value \(server error\)\.

**TotalBytes**  
The total number of bytes served to viewers by CloudFront in response to all requests for all HTTP methods\.

**BytesFromMisses**  
The number of bytes served to viewers for objects that were not in the edge cache at the time of the request\. This value is a good approximation of bytes transferred from your origin to CloudFront edge caches\. However, it excludes requests for objects that are already in the edge cache but that have expired\.

## How Cache Statistics Charts Are Related to Data in the CloudFront Access Logs<a name="cache-statistics-data"></a>

The following table shows how cache statistics charts in the CloudFront console correspond with values in CloudFront access logs\. For more information about CloudFront access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**Total Requests**  
This chart shows the total number of requests for all HTTP status codes \(for example, 200 or 404\) and all methods \(for example, `GET`, `HEAD`, or `POST`\)\. Total requests shown in this chart equal the total number of requests in the access log files for the same time period\.

**Percentage of Viewer Requests by Result Type**  
This chart shows hits, misses, and errors as a percentage of total viewer requests for the selected CloudFront distribution:  
+ **Hit** – A viewer request for which the object is served from a CloudFront edge cache\. In access logs, these are requests for which the value of `x-edge-response-result-type` is `Hit`\.
+ **Miss** – A viewer request for which the object isn't currently in an edge cache, so CloudFront must get the object from your origin\. In access logs, these are requests for which the value of `x-edge-response-result-type` is `Miss`\.
+ **Error** – A viewer request that resulted in an error, so CloudFront didn't serve the object\. In access logs, these are requests for which the value of `x-edge-response-result-type` is `Error`, `LimitExceeded`, or `CapacityExceeded`\.
The chart does not include refresh hits—requests for objects that are in the edge cache but that have expired\. In access logs, refresh hits are requests for which the value of `x-edge-response-result-type` is `RefreshHit`\.

**Bytes Transferred to Viewers**  
This chart shows two values:  
+ **Total Bytes** – The total number of bytes served to viewers by CloudFront in response to all requests for all HTTP methods\. In CloudFront access logs, **Total Bytes** is the sum of the values in the `sc-bytes` column for all of the requests during the same time period\.
+ **Bytes from Misses** – The number of bytes served to viewers for objects that were not in the edge cache at the time of the request\. In CloudFront access logs, **Bytes from Misses** is the sum of the values in the `sc-bytes` column for requests for which the value of `x-edge-result-type` is `Miss`\. This value is a good approximation of bytes transferred from your origin to CloudFront edge caches\. However, it excludes requests for objects that are already in the edge cache but that have expired\.

**HTTP Status Codes**  
This chart shows viewer requests by HTTP status code\. In CloudFront access logs, status codes appear in the `sc-status` column:  
+ **2xx** – The request succeeded\.
+ **3xx** – Additional action is required\. For example, 301 \(Moved Permanently\) means that the requested object has moved to a different location\.
+ **4xx** – The client apparently made an error\. For example, 404 \(Not Found\) means that the client requested an object that could not be found\.
+ **5xx** – The origin server didn't fill the request\. For example, 503 \(Service Unavailable\) means that the origin server is currently unavailable\.

**Percentage of GET Requests that Didn't Finish Downloading**  
This chart shows viewer `GET` requests that didn't finish downloading the requested object as a percentage of total requests\. Typically, downloading an object doesn't complete because the viewer canceled the download, for example, by clicking a different link or by closing the browser\. In CloudFront access logs, these requests have a value of `200` in the `sc-status` column and a value of `Error` in the `x-edge-result-type` column\.