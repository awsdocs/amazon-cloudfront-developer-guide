# Restricting Access to Amazon S3 Content by Using an Origin Access Identity<a name="private-content-restricting-access-to-s3"></a>

To restrict access to content that you serve from Amazon S3 buckets, you create CloudFront signed URLs or signed cookies to limit access to files in your Amazon S3 bucket, and then you create a special CloudFront user called an origin access identity \(OAI\) and associate it with your distribution\. Then you configure permissions so that CloudFront can use the OAI to access and serve files to your users, but users can't use a direct URL to the S3 bucket to access a file there\. Taking these steps help you maintain secure access to the files that you serve through CloudFront\.

In general, if you're using an Amazon S3 bucket as the origin for a CloudFront distribution, you can either allow everyone to have access to the files there, or you can restrict access\. If you limit access by using, for example, CloudFront signed URLs or signed cookies, you also won't want people to be able to view files by simply using the direct URL for the file\. Instead, you want them to only access the files by using the CloudFront URL, so your protections work\. For more information about using signed URLs and signed cookies, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)

This topic explains in detail how to set up the OAI and grant permissions to maintain secure access to your S3 files\.

**Important**  
If you use an Amazon S3 bucket configured as a website endpoint, you must set it up with CloudFront as a custom origin and you can't use the origin access identity feature described in this topic\. However, you *can* restrict access to content on a custom origin by setting up custom headers and configuring your origin to require them\. For more information, see [ Restricting Access to Files on Custom Origins](private-content-overview.md#forward-custom-headers-restrict-access)\.

