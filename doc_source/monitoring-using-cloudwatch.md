# Monitoring CloudFront Activity Using CloudWatch<a name="monitoring-using-cloudwatch"></a>

Amazon CloudFront integrates with Amazon CloudWatch metrics so that you can monitor your website or application\. CloudFront currently provides six free metrics, and these metrics don't count against [CloudWatch limits](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_limits.html)\. When viewing metrics, you can specify a time interval of as little as one minute for time periods in the previous two weeks\.

## Viewing Metrics for a Distribution<a name="viewing-metrics"></a>

**To view metrics for a distribution in the CloudWatch console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the navigation pane, click **Monitoring and Alarms**\.

1. In the **CloudFront Metrics and Alarms From CloudWatch** pane, specify the following values:   
**From and To**  
Select the date and time range for which you want to display CloudWatch metrics\.  
**Granularity**  
Specify the interval of the data points, for example, one per minute or one per hour\. Note that the time period that you choose affects the available granularity\. For example, if you choose to view data for two weeks, the finest granularity is one hour, and if you choose to view data for 24 hours, the finest granularity is one minute\.  
**Web Distribution**  
Select the distribution that you want to display metrics for\.

1. Click **Update Graph** to refresh the graph based on the settings that you specified\.

## Receiving Notifications<a name="receiving-notifications"></a>

**To receive an Amazon Simple Notification Service \(Amazon SNS\) notification based on a CloudFront metric**

1. On the **CloudFront Metrics and Alarms From CloudWatch** page, expand the list of existing alarms to confirm that the alarm that you want to create doesn't already exist\.

1. Click **Create Alarm**\.

1. In the **Create Alarm** dialog box, specify the following values:   
**Metric**  
Choose the metric for which you want to create the alarm\.  
**Distribution**  
Choose the CloudFront distribution for which you want to create the alarm\.  
****Name of alarm****  
Enter a name for the alarm\.  
**Send notification to**  
Choose the existing Amazon SNS topic that you want to send notification to if the status of this metric triggers an alarm\.   
****Whenever** metric *operator value***  
Specify when CloudWatch should trigger an alarm and send a notification to the specified email list\. For example, to receive notification when the 5xx error rate exceeds 1%, you'd specify the following:  
**Whenever Average of 5xxErrorRate** `> 1`  
Note the following about specifying values for *value*:  

   + Enter only whole numbers without punctuation\. For example, to specify one thousand, enter **1000**\.

   + For 4xx, 5xx, and total error rates, the value that you specify is a percentage\.

   + For requests, bytes downloaded, and bytes uploaded, the value you specify is in units, for example, 1000000000 bytes\.  
****For at least** *x* **consecutive periods of** *time period***  
Specify how many consecutive time periods of the specified duration the metric must meet the criteria before CloudWatch sends notification\. When you choose a value, you need to find an appropriate balance between a value that produces frequent notifications for fleeting problems and delayed notifications for real problems\.

1. If you created a new Amazon SNS topic, when you click **Create**, Amazon SNS sends you an email with information about the new topic\. Follow the instructions in the email\. 

## Downloading Data in CSV Format<a name="cloudwatch-csv"></a>

You can download the CloudWatch Metrics report in CSV format\. This section explains how to download the report and describes the values in the report\.

**To download the CloudWatch Metrics report in CSV format**

1. While viewing the CloudWatch metrics, click **CSV**\.

1. In the **Opening *file name*** dialog box, choose whether to open or save the file\.

### Information About the Report<a name="cloudwatch-csv-header"></a>

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

### Data in the CloudWatch Metrics Report<a name="cloudwatch-csv-data"></a>

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

## Amazon CloudFront Metrics<a name="cloudfront-metrics"></a>

The `AWS/CloudFront` namespace includes the following metrics\.

**Note**  
Only one statistic, Average or Sum, is applicable for each metric\. However, all statistics are available through the console, API, and AWS Command Line Interface\. In the following table, each metric specifies the statistic that is applicable to that metric\.


| Metric | Description | 
| --- | --- | 
|  `Requests` |  The number of requests for all HTTP methods and for both HTTP and HTTPS requests\. Valid Statistics: Sum Units: None  | 
|  `BytesDownloaded` |  The number of bytes downloaded by viewers for `GET`, `HEAD`, and `OPTIONS` requests\. Valid Statistics: Sum Units: None  | 
| `BytesUploaded` | The number of bytes uploaded to your origin with CloudFront using `POST` and `PUT` requests\. Valid Statistics: Sum Units: None  | 
| `TotalErrorRate` | The percentage of all requests for which the HTTP status code is `4xx` or `5xx`\. Valid Statistics: Average Units: Percent  | 
| `4xxErrorRate` | The percentage of all requests for which the HTTP status code is `4xx`\. Valid Statistics: Average Units: Percent  | 
| `5xxErrorRate` | The percentage of all requests for which the HTTP status code is `5xx`\. Valid Statistics: Average Units: Percent  | 

## Dimensions for CloudFront Metrics<a name="cloudfront-metricdimensions"></a>

CloudFront metrics use the CloudFront namespace and provide metrics for two dimensions:


| Dimension | Description | 
| --- | --- | 
| `DistributionId` | The CloudFront ID of the distribution for which you want to display metrics\. | 
| `Region` | The region for which you want to display metrics\. This value must be `Global`\. The `Region` dimension is different from the region in which CloudFront metrics are stored, which is US East \(N\. Virginia\)\.  | 