# CloudFront Reports in the Console<a name="reports"></a>

The CloudFront console includes a variety of reports about your CloudFront activity, including the following: 
+ [CloudFront Cache Statistics Reports](#reports-overview-cache-statistics)
+ [CloudFront Popular Objects Report](#reports-overview-popular-objects)
+ [CloudFront Top Referrers Report](#reports-overview-top-referrers)
+ [CloudFront Usage Reports](#reports-overview-usage)
+ [CloudFront Viewers Reports](#reports-overview-viewers)

Most of these reports are based on the data in CloudFront access logs, which contain detailed information about every user request that CloudFront receives\. You don't need to enable access logs to view the reports\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\. The CloudFront usage report is based on the AWS usage report for CloudFront, which also doesn't require any special configuration\. For more information, see [AWS Usage Report for CloudFront](reports-billing.md#usage-report)\.

**CloudFront Cache Statistics Reports**

The CloudFront cache statistics report includes the following information:
+ **Total Requests** – Shows the total number of requests for all HTTP status codes \(for example, 200 or 404\) and all methods \(for example, GET, HEAD, or POST\)\.
+ **Percentage of Viewer Requests by Result Type** – Shows hits, misses, and errors as a percentage of total viewer requests for the selected CloudFront distribution\.
+ **Bytes Transferred to Viewers** – Shows total bytes and bytes from misses\.
+ **HTTP Status Codes** – Shows viewer requests by HTTP status code\.
+ **Percentage of GET Requests that Didn't Finish Downloading**– Shows viewer GET requests that didn't finish downloading the requested object as a percentage of total requests\.

For more information, see [CloudFront Cache Statistics Reports](cache-statistics.md)\.

**CloudFront Popular Objects Report**

The CloudFront popular objects report lists the 50 most popular objects and statistics about those objects, including the number of requests for the object, the number of hits and misses, the hit ratio, the number of bytes served for misses, the total bytes served, the number of incomplete downloads, and the number of requests by HTTP status code \(2xx, 3xx, 4xx, and 5xx\)\.

For more information, see [CloudFront Popular Objects Report](popular-objects-report.md)\.

**CloudFront Top Referrers Report**

The CloudFront top referrers report includes the top 25 referrers, the number of requests from a referrer, and the number of requests from a referrer as a percentage of the total number of requests during the specified period\.

For more information, see [CloudFront Top Referrers Report](top-referrers-report.md)\.

**CloudFront Usage Reports**

The CloudFront usage reports include the following information:
+ **Number of Requests** – Shows the total number of requests that CloudFront responds to from edge locations in the selected region during each time interval for the specified CloudFront distribution\.
+ **Data Transferred by Protocol** and **Data Transferred by Destination** – Both show the total amount of data transferred from CloudFront edge locations in the selected region during each time interval for the specified CloudFront distribution\. They separate the data differently, as follows:
  + **By Protocol** – Separates the data by protocol: HTTP or HTTPS\.
  + **By Destination** – Separates the data by destination: to your users or to your origin\.

For more information, see [CloudFront Usage Reports](usage-charts.md)\.

**CloudFront Viewers Reports**

The CloudFront viewers reports include the following information:
+ **Devices** – Shows the types of devices \(for example, Desktop or Mobile\) that your users use to access your content
+ **Browsers** – Shows the name \(or the name and version\) of the browsers that your users use most frequently to access your content, for example, Chrome or Firefox
+ **Operating Systems** – Shows the name \(or the name and version\) of the operating system that viewers run on most frequently when accessing your content, for example, Linux, Mac OS X, or Windows
+ **Locations** – Shows the locations, by country or by U\.S\. state/territory, of the viewers that access your content most frequently

For more information, see [CloudFront Viewers Reports](viewers-reports.md)\.