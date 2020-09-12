# Values that You Specify When You Create or Update an RTMP Distribution<a name="distribution-rtmp-values-specify"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

To stream media files using CloudFront, you create an RTMP distribution and specify the following values\. 

**Topics**
+ [Origin Domain Name \(Amazon S3 Bucket\)](#StreamingDistValuesDNSName)
+ [Restrict Bucket Access \(Amazon S3 Only\)](#StreamingDistValuesOAIRestrictBucketAccess)
+ [Origin Access Identity \(Amazon S3 Only\)](#StreamingDistValuesOAI)
+ [Comment for New Identity\(Amazon S3 Only\)](#StreamingDistValuesOAIComment)
+ [Your Identities \(Amazon S3 Only\)](#StreamingDistValuesOAIYourIdentities)
+ [Grant Read Permissions on Bucket \(Amazon S3 Only\)](#StreamingDistValuesOAIGrantReadPermissions)
+ [Price Class](#StreamingDistValuesPriceClass)
+ [Alternate Domain Names \(CNAMEs\)](#StreamingDistValuesCNAME)
+ [Logging](#StreamingDistValuesLoggingOnOff)
+ [Bucket for Logs](#StreamingDistValuesLogBucket)
+ [Log Prefix](#StreamingDistValuesLogPrefix)
+ [Comment](#StreamingDistValuesComment)
+ [Distribution State](#StreamingDistValuesEnabled)
+ [Restrict Viewer Access \(Use Signed URLs\)](#StreamingDistValuesRestrictViewerAccess)
+ [Trusted Signers](#StreamingDistValuesTrustedSigners)
+ [AWS Account Numbers](#StreamingDistValuesAWSAccountNumbers)

## Origin Domain Name \(Amazon S3 Bucket\)<a name="StreamingDistValuesDNSName"></a>

The DNS domain name of the Amazon S3 bucket from which you want CloudFront to get objects for this origin, for example, `DOC-EXAMPLE-BUCKET1.s3.amazonaws.com`\. In the CloudFront console, click in the **Origin Domain Name** field, and a list enumerates the Amazon S3 buckets that are associated with the current AWS account\. To use a bucket from a different AWS account, type the domain name of the bucket in the following format:

`bucket-name.s3.region.amazonaws.com` 

If you configured Amazon S3 Transfer Acceleration for your bucket, do not specify the `s3-accelerate` endpoint for **Origin Domain Name**\.

The files must be publicly readable unless you secure your content in Amazon S3 by using a CloudFront origin access identity\. For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

**Important**  
The bucket name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

When you change the bucket from which CloudFront gets objects for the current origin, CloudFront immediately begins replicating the change to CloudFront edge locations\. Until the distribution configuration is updated in a given edge location, CloudFront will continue to forward requests to the previous Amazon S3 bucket\. As soon as the distribution configuration is updated in that edge location, CloudFront begins to forward requests to the new Amazon S3 bucket\.

Changing the bucket does not require CloudFront to repopulate edge caches with objects from the new origin\. As long as the viewer requests in your application have not changed, CloudFront will continue to serve objects that are already in an edge cache until the TTL on each object expires or until seldom\-requested objects are evicted\. 

For more information, see [Using an Amazon S3 Bucket as the Origin for an RTMP Distribution](StreamingDistributionS3Origin.md)\.

## Restrict Bucket Access \(Amazon S3 Only\)<a name="StreamingDistValuesOAIRestrictBucketAccess"></a>

Click **Yes** if you want to require end users to access objects in an Amazon S3 bucket by using only CloudFront URLs, not by using Amazon S3 URLs\. Then specify the applicable values\.

Click **No** if you want end users to be able to access objects using either CloudFront URLs or Amazon S3 URLs\.

For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

## Origin Access Identity \(Amazon S3 Only\)<a name="StreamingDistValuesOAI"></a>

If you chose **Yes** for **Restrict Bucket Access**, choose whether to create a new origin access identity or use an existing one that is associated with your AWS account\. If you already have an origin access identity, we recommend that you reuse it to simplify maintenance\. For more information about origin access identities, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

## Comment for New Identity\(Amazon S3 Only\)<a name="StreamingDistValuesOAIComment"></a>

If you chose **Create a New Identity** for **Origin Access Identity**, enter a comment that identifies the new origin access identity\. CloudFront will create the origin access identity when you create this distribution\.

## Your Identities \(Amazon S3 Only\)<a name="StreamingDistValuesOAIYourIdentities"></a>

If you chose **Use an Existing Identity** for **Origin Access Identity**, choose the origin access identity that you want to use\. You cannot use an origin access identity that is associated with another AWS account\.

## Grant Read Permissions on Bucket \(Amazon S3 Only\)<a name="StreamingDistValuesOAIGrantReadPermissions"></a>

If you want CloudFront to automatically grant the origin access identity the permission to read objects in your Amazon S3 bucket, click **Yes, Update Bucket Policy**\. 

**Important**  
If you click **Yes, Update Bucket Policy**, CloudFront updates the bucket policy to grant the specified origin access identity the permission to read objects in your bucket\. However, CloudFront does not remove existing permissions in the bucket policy or permissions on individual objects\. If users currently have permission to access the objects in your bucket using Amazon S3 URLs, they will still have that permission after CloudFront updates your bucket policy\. To view or change the existing bucket policy and the existing permissions on the objects in your bucket, use a method provided by Amazon S3\. For more information, see [Granting the OAI Permission to Read Files in Your Amazon S3 Bucket](private-content-restricting-access-to-s3.md#private-content-granting-permissions-to-oai)\.

If you want to update permissions manually, for example, if you want to update ACLs on your objects instead of updating bucket permissions, click **No, I will Update Permissions**\.

## Price Class<a name="StreamingDistValuesPriceClass"></a>

The price class that corresponds with the maximum price that you want to pay for CloudFront service\. By default, CloudFront serves your objects from edge locations in all CloudFront regions\. 

For more information about price classes and about how your choice of price class affects CloudFront performance for your distribution, see [Choosing the price class for a CloudFront distribution](PriceClass.md)\. For information about CloudFront pricing, including how price classes map to CloudFront regions, go to [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

## Alternate Domain Names \(CNAMEs\)<a name="StreamingDistValuesCNAME"></a>

Optional\. You can associate one or more CNAME aliases with a distribution so that you can use your domain name \(for example, example\.com\) in the URLs for your objects instead of using the domain name that CloudFront assigned when you created your distribution\. For more information, see [Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.

## Logging<a name="StreamingDistValuesLoggingOnOff"></a>

Whether you want CloudFront to log information about each request for an object and store the log files in an Amazon S3 bucket\. You can enable or disable logging at any time\. There is no extra charge if you enable logging, but you accrue the usual Amazon S3 charges for storing and accessing the files in an Amazon S3 bucket\. You can delete the logs at any time\. For more information about CloudFront access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

## Bucket for Logs<a name="StreamingDistValuesLogBucket"></a>

If you chose **On** for **Logging**, the Amazon S3 bucket that you want CloudFront to store access logs in, for example, `myLogs-DOC-EXAMPLE-BUCKET.s3.amazonaws.com`\. If you enable logging, CloudFront records information about each end\-user request for an object and stores the files in the specified Amazon S3 bucket\. You can enable or disable logging at any time\. For more information about CloudFront access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**Note**  
You must have the permissions required to get and update Amazon S3 bucket ACLs, and the S3 ACL for the bucket must grant you `FULL_CONTROL`\. This allows CloudFront to give the awsdatafeeds account permission to save log files in the bucket\. For more information, see [Permissions Required to Configure Standard Logging and to Access Your Log Files](AccessLogs.md#AccessLogsBucketAndFileOwnership)\.

## Log Prefix<a name="StreamingDistValuesLogPrefix"></a>

Optional\. If you chose **On** for **Logging**, specify the string, if any, that you want CloudFront to prefix to the access log filenames for this distribution, for example, `exampleprefix/`\. The trailing slash \( / \) is optional but recommended to simplify browsing your log files\. For more information about CloudFront access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

## Comment<a name="StreamingDistValuesComment"></a>

Optional\. When you create a distribution, you can include a comment of up to 128 characters\. You can update the comment at any time\.

## Distribution State<a name="StreamingDistValuesEnabled"></a>

When you create a distribution, you must specify whether you want the distribution to be enabled or disabled after it's created:
+ *Enabled* means that as soon as the distribution is fully deployed you can deploy links that use the distribution's domain name and end users can retrieve content\. Whenever a distribution is enabled, CloudFront accepts and processes any end\-user requests for content that use the domain name associated with that distribution\.

  When you create, modify, or delete a CloudFront distribution, it takes time for your changes to propagate to the CloudFront database\. An immediate request for information about a distribution might not show the change\. Propagation usually completes within minutes, but a high system load or network partition might increase this time\. 
+ *Disabled* means that even though the distribution might be deployed and ready to use, end users can't use it\. When a distribution is disabled, CloudFront doesn't accept any end\-user requests that use the domain name associated with that distribution\. Until you switch the distribution from disabled to enabled \(by updating the distribution's configuration\), no one can use it\.

You can toggle a distribution between disabled and enabled as often as you want\. For information about updating a distribution's configuration, see [Updating a Distribution](HowToUpdateDistribution.md)\.

## Restrict Viewer Access \(Use Signed URLs\)<a name="StreamingDistValuesRestrictViewerAccess"></a>

If you want requests for objects served by this distribution to use public URLs, click **No**\. If you want requests to use signed URLs, click **Yes**\. Then specify the AWS accounts that you want to use to create signed URLs; these accounts are known as trusted signers\.

For more information about trusted signers, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\. 

## Trusted Signers<a name="StreamingDistValuesTrustedSigners"></a>

Choose which AWS accounts you want to use as trusted signers for this distribution:
+ **Self:** Use the account with which you're currently signed into the AWS Management Console as a trusted signer\. If you're currently signed in as an IAM user, the associated AWS account is added as a trusted signer\. 
+ **Specify Accounts:** Enter account numbers for trusted signers in the **AWS Account Numbers** field\.

To create signed URLs, an AWS account must have at least one active CloudFront key pair\.

**Important**  
If you're updating a distribution that you're already using to distribute content, add trusted signers only when you're ready to start generating signed URLs for your objects\. After you add trusted signers to a distribution, users must use signed URLs to access the objects served by this distribution\.

## AWS Account Numbers<a name="StreamingDistValuesAWSAccountNumbers"></a>

If you want to create signed URLs using AWS accounts in addition to or instead of the current account, enter one AWS account number per line in this field\. Note the following:
+ The accounts that you specify must have at least one active CloudFront key pair\. For more information, see [Creating CloudFront Key Pairs for Your Trusted Signers](private-content-trusted-signers.md#private-content-creating-cloudfront-key-pairs)\.
+ You can't create CloudFront key pairs for IAM users, so you can't use IAM users as trusted signers\.
+ For information about how to get the AWS account number for an account, see [How Do I Get Security Credentials?](https://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *Amazon Web Services General Reference*\.
+ If you enter the account number for the current account, CloudFront automatically checks the **Self** checkbox and removes the account number from the **AWS Account Numbers** list\.