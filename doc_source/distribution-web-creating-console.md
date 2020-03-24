# Creating a Distribution<a name="distribution-web-creating-console"></a>

You can create or update a distribution by using the CloudFront console or programmatically\. This topic is about working with distributions by using the console\.

If you want to create or update a distribution by using the CloudFront API, see [ Create Distribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) or [Update Distribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\. 

**Important**  
When you update your distribution, be aware that a number of additional fields are required that are not required to create a distribution\. To help make sure that all of the required fields are included when you update your distribution by using the CloudFront API, follow the steps described in the *Amazon CloudFront API Reference*\.

To see the current maximum number of distributions that you can create for each AWS account, or to request a higher quota \(formerly known as limit\), see [General Quotas on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.<a name="CreatingDownloadDistributionsConsoleProcedure"></a>

**To create a CloudFront web distribution \(console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the first page of the **Create Distribution Wizard**, in the **Web** section, choose **Get Started**\.

1. Specify settings for the distribution\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

1. Save changes\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. If you chose to enable the distribution, it will be ready to process requests after the status switches to **Deployed**\. 

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. \(It also appears on the **General** tab for a selected distribution\.\) 
**Tip**  
You can use an alternate domain name, instead of the name assigned to you by CloudFront; by following the steps in [Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.

1. When your distribution is deployed, confirm that you can access your content using your new CloudFront URL or CNAME\. For more information, see [Testing a Distribution](distribution-web-testing.md)\.

To update a distribution \(for example, to add or change cache behaviors\), see [Updating a Distribution](HowToUpdateDistribution.md)\.