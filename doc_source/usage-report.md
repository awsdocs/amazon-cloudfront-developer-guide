# AWS Usage Report for CloudFront<a name="usage-report"></a>

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
For detailed information about every request that CloudFront receives for your objects, turn on CloudFront access logs for your distribution\. For more information, see [Access Logs](AccessLogs.md)\. 

**To download the usage report for CloudFront or Amazon S3**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/console/home](https://console.aws.amazon.com/console/home)\.

1. In the title bar, click your IAM user name, and click **Billing & Cost Management**\.

1. In the navigation pane, click **Reports**\.

1. Under **AWS Usage Report**, click **AWS Usage Report**\.

1. In the **Service** list, click **CloudFront** or **Amazon Simple Storage Service**\.

1. Select the applicable settings:

   + **Usage Types** — For a detailed explanation of CloudFront usage types, see [[ERROR] BAD/MISSING LINK TEXT](billing-and-usage-interpreting.md)\.

     For Amazon S3, select **All Usage Types**\.

   + **Operation** — Select **All Operations**\.

   + **Time Period** — Select the time period that you want the report to cover\.

   + **Report Granularity** — Select whether you want the report to include subtotals by the hour, by the day, or by the month\.

1. Click the download button for the desired format\.

1. Follow the on\-screen prompts to view or save the report\.