# AWS Billing and Usage Reports for CloudFront<a name="reports-billing"></a>

AWS provides two usage reports for CloudFront:
+ The billing report is a high\-level view of all of the activity for the AWS services that you're using, including CloudFront\. For more information, see [AWS Billing Report for CloudFront](#billing-report)\.
+ The usage report is a summary of activity for a specific service, aggregated by hour, day, or month\. It also includes usage charts the provide a graphical representation of your CloudFront usage\. For more information, see [AWS Usage Report for CloudFront](#usage-report)\.

To help you understand these reports, see the detailed information in [Interpreting Your AWS Bill and the AWS Usage Report for CloudFront](billing-and-usage-interpreting.md)\.

**Note**  
Like other AWS services, CloudFront charges you for only what you use\. For more information, see [CloudFront pricing](CloudFrontPricing.md)\.

**Topics**
+ [AWS Billing Report for CloudFront](#billing-report)
+ [AWS Usage Report for CloudFront](#usage-report)
+ [Interpreting Your AWS Bill and the AWS Usage Report for CloudFront](billing-and-usage-interpreting.md)

## AWS Billing Report for CloudFront<a name="billing-report"></a>

You can view a summary of your AWS usage and charges, listed by service, on the Bills page in the AWS Management Console\.

You can also download a more detailed version of the report in CSV format\. The detailed billing report includes the following values that apply to CloudFront:
+ **ProductCode** — `AmazonCloudFront`
+ **UsageType** — One of the following values
  + A code that identifies the type of data transfer
  + `Invalidations`
  + `SSL-Cert-Custom`

  For more information, see [Interpreting Your AWS Bill and the AWS Usage Report for CloudFront](billing-and-usage-interpreting.md)\.
+ **ItemDescription** — A description of the billing rate for the **UsageType**\.
+ **Usage Start Date/Usage End Date** — The day that the usage applies to, in Coordinated Universal Time \(UTC\)\. 
+ **Usage Quantity** — One of the following values: 
  + The number of requests during the specified time period
  + The amount of data transferred in gigabytes
  + The number of objects invalidated
  + The sum of the prorated months that you had SSL certificates associated with enabled CloudFront distributions\. For example, if you have one certificate associated with an enabled distribution for an entire month and another certificate associated with an enabled distribution for half of the month, this value will be 1\.5\. <a name="billing-report-procedure"></a>

**To display summary billing information and download the detailed billing report**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/console/home](https://console.aws.amazon.com/console/home)\.

1. In the title bar, click your IAM user name, and click **Billing & Cost Management**\.

1. In the navigation pane, click **Bills**\.

1. To view summary information for CloudFront, under **Details**, click **CloudFront**\.

1. To download a detailed billing report in CSV format, click **Download CSV**, and follow the on\-screen prompts to save the report\.

## AWS Usage Report for CloudFront<a name="usage-report"></a>

AWS provides a CloudFront usage report that is more detailed than the billing report but less detailed than CloudFront access logs\. The usage report provides aggregate usage data by hour, day, or month; and it lists operations by region and usage type, such as data transferred out of the Australia region\.

The CloudFront usage report includes the following values:
+ **Service** — `AmazonCloudFront`
+ **Operation** — HTTP method\. Values include `DELETE`, `GET`, `HEAD`, `OPTIONS`, `PATCH`, `POST`, and `PUT`\.
+ **UsageType** — One of the following values
  + A code that identifies the type of data transfer
  + `Invalidations`
  + `SSL-Cert-Custom`

  For more information, see [Interpreting Your AWS Bill and the AWS Usage Report for CloudFront](billing-and-usage-interpreting.md)\.
+ **Resource** — Either the ID of the CloudFront distribution associated with the usage or the certificate ID of an SSL certificate that you have associated with a CloudFront distribution\.
+ **StartTime/EndTime** — The day that the usage applies to, in Coordinated Universal Time \(UTC\)\. 
+ **UsageValue** — \(1\) The number of requests during the specified time period or \(2\) the amount of data transferred in bytes\.

If you're using Amazon S3 as the origin for CloudFront, consider running the usage report for Amazon S3, too\. However, if you use Amazon S3 for purposes other than as an origin for your CloudFront distributions, it might not be clear what portion applies to your CloudFront usage\.

**Tip**  
For detailed information about every request that CloudFront receives for your objects, turn on CloudFront access logs for your distribution\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\. <a name="usage-report-procedure"></a>

**To download the usage report for CloudFront or Amazon S3**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/console/home](https://console.aws.amazon.com/console/home)\.

1. In the title bar, click your IAM user name, and click **Billing & Cost Management**\.

1. In the navigation pane, click **Reports**\.

1. Under **AWS Usage Report**, click **AWS Usage Report**\.

1. In the **Service** list, click **CloudFront** or **Amazon Simple Storage Service**\.

1. Select the settings to use:
   + **Usage Types** — For a detailed explanation of CloudFront usage types, see [Interpreting Your AWS Bill and the AWS Usage Report for CloudFront](billing-and-usage-interpreting.md)\.

     For Amazon S3, select **All Usage Types**\.
   + **Operation** — Select **All Operations**\.
   + **Time Period** — Select the time period that you want the report to cover\.
   + **Report Granularity** — Select whether you want the report to include subtotals by the hour, by the day, or by the month\.

1. Click the download button for the desired format\.

1. Follow the on\-screen prompts to view or save the report\.