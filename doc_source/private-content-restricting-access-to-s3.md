# Restricting access to Amazon S3 content by using an origin access identity \(OAI\)<a name="private-content-restricting-access-to-s3"></a>

To restrict access to content that you serve from Amazon S3 buckets, follow these steps:

1. Create a special CloudFront user called an origin access identity \(OAI\) and associate it with your distribution\.

1. Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users\. Make sure that users can’t use a direct URL to the S3 bucket to access a file there\.

After you take these steps, users can only access your files through CloudFront, not directly from the S3 bucket\.

**Note**  
If you restrict access to files by using CloudFront signed URLs or signed cookies, you don’t want viewers to be able to view the files by simply using the direct Amazon S3 URL for the file\. Instead, you want them to only access the files by using the CloudFront URL, so your protections work\. For more information about using signed URLs and signed cookies, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.

This topic explains in detail how to set up the OAI and grant permissions to maintain secure access to your S3 files\.

**Important**  
If you use an Amazon S3 bucket configured as a website endpoint, you must set it up with CloudFront as a custom origin\. You can’t use the origin access identity feature described in this topic\. However, you *can* restrict access to content on a custom origin by setting up custom headers and configuring your origin to require them\. For more information, see [ Restricting access to files on custom origins](private-content-overview.md#forward-custom-headers-restrict-access)\.

**Topics**
+ [Overview of OAI setup](#private-content-restricting-access-to-s3-overview)
+ [Creating a CloudFront OAI and adding it to your distribution](#private-content-creating-oai)
+ [Granting the OAI permission to read files in your Amazon S3 bucket](#private-content-granting-permissions-to-oai)
+ [Using an OAI in Amazon S3 regions that support only signature version 4 authentication](#private-content-origin-access-identity-signature-version-4)

## Overview of OAI setup<a name="private-content-restricting-access-to-s3-overview"></a>

When you first set up an Amazon S3 bucket as the origin for a CloudFront distribution, you grant everyone permission to read the files in your bucket\. This allows anyone to access your files either through CloudFront or using the Amazon S3 URL\. CloudFront doesn't expose Amazon S3 URLs, but your users might have those URLs if your application serves any files directly from Amazon S3 or if anyone gives out direct links to specific files in Amazon S3\.

If you use CloudFront signed URLs or signed cookies to restrict access to files in your Amazon S3 bucket, you probably also want to prevent users from accessing your Amazon S3 files by using Amazon S3 URLs\. If users access your files directly in Amazon S3, they bypass the controls provided by CloudFront signed URLs or signed cookies\. This includes control over the date and time that a user can no longer access your content, and control over which IP addresses can be used to access content\. In addition, if users access files both through CloudFront and directly by using Amazon S3 URLs, CloudFront access logs are less useful because they're incomplete\.

To ensure that your users access your files using only CloudFront URLs, regardless of whether the URLs are signed, do the following:

1. Create an origin access identity, which is a special CloudFront user, and associate the origin access identity with your distribution\. You associate the origin access identity with origins, so that you can secure all or just some of your Amazon S3 content\. You can also create an origin access identity and add it to your distribution when you create the distribution\. For more information, see [Creating a CloudFront OAI and adding it to your distribution](#private-content-creating-oai)\.

1. Change the permissions either on your Amazon S3 bucket or on the files in your bucket so that only the origin access identity has read permission\. When viewers access your Amazon S3 files through CloudFront, the CloudFront origin access identity gets the files on their behalf\. If viewers request files directly by using Amazon S3 URLs, they’re denied access\. The origin access identity has permission to access files in your Amazon S3 bucket, but viewers don’t\. For more information, see [Granting the OAI permission to read files in your Amazon S3 bucket](#private-content-granting-permissions-to-oai)\.

## Creating a CloudFront OAI and adding it to your distribution<a name="private-content-creating-oai"></a>

An AWS account can have [up to 100 CloudFront origin access identities \(OAIs\)](cloudfront-limits.md#limits-web-distributions)\. However, you can add an OAI to as many distributions as you want, so one OAI is usually sufficient\.

If you didn’t create an OAI and add it to your distribution when you created the distribution, you can create and add one now by using either the CloudFront console or the CloudFront API:
+ **To use the CloudFront console** – You can create an OAI and add it to your distribution at the same time\. For step\-by\-step instructions, see [Creating an OAI and adding it to your distribution](#private-content-creating-oai-console)\.
+ **To use the CloudFront API** – You create an OAI, and then you add it to your distribution\. For step\-by\-step instructions, see the following:
  + [Creating an OAI using the CloudFront API](#private-content-creating-oai-api)
  + [Adding an OAI to your distribution using the CloudFront API](#private-content-adding-oai-api)

### Creating an OAI and adding it to your distribution<a name="private-content-creating-oai-console"></a>

If you didn’t create an OAI when you created your distribution, do the following\.<a name="private-content-creating-oai-console-procedure"></a>

**To create a CloudFront OAI using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose the ID of a distribution that has an S3 origin\.

1. Choose the **Origins** tab\.

1. Select the Amazon S3 origin, and then choose **Edit**\.

1. For **S3 bucket access**, choose **Yes use OAI**\.

1. If you already have an OAI that you want to use, select the OAI from the drop\-down list\. If you already have an OAI, we recommend that you reuse it to simplify maintenance\.

   If you want to create an OAI, choose **Create new OAI**\. You can replace the autogenerated OAI name with a custom name if desired\.

1. If you want CloudFront to automatically update the Amazon S3 bucket policy to allow read access to the OAI, choose **Yes, update the bucket policy**\.
**Important**  
If you choose **Yes, update the bucket policy**, CloudFront updates bucket permissions to grant the specified OAI permission to read files in your bucket\. However, CloudFront does not remove existing permissions\. If users currently have permission to access the files in your bucket using Amazon S3 URLs, they will still have that permission after CloudFront updates your bucket permissions\. To view or remove existing bucket permissions, use a method provided by Amazon S3\.

   If you want to manually update permissions on your Amazon S3 bucket, choose **No, I will update the bucket policy**\. For more information, see [Granting the OAI permission to read files in your Amazon S3 bucket](#private-content-granting-permissions-to-oai)\.

1. At the bottom of the page, choose **Save changes**\.

1. If you have more than one Amazon S3 origin, repeat the steps to add an OAI for each one\.

### Creating an OAI using the CloudFront API<a name="private-content-creating-oai-api"></a>

If you already have an origin access identity and you want to reuse it instead of creating another one, skip to [Adding an OAI to your distribution using the CloudFront API](#private-content-adding-oai-api)\.

To create a CloudFront OAI using the CloudFront API, use the `CreateCloudFrontOriginAccessIdentity` API action\. The response includes an `Id` and an `S3CanonicalUserId` for the new OAI\. Make note of these values because you use them later in the process\. For more information, see [CreateCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateCloudFrontOriginAccessIdentity.html) in the *Amazon CloudFront API Reference*\.

### Adding an OAI to your distribution using the CloudFront API<a name="private-content-adding-oai-api"></a>

You can use the CloudFront API to add a CloudFront OAI to an existing distribution or to create a new distribution that includes an OAI\. In either case, include an `OriginAccessIdentity` element\. This element contains the value of the `Id` element that the `CreateCloudFrontOriginAccessIdentity` API action returned when you created the OAI\. You can add the `OriginAccessIdentity` element to one or more origins\.

See the following topics in the *Amazon CloudFront API Reference*:
+ **Create a new distribution** – [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)
+ **Update an existing distribution** – [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)

## Granting the OAI permission to read files in your Amazon S3 bucket<a name="private-content-granting-permissions-to-oai"></a>

When you create an OAI or add one to a distribution with the CloudFront console, you can automatically update the Amazon S3 bucket policy to give the OAI permission to access your bucket\. Alternatively, you can choose to manually create or update the bucket policy\. Whichever method you use, you should still review the permissions to make sure that:
+ Your CloudFront OAI can access files in the bucket on behalf of viewers who are requesting them through CloudFront\.
+ Viewers can’t use Amazon S3 URLs to access your files outside of CloudFront\.

**Important**  
If you configure CloudFront to accept and forward all of the HTTP methods that CloudFront supports, make sure you give your CloudFront OAI the desired permissions\. For example, if you configure CloudFront to accept and forward requests that use the `DELETE` method, configure your bucket policy to handle `DELETE` requests appropriately so viewers can delete only files that you want them to\.

### Using Amazon S3 bucket policies<a name="private-content-updating-s3-bucket-policies"></a>

You can give a CloudFront OAI access to files in an Amazon S3 bucket by creating or updating the bucket policy in the following ways:
+ Using the Amazon S3 bucket’s **Permissions** tab in the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.
+ Using [PutBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketPolicy.html) in the Amazon S3 API\.
+ Using the [CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home)\. When you add an OAI to your origin settings in the CloudFront console, you can choose **Yes, update the bucket policy** to tell CloudFront to update the bucket policy on your behalf\.

If you update the bucket policy manually, make sure that you:
+ Specify the correct OAI as the `Principal` in the policy\.
+ Give the OAI the permissions it needs to access objects on behalf of viewers\.

For more information, see the following sections\.

#### Specify an OAI as the `Principal` in a bucket policy<a name="private-content-updating-s3-bucket-policies-principal"></a>

To specify an OAI as the `Principal` in an Amazon S3 bucket policy, use the OAI’s Amazon Resource Name \(ARN\), which includes the OAI’s ID\. For example:

```
"Principal": {
    "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity EH1HDMB1FH2TC"
}
```

To use the preceding example, replace *EH1HDMB1FH2TC* with the OAI’s ID\. To find the OAI’s ID, see the [Origin access identities page](https://console.aws.amazon.com/cloudfront/v3/home#/oai) in the CloudFront console, or use [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

#### Give permissions to an OAI<a name="private-content-updating-s3-bucket-policies-permissions"></a>

To give the OAI the permissions to access objects in your Amazon S3 bucket, use keywords in the policy that relate to specific Amazon S3 API operations\. For example, the `s3:GetObject` permission allows the OAI to read objects in the bucket\. For more information, see the examples in the following section, or see [Amazon S3 actions](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service User Guide*\.

#### Amazon S3 bucket policy examples<a name="private-content-updating-s3-bucket-policies-examples"></a>

The following examples show Amazon S3 bucket policies that grant access to a CloudFront OAI\. To use these examples:
+ Replace *EH1HDMB1FH2TC* with the OAI’s ID\. To find the OAI’s ID, see the [Origin access identities page](https://console.aws.amazon.com/cloudfront/v3/home#/oai) in the CloudFront console, or use [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.
+ Replace *DOC\-EXAMPLE\-BUCKET* with the name of your Amazon S3 bucket\.

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
            "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
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
            "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
        }
    ]
}
```

### Using Amazon S3 object ACLs \(not recommended\)<a name="private-content-updating-s3-acls"></a>

**Important**  
We recommend [using Amazon S3 bucket policies](#private-content-updating-s3-bucket-policies) to give an OAI access to an S3 bucket\. You can use ACLs as described in this section, but we don’t recommend it\.  
Amazon S3 recommends setting [S3 Object Ownership](https://docs.aws.amazon.com/AmazonS3/latest/userguide/about-object-ownership.html) to **bucket owner enforced**, which means that ACLs are disabled for the bucket and the objects in it\. When you apply this setting for Object Ownership, you must use bucket policies to give access to the OAI \(see the previous section\)\.  
This following section is only for legacy use cases that require ACLs\.

You can give a CloudFront OAI access to files in an Amazon S3 bucket by creating or updating the file’s ACL in the following ways:
+ Using the Amazon S3 object’s **Permissions** tab in the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.
+ Using [PutObjectAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectAcl.html) in the Amazon S3 API\.

When you grant access to an OAI using an ACL, you must specify the OAI using its Amazon S3 canonical user ID\. This is the value of **Amazon S3 canonical user ID** on the [Origin access identities page](https://console.aws.amazon.com/cloudfront/v3/home#/oai) in the CloudFront console\. If you’re using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the OAI, or call [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

## Using an OAI in Amazon S3 regions that support only signature version 4 authentication<a name="private-content-origin-access-identity-signature-version-4"></a>

Newer Amazon S3 Regions require that you use Signature Version 4 for authenticated requests\. \(For the signature versions supported in each Amazon S3 Region, see [Amazon Simple Storage Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/s3.html) in the *AWS General Reference*\.\) If you’re using an origin access identity and if your bucket is in one of the Regions that requires Signature Version 4, note the following:
+ `DELETE`, `GET`, `HEAD`, `OPTIONS`, and `PATCH` requests are supported without qualifications\.
+ If you want to submit `PUT` requests to CloudFront to upload files to your Amazon S3 bucket, you must add an `x-amz-content-sha256` header to the request\.The header value must contain a SHA\-256 hash of the body of the request\. For more information, see the documentation about the `x-amz-content-sha256` header on the [Common Request Headers](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonRequestHeaders.html) page in the *Amazon Simple Storage Service API Reference*\.
+ `POST` requests are not supported\.