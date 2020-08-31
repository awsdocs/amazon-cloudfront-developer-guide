# CloudFront Top Referrers Report<a name="top-referrers-report"></a>

The CloudFront console can display a list of the 25 domains of the websites that originated the most HTTP and HTTPS requests for objects that CloudFront is distributing for a specified distribution\. These top referrers can be search engines, other websites that link directly to your objects, or your own website\. For example, if http://example\.com/index\.html links to 10 graphics, example\.com is the referrer for all 10 graphics\. You can display the Top Referrers report for any date range in the previous 60 days\.

**Note**  
If a user enters a URL directly into the address line of a browser, there is no referrer for the requested object\.

Data for the Top Referrers report is drawn from the same source as CloudFront access logs\. To get an accurate count of the top 25 referrers, CloudFront counts the requests for all of your objects in 10\-minute intervals and keeps a running total of the top 75 referrers\. Near the bottom of the list, referrers constantly rise onto or drop off of the list, so the totals for those referrers are approximations\. The 25 referrers at the top of the list of 75 referrers may rise and fall within the list, but they rarely drop off of the list altogether, so the totals for those referrers typically are more reliable\.

**Note**  
You don't need to enable access logging to view a list of top referrers\.<a name="top-referrers-report-procedure"></a>

**To display top referrers for a distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation pane, click **Top Referrers**\.

1. In the **CloudFront Top Referrers Report** pane, for **Start Date** and **End Date**, select the date range for which you want to display a list of top referrers\. 

   Dates and times are in Coordinated Universal Time \(UTC\)\.

1. In the **Distribution** list, select the distribution for which you want to display a list of top referrers\. 

1. Click **Update**\.

**Topics**
+ [Downloading Data in CSV Format](#top-referrers-csv)
+ [How Data in the Top Referrers Report Is Related to Data in the CloudFront Access Logs](#top-referrers-data)

## Downloading Data in CSV Format<a name="top-referrers-csv"></a>

You can download the Top Referrers report in CSV format\. This section explains how to download the report and describes the values in the report\.<a name="top-referrers-csv-procedure"></a>

**To download the Top Referrers report in CSV format**

1. While viewing the Top Referrers report, click **CSV**\.

1. In the **Opening *file name*** dialog box, choose whether to open or save the file\.

### Information About the Report<a name="top-referrers-csv-header"></a>

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

### Data in the Top Referrers Report<a name="top-referrers-csv-data"></a>

The report includes the following values:

**DistributionID**  
The ID of the distribution that you ran the report for, or `ALL` if you ran the report for all distributions\.

**FriendlyName**  
An alternate domain name \(CNAME\) for the distribution, if any\. If a distribution has no alternate domain names, the list includes an origin domain name for the distribution\.

**Referrer**  
The domain name of the referrer\.

****  
The total number of requests from the domain name in the `Referrer` column\.

**RequestsPct**  
The number of requests submitted by the referrer as a percentage of the total number of requests during the specified period\.

## How Data in the Top Referrers Report Is Related to Data in the CloudFront Access Logs<a name="top-referrers-data"></a>

The following list shows how values in the Top Referrers report in the CloudFront console correspond with values in CloudFront access logs\. For more information about CloudFront access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**Referrer**  
The domain name of the referrer\. In access logs, referrers are listed in the `cs(Referer)` column\. 

**Request Count**  
The total number of requests from the domain name in the **Referrer** column\. This value generally corresponds closely with the number of `GET` requests from the referrer in CloudFront access logs\. 

**Request %**  
The number of requests submitted by the referrer as a percentage of the total number of requests during the specified period\. If you have more than 25 referrers, then you can't calculate **Request %** based on the data in this table because the **Request Count** column doesn't include all of the requests during the specified period\.