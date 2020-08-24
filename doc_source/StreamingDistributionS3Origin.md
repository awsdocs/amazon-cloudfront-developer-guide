# Using an Amazon S3 Bucket as the Origin for an RTMP Distribution<a name="StreamingDistributionS3Origin"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

When you create a distribution, you specify where CloudFront gets the files that it distributes to edge locations\. For an RTMP distribution, you must use an Amazon S3 bucket; custom origins are not supported\. To get your objects into your bucket, you can use any method supported by Amazon S3, for example, the Amazon S3 API or a third\-party tool\. You can create a hierarchy in your bucket just as you would with any other Amazon S3 bucket\. You incur regular Amazon S3 charges for storing the objects in the bucket\. For more information about the charges to use CloudFront, see [CloudFront Reports in the Console](reports.md)\.

Using an existing Amazon S3 bucket as your CloudFront origin server doesn't change the bucket in any way; you can still use it as you normally would to store and access Amazon S3 objects \(at the normal Amazon S3 prices\)\. 

You can use the same Amazon S3 bucket for both RTMP and web distributions\. 

**Note**  
After you create an RTMP distribution, you can't change its origin server\. If you need to change the Amazon S3 bucket for an RTMP distribution, you must create a new distribution that uses the new bucket and update either your links or your DNS records to use the domain name for the new distribution\. You can then delete the original distribution\. For more information, see [Deleting a Distribution](HowToDeleteDistribution.md)\.

When you specify the Amazon S3 bucket that you want CloudFront to get objects from, we recommend that you use the following format to access the bucket:

`bucket-name.s3.region.amazonaws.com`

Another option might be to use the following more general format, but be aware that this format doesn't work for Regions launched in 2019 or later:

`bucket-name.s3.amazonaws.com`

Do not specify the name of the bucket using the following values:
+ The Amazon S3 path style, `s3.amazonaws.com/bucket-name`
+ The Amazon S3 CNAME, if any

**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.