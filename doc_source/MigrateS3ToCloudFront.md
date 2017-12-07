# Using CloudFront with Amazon S3<a name="MigrateS3ToCloudFront"></a>

You can store your content in an Amazon S3 bucket and use CloudFront to distribute the content\. This topic explains how to use CloudFront with your S3 bucket, and how to update your CloudFront distribution if you move the S3 bucket to a different region\. 


+ [Adding CloudFront When You're Distributing Content from Amazon S3](#adding-cloudfront-to-s3)
+ [Moving an Amazon S3 Bucket to a Different Region](#move-s3-bucket-different-region)

## Adding CloudFront When You're Distributing Content from Amazon S3<a name="adding-cloudfront-to-s3"></a>

If you store your objects in an Amazon S3 bucket, you can either have your users get your objects directly from S3, or you can configure CloudFront to get your objects from S3 and distribute them to your users\.

Using CloudFront can be more cost effective if your users access your objects frequently because, at higher usage, the price for CloudFront data transfer is lower than the price for Amazon S3 data transfer\. In addition, downloads are faster with CloudFront than with Amazon S3 alone because your objects are stored closer to your users\.

**Note**  
If you want CloudFront to respect Amazon S3 cross\-origin resource sharing settings, configure CloudFront to forward the `Origin` header to Amazon S3\. For more information, see [Configuring CloudFront to Cache Objects Based on Request Headers](header-caching.md)\.

If you currently distribute content directly from your Amazon S3 bucket using your own domain name \(such as example\.com\) instead of the domain name of your Amazon S3 bucket \(such as MyAWSBucket\.s3\.amazonaws\.com\), you can add CloudFront with no disruption by using the following procedure\.

**To add CloudFront when you're already distributing your content from Amazon S3**

1. Create a CloudFront distribution using the procedure described in the applicable topic:

   + [Task List for Creating a Web Distribution](distribution-web-creating.md)

   + [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)

   When you create the distribution, specify the name of your Amazon S3 bucket as the origin server\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, see [Bucket Restrictions and Limitations](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

   If you're using a CNAME with Amazon S3, specify the CNAME for your distribution, too\.

1. Create a test web page that contains links to publicly readable objects in your Amazon S3 bucket, and test the links\. For this initial test, use the CloudFront domain name of your distribution in the object URLs, for example, `http://d111111abcdef8.cloudfront.net/images/image.jpg`\. 

   For more information about the format of CloudFront URLs, see [Format of URLs for Objects](LinkFormat.md)\.

1. If you're using Amazon S3 CNAMEs, your application uses your domain name \(for example, example\.com\) to reference the objects in your Amazon S3 bucket instead of using the name of your bucket \(for example, myawsbucket\.s3\.amazonaws\.com\)\. To continue using your domain name to reference objects instead of using the CloudFront domain name for your distribution \(for example, d111111abcdef8\.cloudfront\.net\), you need to update your settings with your DNS service provider\.

   For Amazon S3 CNAMEs to work, your DNS service provider must have a CNAME resource record set for your domain that currently routes queries for the domain to your Amazon S3 bucket\. For example, if a user requests this object:

   `http://example.com/images/image.jpg`

   the request is automatically rerouted, and the user sees this object:

   `http://myawsbucket.s3.amazonaws.com/images/image.jpg`

   To route queries to your CloudFront distribution instead of your Amazon S3 bucket, you need to use the method provided by your DNS service provider to update the CNAME resource record set for your domain\. This updated CNAME record will start to redirect DNS queries from your domain to the CloudFront domain name for your distribution\. For more information, see the documentation provided by your DNS service provider\.
**Note**  
If you're using Amazon Route 53 as your DNS service, you can use either a CNAME resource record set or an alias resource record set\. For information about editing resource record sets, see [Editing Resource Record Sets](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html)\. For information about alias resource record sets, see [Choosing Between Alias and Non\-Alias Resource Record Sets](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)\. Both topics are in the *Amazon Route 53 Developer Guide*\.

   For more information about using CNAMEs with CloudFront, see [Using Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.

   After you update the CNAME resource record set, it can take up to 72 hours for the change to propagate throughout the DNS system, although it usually happens faster\. During this time, some requests for your content will continue to be routed to your Amazon S3 bucket, and others will be routed to CloudFront\. 

## Moving an Amazon S3 Bucket to a Different Region<a name="move-s3-bucket-different-region"></a>

If you're using Amazon S3 as the origin for a CloudFront distribution and you move the bucket to a different region, CloudFront can take up to an hour to update its records to include the change of region when both of the following are true:

+ You're using a CloudFront origin access identity \(OAI\) to restrict access to the bucket

+ You move the bucket to an Amazon S3 region that requires Signature Version 4 for authentication

When you're using OAIs, CloudFront uses the region \(among other values\) to calculate the signature that it uses to request objects from your bucket\. For more information about OAIs, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\. For a list of Amazon S3 regions and the signature versions that they support, see [Amazon Simple Storage Service \(Amazon S3\)](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in the "Regions and Endpoints" chapter of the *Amazon Web Services General Reference*\.

To force a faster update to CloudFront's records, you can update your CloudFront distribution, for example, by updating the **Comment** field on the **General** tab in the CloudFront console\. When you update a distribution, CloudFront immediately checks on the region that your bucket is in; propagation of the change to all edge locations should take less than 15 minutes\.