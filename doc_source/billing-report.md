# AWS Billing Report for CloudFront<a name="billing-report"></a>

You can view a summary of your AWS usage and charges, listed by service, on the Bills page in the AWS Management Console\.

You can also download a more detailed version of the report in CSV format\. The detailed billing report includes the following values that are applicable to CloudFront:

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

  + The sum of the prorated months that you had SSL certificates associated with enabled CloudFront distributions\. For example, if you have one certificate associated with an enabled distribution for an entire month and another certificate associated with an enabled distribution for half of the month, this value will be 1\.5\. 

**To display summary billing information and download the detailed billing report**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/console/home](https://console.aws.amazon.com/console/home)\.

1. In the title bar, click your IAM user name, and click **Billing & Cost Management**\.

1. In the navigation pane, click **Bills**\.

1. To view summary information for CloudFront, under **Details**, click **CloudFront**\.

1. To download a detailed billing report in CSV format, click **Download CSV**, and follow the on\-screen prompts to save the report\.