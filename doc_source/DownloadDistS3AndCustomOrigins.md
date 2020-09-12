# Using Amazon S3 Origins, MediaPackage Channels, and Custom Origins for Web Distributions<a name="DownloadDistS3AndCustomOrigins"></a>

When you create a distribution, you specify where CloudFront sends requests for the files\. CloudFront supports using several AWS resources as origins\. For example, you can specify an Amazon S3 bucket or a MediaStore container, a MediaPackage channel, or a custom origin, such as an Amazon EC2 instance or your own HTTP web server\.

**Topics**
+ [Using Amazon S3 Buckets for Your Origin](#concept_S3Origin)
+ [Using Amazon S3 Buckets Configured as Website Endpoints for Your Origin](#concept_S3Origin_website)
+ [Using a MediaStore Container or a MediaPackage Channel for Your Origin](#concept_AWS_Media)
+ [Using Amazon EC2 or Other Custom Origins](#concept_CustomOrigin)
+ [Using CloudFront Origin Groups](#concept_origin_groups)
+ [Adding CloudFront When You're Already Distributing Content from Amazon S3](#adding-cloudfront-to-s3)
+ [Moving an Amazon S3 Bucket to a Different Region](#move-s3-bucket-different-region)

## Using Amazon S3 Buckets for Your Origin<a name="concept_S3Origin"></a>

When you use Amazon S3 as an origin for your distribution, you place any objects that you want CloudFront to deliver in an Amazon S3 bucket\. You can use any method that is supported by Amazon S3 to get your objects into Amazon S3, for example, the Amazon S3 console or API, or a third\-party tool\. You can create a hierarchy in your bucket to store the objects, just as you would with any other Amazon S3 bucket\.

Using an existing Amazon S3 bucket as your CloudFront origin server doesn't change the bucket in any way; you can still use it as you normally would to store and access Amazon S3 objects at the standard Amazon S3 price\. You incur regular Amazon S3 charges for storing the objects in the bucket\. For more information about the charges to use CloudFront, see [CloudFront Reports in the Console](reports.md)\.

**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

When you specify the Amazon S3 bucket that you want CloudFront to get objects from, we recommend that you use the following format to access the bucket:

`bucket-name.s3.region.amazonaws.com`

Another option might be to use the following more general format, but be aware that this format doesn't work for Regions launched in 2019 or later:

`bucket-name.s3.amazonaws.com`

When you specify the bucket name in this format, you can use the following CloudFront features:
+ Configure CloudFront to communicate with your Amazon S3 bucket using SSL\. For more information, see [Using HTTPS with CloudFront](using-https.md)\.
+ Use an origin access identity to require that your users access your content using CloudFront URLs, not by using Amazon S3 URLs\. For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.
+ Update the content of your bucket by submitting `POST` and `PUT` requests to CloudFront\. For more information, see [HTTP Methods](RequestAndResponseBehaviorS3Origin.md#RequestS3HTTPMethods) in the topic [How CloudFront Processes and Forwards Requests to Your Amazon S3 Origin Server](RequestAndResponseBehaviorS3Origin.md#RequestBehaviorS3Origin)\.

Do not specify the bucket using the following formats:
+ The Amazon S3 path style, `s3.amazonaws.com/bucket-name`
+ The Amazon S3 CNAME, if any

## Using Amazon S3 Buckets Configured as Website Endpoints for Your Origin<a name="concept_S3Origin_website"></a>

You can set up an Amazon S3 bucket that is configured as a website endpoint as custom origin with CloudFront\.
+ When you configure your CloudFront distribution, for the origin, enter the Amazon S3 static website hosting endpoint for your bucket\. This value appears in the Amazon S3 console, on the **Properties** tab, in the **Static website hosting** pane\. For example:

  `http://bucket-name.s3-website-region.amazonaws.com`

For more information about specifying Amazon S3 static website endpoints, see [Website endpoints](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteEndpoints.html) in the *Amazon Simple Storage Service Developer Guide*\.

When you specify the bucket name in this format as your origin, you can use Amazon S3 redirects and Amazon S3 custom error documents\. For more information about Amazon S3 features, see the [Amazon S3 documentation](https://docs.aws.amazon.com/s3/)\. \(CloudFront also provides custom error pages\. For more information, see [Creating a Custom Error Page for Specific HTTP Status Codes](custom-error-pages.md)\.\)

Using an Amazon S3 bucket as your CloudFront origin server doesn’t change it in any way\. You can still use it as you normally would and you incur regular Amazon S3 charges\. For more information about the charges to use CloudFront, see [CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

**Note**  
If you use the CloudFront API to create your distribution with an Amazon S3 bucket that is configured as a website endpoint, you must configure it by using `CustomOriginConfig`, even though the website is hosted in an Amazon S3 bucket\. For more information about creating distributions by using the CloudFront API, see [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) in the *Amazon CloudFront API Reference*\.

## Using a MediaStore Container or a MediaPackage Channel for Your Origin<a name="concept_AWS_Media"></a>

To stream video by using CloudFront, you can set up an Amazon S3 bucket that is configured as a MediaStore container, or create a channel and endpoints with MediaPackage\. Then you create and configure a distribution in CloudFront to stream the video\.

For more information and step\-by\-step instructions, see the following topics:
+ [Serving Video Using AWS Elemental MediaStore as the Origin](live-streaming.md#video-streaming-mediastore)
+ [Serving Live Video Formatted with AWS Elemental MediaPackage](live-streaming.md#live-streaming-with-mediapackage)

## Using Amazon EC2 or Other Custom Origins<a name="concept_CustomOrigin"></a>

A custom origin is an HTTP server, for example, a web server\. The HTTP server can be an Amazon Elastic Compute Cloud \(Amazon EC2\) instance or an HTTP server that you manage privately\. An Amazon S3 origin configured as a website endpoint is also considered a custom origin\.

When you use a custom origin that is your own HTTP server, you specify the DNS name of the server, along with the HTTP and HTTPS ports and the protocol that you want CloudFront to use when fetching objects from your origin\.

Most CloudFront features are supported when you use a custom origin with the following exceptions:
+ **RTMP distributions**—Not supported\.
+ **Private content**—Although you can use a signed URL to distribute content from a custom origin, for CloudFront to access the custom origin, the origin must remain publicly accessible\. For more information, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

Follow these guidelines for using Amazon EC2 instances and other custom origins with CloudFront\. 
+ Host and serve the same content on all servers that are serving content for the same CloudFront origin\. For more information, see [Origin Settings](distribution-web-values-specify.md#DownloadDistValuesOrigin) in the [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md) topic\.
+ Log the `X-Amz-Cf-Id` header entries on all servers; CloudFront requires this information for debugging\.
+ Restrict access requests to the HTTP and HTTPS ports that your custom origin listens on\.
+ Synchronize the clocks of all servers in your implementation\. Note that CloudFront uses Coordinated Universal Time \(UTC\) for signed URLs and signed cookies, for access logs, and reports\. In addition, if you monitor CloudFront activity using CloudWatch metrics, note that CloudWatch also uses UTC\.
+ Use redundant servers to handle failures\.
+ For information about using a custom origin to serve private content, see [ Restricting Access to Files on Custom Origins](private-content-overview.md#forward-custom-headers-restrict-access)\.
+ For information about request and response behavior and about supported HTTP status codes, see [Request and Response Behavior](RequestAndResponseBehavior.md)\.

If you use Amazon EC2 for your custom origins, we recommend that you do the following:

1. Use an Amazon Machine Image that automatically installs the software for a web server\. For more information, see the [Amazon EC2 documentation](http://aws.amazon.com/documentation/ec2/)\.

1. Use an Elastic Load Balancing load balancer to handle traffic across multiple Amazon EC2 instances and to isolate your application from changes to Amazon EC2 instances\. For example, if you use a load balancer, you can add and delete Amazon EC2 instances without changing your application\. For more information, see the [Elastic Load Balancing documentation](http://aws.amazon.com/documentation/elasticloadbalancing/)\.

1. When you create your CloudFront distribution, specify the URL of the load balancer for the domain name of your origin server\. For more information, see [Creating a Distribution](distribution-web-creating-console.md)\.

## Using CloudFront Origin Groups<a name="concept_origin_groups"></a>

You can specify an origin group for your CloudFront origin if, for example, you want to configure origin failover for scenarios when you need high availability\. Use origin failover to designate a primary origin for CloudFront plus a second origin that CloudFront automatically switches to when the primary origin returns specific HTTP status code failure responses\. 

To see the steps for setting up an origin group and for more information, see [Optimizing High Availability with CloudFront Origin Failover](high_availability_origin_failover.md)\.

## Adding CloudFront When You're Already Distributing Content from Amazon S3<a name="adding-cloudfront-to-s3"></a>

If you store your objects in an Amazon S3 bucket, you can either have users get your objects directly from S3, or you can configure CloudFront to get your objects from S3 and then distribute them to your users\.

**Note**  
To learn more about using Amazon S3 buckets for your origin with CloudFront, including when you have an Amazon S3 bucket configured as a website endpoint, see [Using Amazon S3 Origins, MediaPackage Channels, and Custom Origins for Web Distributions](#DownloadDistS3AndCustomOrigins)\.

Using CloudFront can be more cost effective if your users access your objects frequently because, at higher usage, the price for CloudFront data transfer is lower than the price for Amazon S3 data transfer\. In addition, downloads are faster with CloudFront than with Amazon S3 alone because your objects are stored closer to your users\.

**Note**  
If you want CloudFront to respect Amazon S3 cross\-origin resource sharing settings, configure CloudFront to forward the `Origin` header to Amazon S3\. For more information, see [Caching Content Based on Request Headers](header-caching.md)\.

If you currently distribute content directly from your Amazon S3 bucket using your own domain name \(such as example\.com\) instead of the domain name of your Amazon S3 bucket \(such as MyAWSBucket\.s3\.us\-west\-2\.amazonaws\.com\), you can add CloudFront with no disruption by using the following procedure\.<a name="migrate-s3-to-cloudfront-process"></a>

**To add CloudFront when you're already distributing your content from Amazon S3**

1. Create a CloudFront distribution using one of the following procedures:
   + [Steps for Creating a Distribution \(Overview\)](distribution-web-creating.md)
   + [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)

   When you create the distribution, specify the name of your Amazon S3 bucket as the origin server\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, see [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

   If you're using a CNAME with Amazon S3, specify the CNAME for your distribution, too\.

1. Create a test webpage that contains links to publicly readable objects in your Amazon S3 bucket, and test the links\. For this initial test, use the CloudFront domain name of your distribution in the object URLs, for example, `http://d111111abcdef8.cloudfront.net/images/image.jpg`\. 

   For more information about the format of CloudFront URLs, see [Customizing the URL Format for Files in CloudFront](LinkFormat.md)\.

1. If you're using Amazon S3 CNAMEs, your application uses your domain name \(for example, example\.com\) to reference the objects in your Amazon S3 bucket instead of using the name of your bucket \(for example, *DOC\-EXAMPLE\-BUCKET1*\.s3\.amazonaws\.com\)\. To continue using your domain name to reference objects instead of using the CloudFront domain name for your distribution \(for example, d111111abcdef8\.cloudfront\.net\), you need to update your settings with your DNS service provider\.

   For Amazon S3 CNAMEs to work, your DNS service provider must have a CNAME resource record set for your domain that currently routes queries for the domain to your Amazon S3 bucket\. For example, if a user requests this object:

   `http://example.com/images/image.jpg`

   The request is automatically rerouted, and the user sees this object:

   `http://DOC-EXAMPLE-BUCKET1.s3.amazonaws.com/images/image.jpg`

   To route queries to your CloudFront distribution instead of your Amazon S3 bucket, you need to use the method provided by your DNS service provider to update the CNAME resource record set for your domain\. This updated CNAME record starts to redirect DNS queries from your domain to the CloudFront domain name for your distribution\. For more information, see the documentation provided by your DNS service provider\.
**Note**  
If you're using Route 53 as your DNS service, you can use either a CNAME resource record set or an alias resource record set\. For information about editing resource record sets, see [Editing Resource Record Sets](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html)\. For information about alias resource record sets, see [Choosing Between Alias and Non\-Alias Resource Record Sets](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)\. Both topics are in the *Amazon Route 53 Developer Guide*\.

   For more information about using CNAMEs with CloudFront, see [Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.

   After you update the CNAME resource record set, it can take up to 72 hours for the change to propagate throughout the DNS system, although it usually happens faster\. During this time, some requests for your content will continue to be routed to your Amazon S3 bucket, and others will be routed to CloudFront\. 

## Moving an Amazon S3 Bucket to a Different Region<a name="move-s3-bucket-different-region"></a>

If you're using Amazon S3 as the origin for a CloudFront distribution and you move the bucket to a different Region, CloudFront can take up to an hour to update its records to include the change of Region when both of the following are true:
+ You're using a CloudFront origin access identity \(OAI\) to restrict access to the bucket\.
+ You move the bucket to an Amazon S3 Region that requires Signature Version 4 for authentication\.

When you're using OAIs, CloudFront uses the Region \(among other values\) to calculate the signature that it uses to request objects from your bucket\. For more information about OAIs, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\. For a list of Amazon S3 Regions and the signature versions that they support, see [Amazon Simple Storage Service \(Amazon S3\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in the "Regions and Endpoints" chapter of the *Amazon Web Services General Reference*\.

To force a faster update to CloudFront's records, you can update your CloudFront distribution, for example, by updating the **Comment** field on the **General** tab in the CloudFront console\. When you update a distribution, CloudFront immediately checks on the Region that your bucket is in; propagation of the change to all edge locations should take less than 15 minutes\.