**Topics**
+ [Overview of Origin Access Identity Setup](#private-content-restricting-access-to-s3-overview)
+ [Creating a CloudFront Origin Access Identity and Adding it to Your Distribution](#private-content-creating-oai)
+ [Granting the Origin Access Identity Permission to Read Files in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)
+ [Using an Origin Access Identity in Amazon S3 Regions that Support Only Signature Version 4 Authentication](#private-content-origin-access-identity-signature-version-4)

## Overview of Origin Access Identity Setup<a name="private-content-restricting-access-to-s3-overview"></a>

When you first set up an Amazon S3 bucket as the origin for a CloudFront distribution, you grant everyone permission to read the files in your bucket\. This allows anyone to access your files either through CloudFront or using the Amazon S3 URL\. CloudFront doesn't expose Amazon S3 URLs, but your users might have those URLs if your application serves any files directly from Amazon S3 or if anyone gives out direct links to specific files in Amazon S3\.

If you use CloudFront signed URLs or signed cookies to limit access to files in your Amazon S3 bucket, you probably also want to prevent users from accessing your Amazon S3 files by using Amazon S3 URLs\. If users access your files directly in Amazon S3, they bypass the controls provided by CloudFront signed URLs or signed cookies, for example, control over the date and time that a user can no longer access your content and control over which IP addresses can be used to access content\. In addition, if users access files both through CloudFront and directly by using Amazon S3 URLs, CloudFront access logs are less useful because they're incomplete\.

To ensure that your users access your files using only CloudFront URLs, regardless of whether the URLs are signed, do the following:

1. Create an origin access identity, which is a special CloudFront user, and associate the origin access identity with your distribution\. You associate the origin access identity with origins, so that you can secure all or just some of your Amazon S3 content\. You can also create an origin access identity and add it to your distribution when you create the distribution\. For more information, see [Creating a CloudFront Origin Access Identity and Adding it to Your Distribution](#private-content-creating-oai)\.

1. Change the permissions either on your Amazon S3 bucket or on the files in your bucket so that only the origin access identity has read permission \(or read and download permission\)\. When your users access your Amazon S3 files through CloudFront, the CloudFront origin access identity gets the files on behalf of your users\. If your users request files directly by using Amazon S3 URLs, they're denied access\. The origin access identity has permission to access files in your Amazon S3 bucket, but users don't\. For more information, see [Granting the Origin Access Identity Permission to Read Files in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)\.

**Tip**  
If you're just getting started with setting up CloudFront with private content on an S3 bucket, you might find it helpful to read through the following blog post for an end\-to\-end view of the process: [How to Set Up and Serve Private Content Using S3 and Amazon CloudFront](http://improve.dk/how-to-set-up-and-serve-private-content-using-s3/)\. 

## Creating a CloudFront Origin Access Identity and Adding it to Your Distribution<a name="private-content-creating-oai"></a>

An AWS account can have [up to 100 CloudFront origin access identities](cloudfront-limits.md#limits-web-distributions)\. However, you can add an origin access identity to as many distributions as you want, so one origin access identity is usually sufficient\.

If you didn't create an origin access identity and add it to your distribution when you created the distribution, you can create and add one now by using either the CloudFront console or the CloudFront API:
+ **To use the CloudFront console** – You can create an origin access identity and add it to your distribution at the same time\. For step\-by\-step instructions, see [Creating an Origin Access Identity and Adding it to Your Distribution](#private-content-creating-oai-console)\.
+ **To use the CloudFront API** – You create an origin access identity, and then you add it to your distribution\. For step\-by\-step instructions, see the following:
  + [Creating an Origin Access Identity Using the CloudFront API](#private-content-creating-oai-api)
  + [Adding an Origin Access Identity to Your Distribution Using the CloudFront API](#private-content-adding-oai-api)

**Note**  
To create origin access identities, you must use the CloudFront console or CloudFront API version 2009\-09\-09 or later\.

### Creating an Origin Access Identity and Adding it to Your Distribution<a name="private-content-creating-oai-console"></a>

If you didn't create an origin access identity when you created your distribution, do the following\. <a name="private-content-creating-oai-console-procedure"></a>

**To create a CloudFront origin access identity using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Click the ID of a distribution that has an S3 origin, and then choose **Distribution Settings**\.

1. Choose the **Origins** tab\.

1. Choose an origin, and then choose **Edit**\.

1. For **Restrict Bucket Access**, choose **Yes**\.
**Note**  
If you don't see the **Restrict Bucket Access** option, your Amazon S3 origin might be configured as a website endpoint\. In that configuration, S3 buckets must be set up with CloudFront as custom origins and you can't use an origin access identity with them\.

1. If you already have an origin access identity that you want to use, click **Use an Existing Identity**\. Then select the identity in the **Your Identities** list\.
**Note**  
If you already have an origin access identity, we recommend that you reuse it to simplify maintenance\. 

   If you want to create an identity, click **Create a New Identity**\. If you like, you can replace the bucket name in the **Comment** field with a custom description\.

1. If you want CloudFront to automatically give the origin access identity permission to read the files in the Amazon S3 bucket specified in **Origin Domain Name**, click **Yes, Update Bucket Policy**\.
**Important**  
If you choose **Yes, Update Bucket Policy**, CloudFront updates bucket permissions to grant the specified origin access identity the permission to read files in your bucket\. However, CloudFront does not remove existing permissions\. If users currently have permission to access the files in your bucket using Amazon S3 URLs, they will still have that permission after CloudFront updates your bucket permissions\. To view or remove existing bucket permissions, use a method provided by Amazon S3\. For more information, see [Granting the Origin Access Identity Permission to Read Files in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)\.

   If you want to manually update permissions on your Amazon S3 bucket, choose **No, I Will Update Permissions**\.

1. Choose **Yes, Edit**\.

1. If you have more than one origin, repeat the steps to add an origin access identity for each one\.

### Creating an Origin Access Identity Using the CloudFront API<a name="private-content-creating-oai-api"></a>

If you already have an origin access identity and you want to reuse it instead of creating another one, skip to [Adding an Origin Access Identity to Your Distribution Using the CloudFront API](#private-content-adding-oai-api)\.

To create a CloudFront origin access identity by using the CloudFront API, use the `POST Origin Access Identity` API action\. The response includes an `Id` and an `S3CanonicalUserId` for the new origin access identity\. Make note of these values because you will use them later in the process:
+ **Id element** – You use the value of the `Id` element to associate an origin access ID with your distribution\.
+ **S3CanonicalUserId element** – You use the value of the `S3CanonicalUserId` element when you give CloudFront access to your Amazon S3 bucket or files\. 

For more information, see [CreateCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateCloudFrontOriginAccessIdentity.html) in the *Amazon CloudFront API Reference*\.  

### Adding an Origin Access Identity to Your Distribution Using the CloudFront API<a name="private-content-adding-oai-api"></a>

You can use the CloudFront API to add a CloudFront origin access identity to an existing distribution or to create a new distribution that includes an origin access identity\. In either case, include an `OriginAccessIdentity` element\. This element contains the value of the `Id` element that the `POST Origin Access Identity` API action returned when you created the origin access identity\. You can add the `OriginAccessIdentity` element to one or more origins\.

See the following topics in the *Amazon CloudFront API Reference*:
+ **Create a new web distribution** – [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)
+ **Update an existing web distribution** – [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)

## Granting the Origin Access Identity Permission to Read Files in Your Amazon S3 Bucket<a name="private-content-granting-permissions-to-oai"></a>

When you create or update a distribution, you can add an origin access identity and automatically update the bucket policy to give the origin access identity permission to access your bucket\. Alternatively, you can choose to manually change the bucket policy or change ACLs, which control permissions on individual files in your bucket\.

Whichever method you use, you should still review the bucket policy for your bucket and review the permissions on your files to ensure that:
+ CloudFront can access files in the bucket on behalf of users who are requesting your files through CloudFront\.
+ Users can't use Amazon S3 URLs to access your files\.

**Important**  
If you configure CloudFront to accept and forward to Amazon S3 all of the HTTP methods that CloudFront supports, create a CloudFront origin access identity to restrict access to your Amazon S3 content, and grant the origin access identity the desired permissions\. For example, if you configure CloudFront to accept and forward these methods because you want to use the `PUT` method, you must configure Amazon S3 bucket policies or ACLs to handle `DELETE` requests appropriately so users can't delete resources that you don't want them to\. 

Note the following:
+ You might find it easier to update Amazon S3 bucket policies than ACLs because you can add files to the bucket without updating permissions\. However, ACLs give you more fine\-grained control because you're granting permissions on each file\.
+ By default, your Amazon S3 bucket and all of the files in it are private—only the AWS account that created the bucket has permission to read or write the files in it\.
+ If you're adding an origin access identity to an existing distribution, modify the bucket policy or any file ACLs as appropriate to ensure that the files are not publicly available\.
+ Grant additional permissions to one or more secure administrator accounts so you can continue to update the contents of the Amazon S3 bucket\.

**Important**  
There might be a brief delay between when you save your changes to Amazon S3 permissions and when the changes take effect\. Until the changes take effect, you can get permission\-denied errors when you try to access files in your bucket\.

### Updating Amazon S3 Bucket Policies<a name="private-content-updating-s3-bucket-policies"></a>

You can update the Amazon S3 bucket policy by using either the AWS Management Console or the Amazon S3 API:
+ Grant the CloudFront origin access identity the desired permissions on the bucket\.

  To specify an origin access identity, use the value of **Amazon S3 Canonical User ID** on the **Origin Access Identity** page in the CloudFront console\. If you're using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the origin access identity\.
+ Deny access to anyone that you don't want to have access using Amazon S3 URLs\.

For more information, go to [Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucketPolicies.html) in the *Amazon Simple Storage Service Developer Guide*\.

For an example, see "Granting Permission to an Amazon CloudFront Origin Identity" in the topic [Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/AccessPolicyLanguage_UseCases_s3_a.html), also in the *Amazon Simple Storage Service Developer Guide*\.

### Updating Amazon S3 ACLs<a name="private-content-updating-s3-acls"></a>

Using either the AWS Management Console or the Amazon S3 API, change the Amazon S3 ACL:
+ Grant the CloudFront origin access identity the desired permissions on each file that the CloudFront distribution serves\.

  To specify an origin access identity, use the value of **Amazon S3 Canonical User ID** on the **Origin Access Identity** page in the CloudFront console\. If you're using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the origin access identity\.
+ Deny access to anyone that you don't want to have access using Amazon S3 URLs\.

If another AWS account uploads files to your bucket, that account is the owner of those files\. Bucket policies only apply to files that the bucket owner owns\. This means that if another account uploads files to your bucket, the bucket policy that you created for your OAI will not be evaluated for those files\.

For more information, see [Managing Access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service Developer Guide*\.

You can also change the ACLs programmatically by using one of the AWS SDKs\. For an example, see the downloadable sample code in [Create a URL Signature Using C\# and the \.NET Framework](CreateSignatureInCSharp.md)\.

## Using an Origin Access Identity in Amazon S3 Regions that Support Only Signature Version 4 Authentication<a name="private-content-origin-access-identity-signature-version-4"></a>

Newer Amazon S3 regions require that you use signature version 4 for authenticated requests\. \(For the versions of signature supported in each Amazon S3 region, see [Amazon Simple Storage Service \(S3\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in the topic [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.\) However, when you create an origin access identity and add it to a CloudFront distribution, CloudFront typically uses signature version 4 for authentication when it requests files in your Amazon S3 bucket\. If you're using an origin access identity and if your bucket is in one of the regions that requires signature version 4 for authentication, note the following:
+ `DELETE`, `GET`, `HEAD`, `OPTIONS`, and `PATCH` requests are supported without qualifications\.
+ If you want to submit `PUT` requests to CloudFront to upload files to your Amazon S3 bucket, you must add an `x-amz-content-sha256` header to the request, and the header value must contain a SHA256 hash of the body of the request\. For more information, see the documentation about the `x-amz-content-sha256` header on the [Common Request Headers](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonRequestHeaders.html) page in the *Amazon Simple Storage Service API Reference*\.
+ `POST` requests are not supported\.