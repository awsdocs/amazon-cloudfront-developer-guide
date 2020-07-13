# Restricting Access to Amazon S3 Content by Using an Origin Access Identity<a name="private-content-restricting-access-to-s3"></a>

To restrict access to content that you serve from Amazon S3 buckets, follow these steps:

1. Create a special CloudFront user called an origin access identity \(OAI\) and associate it with your distribution\.

1. Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users\. Make sure that users can’t use a direct URL to the S3 bucket to access a file there\.

After you take these steps, users can only access your files through CloudFront, not directly from the S3 bucket\.

In general, if you’re using an Amazon S3 bucket as the origin for a CloudFront distribution, you can either allow everyone to have access to the files there, or you can restrict access\. If you restrict access by using, for example, CloudFront signed URLs or signed cookies, you also won’t want people to be able to view files by simply using the direct Amazon S3 URL for the file\. Instead, you want them to only access the files by using the CloudFront URL, so your protections work\. For more information about using signed URLs and signed cookies, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

This topic explains in detail how to set up the OAI and grant permissions to maintain secure access to your S3 files\.

**Important**  
If you use an Amazon S3 bucket configured as a website endpoint, you must set it up with CloudFront as a custom origin\. You can’t use the origin access identity feature described in this topic\. However, you *can* restrict access to content on a custom origin by setting up custom headers and configuring your origin to require them\. For more information, see [ Restricting Access to Files on Custom Origins](private-content-overview.md#forward-custom-headers-restrict-access)\.

**Topics**
+ [Overview of OAI Setup](#private-content-restricting-access-to-s3-overview)
+ [Creating a CloudFront OAI and Adding it to Your Distribution](#private-content-creating-oai)
+ [Granting the OAI Permission to Read Files in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)
+ [Using an OAI in Amazon S3 Regions that Support Only Signature Version 4 Authentication](#private-content-origin-access-identity-signature-version-4)

## Overview of OAI Setup<a name="private-content-restricting-access-to-s3-overview"></a>

When you first set up an Amazon S3 bucket as the origin for a CloudFront distribution, you grant everyone permission to read the files in your bucket\. This allows anyone to access your files either through CloudFront or using the Amazon S3 URL\. CloudFront doesn't expose Amazon S3 URLs, but your users might have those URLs if your application serves any files directly from Amazon S3 or if anyone gives out direct links to specific files in Amazon S3\.

If you use CloudFront signed URLs or signed cookies to restrict access to files in your Amazon S3 bucket, you probably also want to prevent users from accessing your Amazon S3 files by using Amazon S3 URLs\. If users access your files directly in Amazon S3, they bypass the controls provided by CloudFront signed URLs or signed cookies\. This includes control over the date and time that a user can no longer access your content, and control over which IP addresses can be used to access content\. In addition, if users access files both through CloudFront and directly by using Amazon S3 URLs, CloudFront access logs are less useful because they're incomplete\.

To ensure that your users access your files using only CloudFront URLs, regardless of whether the URLs are signed, do the following:

1. Create an origin access identity, which is a special CloudFront user, and associate the origin access identity with your distribution\. You associate the origin access identity with origins, so that you can secure all or just some of your Amazon S3 content\. You can also create an origin access identity and add it to your distribution when you create the distribution\. For more information, see [Creating a CloudFront OAI and Adding it to Your Distribution](#private-content-creating-oai)\.

1. Change the permissions either on your Amazon S3 bucket or on the files in your bucket so that only the origin access identity has read permission \(or read and download permission\)\. When your users access your Amazon S3 files through CloudFront, the CloudFront origin access identity gets the files on behalf of your users\. If your users request files directly by using Amazon S3 URLs, they're denied access\. The origin access identity has permission to access files in your Amazon S3 bucket, but users don't\. For more information, see [Granting the OAI Permission to Read Files in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)\.

## Creating a CloudFront OAI and Adding it to Your Distribution<a name="private-content-creating-oai"></a>

An AWS account can have [up to 100 CloudFront origin access identities \(OAIs\)](cloudfront-limits.md#limits-web-distributions)\. However, you can add an OAI to as many distributions as you want, so one OAI is usually sufficient\.

If you didn’t create an OAI and add it to your distribution when you created the distribution, you can create and add one now by using either the CloudFront console or the CloudFront API:
+ **To use the CloudFront console** – You can create an OAI and add it to your distribution at the same time\. For step\-by\-step instructions, see [Creating an OAI and Adding it to Your Distribution](#private-content-creating-oai-console)\.
+ **To use the CloudFront API** – You create an OAI, and then you add it to your distribution\. For step\-by\-step instructions, see the following:
  + [Creating an OAI Using the CloudFront API](#private-content-creating-oai-api)
  + [Adding an OAI to Your Distribution Using the CloudFront API](#private-content-adding-oai-api)

**Note**  
To create OAIs, you must use the CloudFront console, or CloudFront API version 2009\-09\-09 or later\.

### Creating an OAI and Adding it to Your Distribution<a name="private-content-creating-oai-console"></a>

If you didn’t create an OAI when you created your distribution, do the following\.<a name="private-content-creating-oai-console-procedure"></a>

**To create a CloudFront OAI using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID of a distribution that has an S3 origin\.

1. Choose the **Origins and Origin Groups** tab\.

1. Choose the check box next to an origin, and then choose **Edit**\.

1. For **Restrict Bucket Access**, choose **Yes**\.

1. If you already have an OAI that you want to use, choose **Use an Existing Identity**\. Then choose the OAI in the **Your Identities** list\.
**Note**  
If you already have an OAI, we recommend that you reuse it to simplify maintenance\.

   If you want to create an OAI, choose **Create a New Identity**\. You can replace the bucket name in the **Comment** field with a custom description\.

1. If you want CloudFront to automatically give the OAI permission to read the files in the Amazon S3 bucket specified in **Origin Domain Name**, choose **Yes, Update Bucket Policy**\.
**Important**  
If you choose **Yes, Update Bucket Policy**, CloudFront updates bucket permissions to grant the specified OAI permission to read files in your bucket\. However, CloudFront does not remove existing permissions\. If users currently have permission to access the files in your bucket using Amazon S3 URLs, they will still have that permission after CloudFront updates your bucket permissions\. To view or remove existing bucket permissions, use a method provided by Amazon S3\.

   If you want to manually update permissions on your Amazon S3 bucket, choose **No, I Will Update Permissions**\. For more information, see [Granting the OAI Permission to Read Files in Your Amazon S3 Bucket](#private-content-granting-permissions-to-oai)\.

1. Choose **Yes, Edit**\.

1. If you have more than one origin, repeat the steps to add an OAI for each one\.

### Creating an OAI Using the CloudFront API<a name="private-content-creating-oai-api"></a>

If you already have an origin access identity and you want to reuse it instead of creating another one, skip to [Adding an OAI to Your Distribution Using the CloudFront API](#private-content-adding-oai-api)\.

To create a CloudFront OAI using the CloudFront API, use the `POST Origin Access Identity` API action\. The response includes an `Id` and an `S3CanonicalUserId` for the new OAI\. Make note of these values because you use them later in the process:
+ **Id element** – You use the value of the `Id` element to associate the OAI with your distribution\.
+ **S3CanonicalUserId element** – You use the value of the `S3CanonicalUserId` element when you use Amazon S3 object ACLs to give the OAI access to your Amazon S3 objects\.

For more information, see [CreateCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateCloudFrontOriginAccessIdentity.html) in the *Amazon CloudFront API Reference*\.

### Adding an OAI to Your Distribution Using the CloudFront API<a name="private-content-adding-oai-api"></a>

You can use the CloudFront API to add a CloudFront OAI to an existing distribution or to create a new distribution that includes an OAI\. In either case, include an `OriginAccessIdentity` element\. This element contains the value of the `Id` element that the `POST Origin Access Identity` API action returned when you created the OAI\. You can add the `OriginAccessIdentity` element to one or more origins\.

See the following topics in the *Amazon CloudFront API Reference*:
+ **Create a new web distribution** – [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)
+ **Update an existing web distribution** – [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)

## Granting the OAI Permission to Read Files in Your Amazon S3 Bucket<a name="private-content-granting-permissions-to-oai"></a>

When you create or update a distribution, you can add an origin access identity \(OAI\) and automatically update the Amazon S3 bucket policy to give the OAI permission to access your bucket\. Alternatively, you can choose to manually create or update the bucket policy, or use object ACLs that control access to individual files in the bucket\.

Whichever method you use, you should still review the permissions to make sure that:
+ Your CloudFront OAI can access files in the bucket on behalf of viewers who are requesting them through CloudFront\.
+ Viewers can’t use Amazon S3 URLs to access your files outside of CloudFront\.

**Important**  
If you configure CloudFront to accept and forward all of the HTTP methods that CloudFront supports, make sure you give your CloudFront OAI the desired permissions\. For example, if you configure CloudFront to accept and forward requests that use the `DELETE` method, configure your bucket policy or object ACLs to handle `DELETE` requests appropriately so viewers can delete only files that you want them to\.

Note the following:
+ You might find it easier to use Amazon S3 bucket policies than object ACLs because you can add files to the bucket without updating permissions\. However, ACLs give you more fine\-grained control because you’re granting permissions on each individual file\.
+ By default, your Amazon S3 bucket and all of the files in it are private\. Only the AWS account that created the bucket has permission to read or write the files in it\.
+ If another AWS account uploads files to your bucket, that account is the owner of those files\. Bucket policies only apply to files that the bucket owner owns\. This means that if another account uploads files to your bucket, the bucket policy that you created for your OAI not evaluated for those files\. In that case, use object ACLs to give permissions to your OAI\.
+ If you’re adding an OAI to an existing distribution, modify the bucket policy or any object ACLs as appropriate to ensure that the files are not publicly available outside of CloudFront\.
+ Grant additional permissions to one or more secure administrator accounts so you can continue to update the contents of the Amazon S3 bucket\.

**Important**  
There might be a brief delay between when you save your changes to Amazon S3 permissions and when the changes take effect\. Until the changes take effect, you might get “permission denied” errors when you try to access files in your bucket\.

### Using Amazon S3 Bucket Policies<a name="private-content-updating-s3-bucket-policies"></a>

You can give a CloudFront OAI access to files in an Amazon S3 bucket by creating or updating the bucket policy in the following ways:
+ Using the Amazon S3 bucket’s **Permissions** tab in the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.
+ Using [PutBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketPolicy.html) in the Amazon S3 API\.
+ Using the [CloudFront console](https://console.aws.amazon.com/cloudfront/home)\. When you add an OAI to your origin settings in the CloudFront console, you can choose **Yes, Update Bucket Policy** to tell CloudFront to update the bucket policy on your behalf\.

If you update the bucket policy manually, make sure that you:
+ Specify the correct OAI as the `Principal` in the policy\.
+ Give the OAI the permissions it needs to access objects on behalf of viewers\.

For more information, see the following sections\.

#### Specify an OAI as the `Principal`<a name="private-content-updating-s3-bucket-policies-principal"></a>

To specify an OAI as the `Principal` in an Amazon S3 bucket policy, use the OAI’s Amazon Resource Name \(ARN\), which includes the OAI’s ID\. For example:

```
"Principal": {
    "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity EH1HDMB1FH2TC"
}
```

To use the preceding example, replace *EH1HDMB1FH2TC* with the OAI’s ID\. To find the OAI’s ID, see the [Origin Access Identity page](https://console.aws.amazon.com/cloudfront/home?region=us-east-1#oai:) in the CloudFront console, or use [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

You can also specify an OAI as the `Principal` by using its Amazon S3 canonical ID\. For example:

```
"Principal": {
    "CanonicalUser": "79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be"
}
```

To use the preceding example, replace *79a59df900b949e55d96a1e698fbacedfd6e09d98eacf8f8d5218e7cd47ef2be* with the OAI’s canonical ID\. You can find the OAI’s canonical ID in the same ways that you find its ID\.

**Note**  
One advantage of using the OAI’s ARN to specify it as the `Principal` is that it’s easier to understand who the bucket policy is granting access to\. Amazon S3 canonical IDs can refer to different kinds of AWS identities, not just CloudFront OAIs, and it can be difficult to determine which identity a canonical ID refers to\. Using the OAI’s ARN can make it easier to understand the bucket policy\.  
Also, when you use the OAI’s canonical ID in a bucket policy, AWS replaces the canonical ID with the OAI’s ARN\. When you write a policy that specifies an OAI’s canonical ID and then later view the same policy, you’ll see that the canonical ID has been replaced by the corresponding ARN\. For this reason, it might make sense to just write the policy using the OAI’s ARN\.

#### Give Permissions to an OAI<a name="private-content-updating-s3-bucket-policies-permissions"></a>

To give the OAI the permissions to access objects in your Amazon S3 bucket, use keywords in the policy that relate to specific Amazon S3 API operations\. For example, the `s3:GetObject` permission allows the OAI to read objects in the bucket\. For more information, see the examples in the following section, or see [Specifying Permissions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service Developer Guide*\.

#### Amazon S3 Bucket Policy Examples<a name="private-content-updating-s3-bucket-policies-examples"></a>

The following examples show Amazon S3 bucket policies that grant access to a CloudFront OAI\. To use these examples:
+ Replace *EH1HDMB1FH2TC* with the OAI’s ID\. To find the OAI’s ID, see the [Origin Access Identity page](https://console.aws.amazon.com/cloudfront/home?region=us-east-1#oai:) in the CloudFront console, or use [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.
+ Replace *awsexamplebucket* with the name of your Amazon S3 bucket\.

**Example Amazon S3 bucket policy that gives the OAI read access**  
The following example allows the OAI to read objects in the specified bucket \(`s3:GetObject`\)\.  

```
{
    "Version": "2012-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity EH1HDMB1FH2TC"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::awsexamplebucket/*"
        }
    ]
}
```

**Example Amazon S3 bucket policy that gives the OAI read and write access**  
The following example allows the OAI to read and write objects in the specified bucket \(`s3:GetObject` and `s3:PutObject`\)\. This allows viewers to upload files to your Amazon S3 bucket through CloudFront\.  

```
{
    "Version": "2012-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity EH1HDMB1FH2TC"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::aws-example-bucket/*"
        }
    ]
}
```

### Updating Amazon S3 Object ACLs<a name="private-content-updating-s3-acls"></a>

You can give a CloudFront OAI access to files in an Amazon S3 bucket by creating or updating the file’s ACL in the following ways:
+ Using the Amazon S3 object’s **Permissions** tab in the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.
+ Using [PutObjectAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectAcl.html) in the Amazon S3 API\.

When you grant access to an OAI using an ACL, you must specify the OAI using its Amazon S3 canonical user ID\. This is the value of **Amazon S3 Canonical User ID** on the [Origin Access Identity page](https://console.aws.amazon.com/cloudfront/home?region=us-east-1#oai:) in the CloudFront console\. If you’re using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the OAI, or call [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

For more information about Amazon S3 object ACLs, see [Managing Access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Using an OAI in Amazon S3 Regions that Support Only Signature Version 4 Authentication<a name="private-content-origin-access-identity-signature-version-4"></a>

Newer Amazon S3 Regions require that you use Signature Version 4 for authenticated requests\. \(For the versions of signature supported in each Amazon S3 Region, see [Amazon Simple Storage Service \(S3\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in the topic [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.\) However, when you create an origin access identity and add it to a CloudFront distribution, CloudFront typically uses Signature Version 4 for authentication when it requests files in your Amazon S3 bucket\. If you're using an origin access identity and if your bucket is in one of the Regions that requires Signature Version 4 for authentication, note the following:
+ `DELETE`, `GET`, `HEAD`, `OPTIONS`, and `PATCH` requests are supported without qualifications\.
+ If you want to submit `PUT` requests to CloudFront to upload files to your Amazon S3 bucket, you must add an `x-amz-content-sha256` header to the request\.The header value must contain an SHA256 hash of the body of the request\. For more information, see the documentation about the `x-amz-content-sha256` header on the [Common Request Headers](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonRequestHeaders.html) page in the *Amazon Simple Storage Service API Reference*\.
+ `POST` requests are not supported\.