# CloudFront reports in the console<a name="reports"></a>

The CloudFront console includes a variety of reports about your CloudFront activity, including the following: 
+ [CloudFront cache statistics reports](#reports-overview-cache-statistics)
+ [CloudFront popular objects report](#reports-overview-popular-objects)
+ [CloudFront top referrers report](#reports-overview-top-referrers)
+ [CloudFront usage reports](#reports-overview-usage)
+ [CloudFront viewers reports](#reports-overview-viewers)

Most of these reports are based on the data in CloudFront access logs, which contain detailed information about every user request that CloudFront receives\. You don't need to enable access logs to view the reports\. For more information, see [Configuring and using standard logs \(access logs\)](AccessLogs.md)\. The CloudFront usage report is based on the AWS usage report for CloudFront, which also doesn't require any special configuration\. For more information, see [AWS usage report for CloudFront](reports-billing.md#usage-report)\.

**CloudFront cache statistics reports**

The CloudFront cache statistics report includes the following information:
+ **Total requests** – Shows the total number of requests for all HTTP status codes \(for example, 200 or 404\) and all methods \(for example, GET, HEAD, or POST\)\.
+ **Percentage of viewer requests by result type** – Shows hits, misses, and errors as a percentage of total viewer requests for the selected CloudFront distribution\.
+ **Bytes transferred to viewers** – Shows total bytes and bytes from misses\.
+ **HTTP status codes** – Shows viewer requests by HTTP status code\.
+ **Percentage of GET requests that didn't finish downloading**– Shows viewer GET requests that didn't finish downloading the requested object as a percentage of total requests\.

For more information, see [CloudFront cache statistics reports](cache-statistics.md)\.

**CloudFront popular objects report**

The CloudFront popular objects report lists the 50 most popular objects and statistics about those objects, including the number of requests for the object, the number of hits and misses, the hit ratio, the number of bytes served for misses, the total bytes served, the number of incomplete downloads, and the number of requests by HTTP status code \(2xx, 3xx, 4xx, and 5xx\)\.

For more information, see [CloudFront popular objects report](popular-objects-report.md)\.

**CloudFront top referrers report**

The CloudFront top referrers report includes the top 25 referrers, the number of requests from a referrer, and the number of requests from a referrer as a percentage of the total number of requests during the specified period\.

For more information, see [CloudFront top referrers report](top-referrers-report.md)\.

**CloudFront usage reports**

The CloudFront usage reports include the following information:
+ **Number of requests** – Shows the total number of requests that CloudFront responds to from edge locations in the selected region during each time interval for the specified CloudFront distribution\.
+ **Data transferred by protocol** and **data transferred by destination** – Both show the total amount of data transferred from CloudFront edge locations in the selected region during each time interval for the specified CloudFront distribution\. They separate the data differently, as follows:
  + **By protocol** – Separates the data by protocol: HTTP or HTTPS\.
  + **By destination** – Separates the data by destination: to your users or to your origin\.

For more information, see [CloudFront usage reports](usage-charts.md)\.

**CloudFront viewers reports**

The CloudFront viewers reports include the following information:
+ **Devices** – Shows the types of devices \(for example, Desktop or Mobile\) that your users use to access your content
+ **Browsers** – Shows the name \(or the name and version\) of the browsers that your users use most frequently to access your content, for example, Chrome or Firefox
+ **Operating systems** – Shows the name \(or the name and version\) of the operating system that viewers run on most frequently when accessing your content, for example, Linux, macOS, or Windows
+ **Locations** – Shows the locations, by country or by U\.S\. state/territory, of the viewers that access your content most frequently

For more information, see [CloudFront viewers reports](viewers-reports.md)\.