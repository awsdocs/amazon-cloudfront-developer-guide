# Creating or Updating a Web Distribution Using the CloudFront Console<a name="distribution-web-creating-console"></a>

The procedures in this topic explain how to create or update a web distribution using the CloudFront console\. If you want to create a web distribution using the CloudFront API, go to [POST Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateDistribution.html) in the *Amazon CloudFront API Reference*\. If you want to update a web distribution using the CloudFront API, go to [PUT DistributionConfig](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) in the *Amazon CloudFront API Reference*\. 

For the current limit on the number of web distributions that you can create for each AWS account, see [Amazon CloudFront Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_cloudfront) in the *Amazon Web Services General Reference*\. To request a higher limit, go to [https://console\.aws\.amazon\.com/support/home\#/case/create?issueType=service\-limit\-increase&limitType=service\-code\-cloudfront\-distributions](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.

+ [To create a CloudFront web distribution using the CloudFront console](#CreatingDownloadDistributionsConsoleProcedure)

+ [To update a CloudFront web distribution using the CloudFront console](#UpdatingDownloadDistributionsConsoleProcedure)

**To create a CloudFront web distribution using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the first page of the **Create Distribution Wizard**, in the **Web** section, choose **Get Started**\.

1. Specify settings for the distribution\. For more information, see [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md)\.

1. Save changes\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. If you chose to enable the distribution, it will then be ready to process requests\. 

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. \(It also appears on the **General** tab for a selected distribution\.\)

1. When your distribution is deployed, confirm that you can access your content using your new CloudFront URL or CNAME\. For more information, see [Testing Your Web Distribution](distribution-web-testing.md)\.

**To update a CloudFront web distribution using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID for the distribution that you want to update\.

1. Update the applicable values\. For more information, see [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md)\.

1. Choose **Create Distribution**\.

1. After you change settings, the value of the **Status** column for your distribution changes to **InProgress** while CloudFront propagates the changes to edge locations\. When **Status** changes to **Deployed**, the distribution is ready to process requests\. \(The value of the **State** column for the distribution must also be **Enabled**\.\)