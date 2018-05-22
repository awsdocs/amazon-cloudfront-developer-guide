# Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content<a name="private-content-restricting-access-to-s3"></a>

**Topics**
+ [Creating a CloudFront Origin Access Identity and Adding it to Your Distribution](#private-content-creating-oai)
+ [Granting the Origin Access Identity Permission to Read Objects in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)
+ [Using an Origin Access Identity in Amazon S3 Regions that Support Only Signature Version 4 Authentication](#private-content-origin-access-identity-signature-version-4)

Typically, if you're using an Amazon S3 bucket as the origin for a CloudFront distribution, you grant everyone permission to read the objects in your bucket\. This allows anyone to access your objects either through CloudFront or using the Amazon S3 URL\. CloudFront doesn't expose Amazon S3 URLs, but your users might have those URLs if your application serves any objects directly from Amazon S3 or if anyone gives out direct links to specific objects in Amazon S3\.

**Note**  
You can also restrict access to content on a custom origin by using custom headers\. For more information, see [Using Custom Headers to Restrict Access to Your Content on a Custom Origin](forward-custom-headers.md#forward-custom-headers-restrict-access)\.

If you want to use CloudFront signed URLs or signed cookies to provide access to objects in your Amazon S3 bucket, you probably also want to prevent users from accessing your Amazon S3 objects using Amazon S3 URLs\. If users access your objects directly in Amazon S3, they bypass the controls provided by CloudFront signed URLs or signed cookies, for example, control over the date and time that a user can no longer access your content and control over which IP addresses can be used to access content\. In addition, if users access objects both through CloudFront and directly by using Amazon S3 URLs, CloudFront access logs are less useful because they're incomplete\.

**Note**  
To create origin access identities, you must use the CloudFront console or CloudFront API version 2009\-09\-09 or later\.

To ensure that your users access your objects using only CloudFront URLs, regardless of whether the URLs are signed, perform the following tasks:

1. Create an origin access identity, which is a special CloudFront user, and associate the origin access identity with your distribution\. \(For web distributions, you associate the origin access identity with origins, so you can secure all or just some of your Amazon S3 content\.\) You can also create an origin access identity and add it to your distribution when you create the distribution\. For more information, see [Creating a CloudFront Origin Access Identity and Adding it to Your Distribution](#private-content-creating-oai)\.

1. Change the permissions either on your Amazon S3 bucket or on the objects in your bucket so only the origin access identity has read permission \(or read and download permission\)\. When your users access your Amazon S3 objects through CloudFront, the CloudFront origin access identity gets the objects on behalf of your users\. If your users request objects directly by using Amazon S3 URLs, they're denied access\. The origin access identity has permission to access objects in your Amazon S3 bucket, but users don't\. For more information, see [Granting the Origin Access Identity Permission to Read Objects in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)\.

For detailed information about setting up a private Amazon S3 bucket to use with CloudFront, see [ How to Set Up and Serve Private Content Using S3 and Amazon CloudFront](http://improve.dk/how-to-set-up-and-serve-private-content-using-s3/)\.

## Creating a CloudFront Origin Access Identity and Adding it to Your Distribution<a name="private-content-creating-oai"></a>

An AWS account can have up to 100 CloudFront origin access identities\. However, you can add an origin access identity to as many distributions as you want, so one origin access identity is usually sufficient\. 

If you didn't create an origin access identity and add it to your distribution when you created the distribution, you can create and add one now using either the CloudFront console or the CloudFront API:
+ **If you're using the CloudFront console** – You can create an origin access identity and add it to your distribution at the same time\. For more information, see [Creating an Origin Access Identity and Adding it to Your Distribution Using the CloudFront Console](#private-content-creating-oai-console)\.
+ **If you're using the CloudFront API** – You create an origin access identity and then you add it to your distribution\. Perform the procedure in each of the following topics:
  + [Creating an Origin Access Identity Using the CloudFront API](#private-content-creating-oai-api)
  + [Adding an Origin Access Identity to Your Distribution Using the CloudFront API](#private-content-adding-oai-api)

### Creating an Origin Access Identity and Adding it to Your Distribution Using the CloudFront Console<a name="private-content-creating-oai-console"></a>

If you didn't create an origin access identity when you created your distribution, perform the following procedure\. <a name="private-content-creating-oai-console-procedure"></a>

**To create a CloudFront origin access identity using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Click the ID of the distribution that you want to add an origin access identity to\.

1. Change to edit mode:
   + **Web distributions** – Click the **Origins** tab, select the origin that you want to edit, and click **Edit**\. You can only create an origin access identity for origins for which **Origin Type** is **S3 Origin**\.
   + **RTMP distributions** – Click **Edit**\.

1. For **Restrict Bucket Access**, click **Yes**\.

1. If you already have an origin access identity that you want to use, click **Use an Existing Identity**\. Then select the identity in the **Your Identities** list\.
**Note**  
If you already have an origin access identity, we recommend that you reuse it to simplify maintenance\. 

   If you want to create an identity, click **Create a New Identity**\. Then enter a description for the identity in the **Comment** field\.

1. If you want CloudFront to automatically give the origin access identity permission to read the objects in the Amazon S3 bucket specified in **Origin Domain Name**, click **Yes, Update Bucket Policy**\.
**Important**  
If you click **Yes, Update Bucket Policy**, CloudFront updates bucket permissions to grant the specified origin access identity the permission to read objects in your bucket\. However, CloudFront does not remove existing permissions\. If users currently have permission to access the objects in your bucket using Amazon S3 URLs, they will still have that permission after CloudFront updates your bucket permissions\. To view or remove existing bucket permissions, use a method provided by Amazon S3\. For more information, see [Granting the Origin Access Identity Permission to Read Objects in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)\.

   If you want to manually update permissions on your Amazon S3 bucket, click **No, I Will Update Permissions**\.

1. Click **Yes, Edit**\.

1. If you're adding an origin access identity to a web distribution and you have more than one origin, repeat step 3 through step 7 as applicable\.

### Creating an Origin Access Identity Using the CloudFront API<a name="private-content-creating-oai-api"></a>

If you already have an origin access identity and you want to reuse it instead of creating another one, skip to [Adding an Origin Access Identity to Your Distribution Using the CloudFront API](#private-content-adding-oai-api)\.

To create a CloudFront origin access identity using the CloudFront API, use the `POST Origin Access Identity` API action\. The response includes an `Id` and an `S3CanonicalUserId` for the new origin access identity\. Make note of these values because you will use them later in the process:
+ **Id element** – You use the value of the `Id` element to associate an origin access ID with your distribution\.
+ **S3CanonicalUserId element** – You use the value of the `S3CanonicalUserId` element when you give CloudFront access to your Amazon S3 bucket or objects\. 

For more information about the `POST Origin Access Identity API` action, go to [POST Origin Access Identity](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateOAI.html) in the *Amazon CloudFront API Reference*\. For a list of other actions that you can perform on origin access identities, go to [Actions on Origin Access Identities](http://docs.aws.amazon.com/cloudfront/latest/APIReference/Actions_OAI.html), also in the *Amazon CloudFront API Reference*\.

### Adding an Origin Access Identity to Your Distribution Using the CloudFront API<a name="private-content-adding-oai-api"></a>

You can use the CloudFront API to add a CloudFront origin access identity to an existing distribution or to create a new distribution that includes an origin access identity\. In either case, include an `OriginAccessIdentity` element\. This element contains the value of the `Id` element that the `POST Origin Access Identity` API action returned when you created the origin access identity\. For web distributions, add the `OriginAccessIdentity` element to one or more origins\. For RTMP distributions, add the `OriginAccessIdentity` element to the distribution\.

See the applicable topic in the *Amazon CloudFront API Reference*:
+ **Create a new web distribution** – [POST Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)
+ **Update an existing web distribution** – [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_PutConfig.html)
+ **Create a new RTMP distribution** – [POST Streaming Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateStreamingDistribution.html)
+ **Update an existing RTMP distribution** – [PUT Streaming Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_PutStreamingDistConfig.html)

## Granting the Origin Access Identity Permission to Read Objects in Your Amazon S3 Bucket<a name="private-content-granting-permissions-to-oai"></a>

When you create or update a distribution, you can add an origin access identity and automatically update the bucket policy to give the origin access identity permission to access your bucket\. Alternatively, you can choose to manually change the bucket policy or change ACLs, which control permissions on individual objects in your bucket\.

Whichever method you use, you should still review the bucket policy for your bucket and review the permissions on your objects to ensure that:
+ CloudFront can access objects in the bucket on behalf of users who are requesting your objects through CloudFront\.
+ Users can't use Amazon S3 URLs to access your objects\.

**Important**  
If you configure CloudFront to accept and forward to Amazon S3 all of the HTTP methods that CloudFront supports, create a CloudFront origin access identity to restrict access to your Amazon S3 content, and grant the origin access identity the applicable permissions\. For example, if you configure CloudFront to accept and forward these methods because you want to use the `PUT` method, you must configure Amazon S3 bucket policies or ACLs to handle `DELETE` requests appropriately so users can't delete resources that you don't want them to\. 

Note the following:
+ You might find it easier to update Amazon S3 bucket policies than ACLs because you can add objects to the bucket without updating permissions\. However, ACLs give you more fine\-grained control because you're granting permissions on each object\.
+ By default, your Amazon S3 bucket and all of the objects in it are private—only the AWS account that created the bucket has permission to read or write the objects in it\.
+ If you're adding an origin access identity to an existing distribution, modify the bucket policy or any object ACLs as appropriate to ensure that the objects are not publicly available\.
+ Grant additional permissions to one or more secure administrator accounts so you can continue to update the contents of the Amazon S3 bucket\.

**Important**  
There might be a brief delay between when you save your changes to Amazon S3 permissions and when the changes take effect\. Until the changes take effect, you can get permission\-denied errors when you try to access objects in your bucket\.

### Updating Amazon S3 Bucket Policies<a name="private-content-updating-s3-bucket-policies"></a>

You can update the Amazon S3 bucket policy using either the AWS Management Console or the Amazon S3 API:
+ Grant the CloudFront origin access identity the applicable permissions on the bucket\.

  To specify an origin access identity, use the value of **Amazon S3 Canonical User ID** on the **Origin Access Identity** page in the CloudFront console\. If you're using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the origin access identity\.
+ Deny access to anyone that you don't want to have access using Amazon S3 URLs\.

For more information, go to [Using Bucket Policies and User Policies](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucketPolicies.html) in the *Amazon Simple Storage Service Developer Guide*\.

For an example, see "Granting Permission to an Amazon CloudFront Origin Identity" in the topic [Bucket Policy Examples](http://docs.aws.amazon.com/AmazonS3/latest/dev/AccessPolicyLanguage_UseCases_s3_a.html), also in the *Amazon Simple Storage Service Developer Guide*\.

### Updating Amazon S3 ACLs<a name="private-content-updating-s3-acls"></a>

Using either the AWS Management Console or the Amazon S3 API, change the Amazon S3 ACL:
+ Grant the CloudFront origin access identity the applicable permissions on each object that the CloudFront distribution serves\.

  To specify an origin access identity, use the value of **Amazon S3 Canonical User ID** on the **Origin Access Identity** page in the CloudFront console\. If you're using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the origin access identity\.
+ Deny access to anyone that you don't want to have access using Amazon S3 URLs\.

If another AWS account uploads objects to your bucket, that account is the owner of those objects\. Bucket policies only apply to objects that the bucket owner owns\. This means that if another account uploads objects to your bucket, the bucket policy that you created for your OAI will not be evaluated for those objects\.

For more information, see [Managing Access with ACLs](http://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service Developer Guide*\.

You can also change the ACLs programmatically by using one of the AWS SDKs\. For an example, see the downloadable sample code in [Create a URL Signature Using C\# and the \.NET Framework](CreateSignatureInCSharp.md)\.

## Using an Origin Access Identity in Amazon S3 Regions that Support Only Signature Version 4 Authentication<a name="private-content-origin-access-identity-signature-version-4"></a>

Newer Amazon S3 regions require that you use signature version 4 for authenticated requests\. \(For the versions of signature supported in each Amazon S3 region, see [Amazon Simple Storage Service \(S3\)](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in the topic [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.\) However, when you create an origin access identity and add it to a CloudFront distribution, CloudFront typically uses signature version 2 for authentication when it requests objects in your Amazon S3 bucket\. If you're using an origin access identity and if your bucket is in one of the regions that requires signature version 4 for authentication, note the following:
+ `DELETE`, `GET`, `HEAD`, `OPTIONS`, and `PATCH` requests are supported without qualifications\.
+ If you want to submit `PUT` requests to CloudFront to upload objects to your Amazon S3 bucket, you must add an `x-amz-content-sha256` header to the request, and the header value must contain a SHA256 hash of the body of the request\. For more information, see the documentation about the `x-amz-content-sha256` header on the [Common Request Headers](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonRequestHeaders.html) page in the *Amazon Simple Storage Service API Reference*\.
+ `POST` requests are not supported\.