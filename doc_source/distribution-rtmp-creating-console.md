# Creating an RTMP Distribution Using the CloudFront Console<a name="distribution-rtmp-creating-console"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

The following procedure explains how to create an RTMP distribution using the CloudFront console\. If you want to create an RTMP distribution using the CloudFront API, go to [CreateStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateStreamingDistribution.html) in the *Amazon CloudFront API Reference*\. 

For the current quota \(formerly known as limit\) on the number of RTMP distributions that you can create for each AWS account, see [Quotas](cloudfront-limits.md)\. To request a higher quota, go to [https://console\.aws\.amazon\.com/support/home\#/case/create?issueType=service\-limit\-increase&limitType=service\-code\-cloudfront\-distributions](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.<a name="distribution-rtmp-creating-console-procedure"></a>

**To create an RTMP distribution using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Click **Create Distribution**\.

1. On the first page of the **Create Distribution Wizard**, in the **RTMP** section, choose **Get Started**\.

1. Specify settings for the distribution\. For more information, see [Values that You Specify When You Create or Update an RTMP Distribution](distribution-rtmp-values-specify.md)\.

1. Click **Create Distribution**\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. If you chose to enable the distribution, it will then be ready to process requests\. This should take less than 15 minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. The domain name also appears on the **General** tab for a selected distribution\.