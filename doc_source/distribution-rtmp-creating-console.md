# Creating an RTMP Distribution Using the CloudFront Console<a name="distribution-rtmp-creating-console"></a>

The following procedure explains how to create an RTMP distribution using the CloudFront console\. If you want to create an RTMP distribution using the CloudFront API, go to [POST Streaming Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateStreamingDistribution.html) in the *Amazon CloudFront API Reference*\. 

For the current limit on the number of RTMP distributions that you can create for each AWS account, see [Amazon CloudFront Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_cloudfront) in the *Amazon Web Services General Reference*\. To request a higher limit, go to [https://console\.aws\.amazon\.com/support/home\#/case/create?issueType=service\-limit\-increase&limitType=service\-code\-cloudfront\-distributions](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.<a name="distribution-rtmp-creating-console-procedure"></a>

**To create an RTMP distribution using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Click **Create Distribution**\.

1. On the first page of the **Create Distribution Wizard**, in the **RTMP** section, choose **Get Started**\.

1. Specify settings for the distribution\. For more information, see [Values that You Specify When You Create or Update an RTMP Distribution](distribution-rtmp-values-specify.md)\.

1. Click **Create Distribution**\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. If you chose to enable the distribution, it will then be ready to process requests\. This should take less than 15 minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. The domain name also appears on the **General** tab for a selected distribution\.