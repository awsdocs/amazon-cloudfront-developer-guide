# AWS billing and usage reports for CloudFront<a name="reports-billing"></a>

AWS provides two usage reports for CloudFront:
+ The billing report is a high\-level view of all of the activity for the AWS services that you’re using, including CloudFront\. For more information, see [AWS billing report for CloudFront](#billing-report)\.
+ The usage report is a summary of activity for a specific service, aggregated by hour, day, or month\. It also includes usage charts the provide a graphical representation of your CloudFront usage\. For more information, see [AWS usage report for CloudFront](#usage-report)\.

To help you understand these reports, see the detailed information in [Interpreting your AWS bill and the AWS usage report for CloudFront](billing-and-usage-interpreting.md)\.

**Note**  
Like other AWS services, CloudFront charges you for only what you use\. For more information, see [CloudFront pricing](CloudFrontPricing.md)\.

**Topics**
+ [AWS billing report for CloudFront](#billing-report)
+ [AWS usage report for CloudFront](#usage-report)
+ [Interpreting your AWS bill and the AWS usage report for CloudFront](billing-and-usage-interpreting.md)

## AWS billing report for CloudFront<a name="billing-report"></a>

You can view a summary of your AWS usage and charges, listed by service, on the bills page in the AWS Management Console\.

You can also download a more detailed version of the report in CSV format\. The detailed billing report includes the following values that apply to CloudFront:
+ **ProductCode** — `AmazonCloudFront`
+ **UsageType** — One of the following values:
  + A code that identifies the type of data transfer
  + `Invalidations`
  + `SSL-Cert-Custom`

  For more information, see [Interpreting your AWS bill and the AWS usage report for CloudFront](billing-and-usage-interpreting.md)\.
+ **ItemDescription** — A description of the billing rate for the **UsageType**\.
+ **Usage Start Date/Usage End Date** — The day that the usage applies to, in Coordinated Universal Time \(UTC\)\. 
+ **Usage Quantity** — One of the following values: 
  + The number of requests during the specified time period
  + The amount of data transferred in gigabytes
  + The number of objects invalidated
  + The sum of the prorated months that you had SSL certificates associated with enabled CloudFront distributions\. For example, if you have one certificate associated with an enabled distribution for an entire month and another certificate associated with an enabled distribution for half of the month, this value will be 1\.5\. <a name="billing-report-procedure"></a>

**To display summary billing information and download the detailed billing report**

1. Sign in to the AWS Management Console at [https://console.aws.amazon.com/console/home](https://console.aws.amazon.com/console/home)\.

1. In the title bar, choose your user name, then choose **Billing Dashboard**\.

1. In the navigation pane, choose **Bills**\.

1. To view summary information for CloudFront, under **Details**, choose **CloudFront**\.

1. To download a detailed billing report in CSV format, choose **Download CSV**, then follow the on\-screen prompts to save the report\.

## AWS usage report for CloudFront<a name="usage-report"></a>

AWS provides a CloudFront usage report that is more detailed than the billing report but less detailed than CloudFront access logs\. The usage report provides aggregate usage data by hour, day, or month, and it lists operations by region and usage type, such as data transferred out of the Australia region\.

The CloudFront usage report includes the following values:
+ **Service** — `AmazonCloudFront`
+ **Operation** — HTTP method\. Values include `DELETE`, `GET`, `HEAD`, `OPTIONS`, `PATCH`, `POST`, and `PUT`\.
+ **UsageType** — One of the following values:
  + A code that identifies the type of data transfer
  + `Invalidations`
  + `SSL-Cert-Custom`

  For more information, see [Interpreting your AWS bill and the AWS usage report for CloudFront](billing-and-usage-interpreting.md)\.
+ **Resource** — Either the ID of the CloudFront distribution associated with the usage or the certificate ID of an SSL certificate that you have associated with a CloudFront distribution\.
+ **StartTime/EndTime** — The day that the usage applies to, in Coordinated Universal Time \(UTC\)\. 
+ **UsageValue** — \(1\) The number of requests during the specified time period or \(2\) the amount of data transferred in bytes\.

If you’re using Amazon S3 as the origin for CloudFront, consider running the usage report for Amazon S3, too\. However, if you use Amazon S3 for purposes other than as an origin for your CloudFront distributions, it might not be clear what portion applies to your CloudFront usage\.

**Tip**  
For detailed information about every request that CloudFront receives for your objects, turn on CloudFront access logs for your distribution\. For more information, see [Configuring and using standard logs \(access logs\)](AccessLogs.md)\.