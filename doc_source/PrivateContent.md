# Serving Private Content through CloudFront<a name="PrivateContent"></a>

Many companies that distribute content via the internet want to restrict access to documents, business data, media streams, or content that is intended for selected users, for example, users who have paid a fee\. To securely serve this private content using CloudFront, you can do the following:

+ Require that your users access your private content by using special CloudFront signed URLs or signed cookies\. 

+ Require that your users access your Amazon S3 content using CloudFront URLs, not Amazon S3 URLs\. Requiring CloudFront URLs isn't required, but we recommend it to prevent users from bypassing the restrictions that you specify in signed URLs or signed cookies\.


+ [Overview of Private Content](#private-content-overview)
+ [Using an HTTP Server for Private Content](#private-content-overview-choosing-origin)
+ [Task List: Serving Private Content](private-content-task-list.md)
+ [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)
+ [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)
+ [Choosing Between Signed URLs and Signed Cookies](private-content-choosing-signed-urls-cookies.md)
+ [Using Signed URLs](private-content-signed-urls.md)
+ [Using Signed Cookies](private-content-signed-cookies.md)
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)

## Overview of Private Content<a name="private-content-overview"></a>

You can control user access to your private content in two ways, as shown in the following illustration:

1. Restrict access to objects in CloudFront edge caches

1. Restrict access to objects in your Amazon S3 bucket

![\[Basic flow for access logs\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

### Restricting Access to Objects in CloudFront Edge Caches<a name="private-content-overview-edge-caches"></a>

You can configure CloudFront to require that users access your objects using either **signed URLs** or **signed cookies**\. You then develop your application either to create and distribute signed URLs to authenticated users or to send `Set-Cookie` headers that set signed cookies on the viewers for authenticated users\. \(To give a few users long\-term access to a limited number of objects, you can also create signed URLs manually\.\) 

When you create signed URLs or signed cookies to control access to your objects, you can specify the following restrictions:

+ An ending date and time, after which the URL is no longer valid\. 

+ \(Optional\) The date and time that the URL becomes valid\.

+ \(Optional\) The IP address or range of addresses of the computers that can be used to access your content\. 

One part of a signed URL or a signed cookie is hashed and signed using the private key from a public/private key pair\. When someone uses a signed URL or signed cookie to access an object, CloudFront compares the signed and unsigned portions of the URL or cookie\. If they don't match, CloudFront doesn't serve the object\.

### Restricting Access to Objects in Amazon S3 Buckets<a name="private-content-overview-s3"></a>

You can optionally secure the content in your Amazon S3 bucket so users can access it through CloudFront but cannot access it directly by using Amazon S3 URLs\. This prevents anyone from bypassing CloudFront and using the Amazon S3 URL to get content that you want to restrict access to\. This step isn't required to use signed URLs, but we recommend it\.

To require that users access your content through CloudFront URLs, you perform the following tasks:

+ Create a special CloudFront user called an **origin access identity**\.

+ Give the origin access identity permission to read the objects in your bucket\.

+ Remove permission for anyone else to use Amazon S3 URLs to read the objects\.

## Using an HTTP Server for Private Content<a name="private-content-overview-choosing-origin"></a>

You can use signed URLs or signed cookies for any CloudFront distribution, regardless of whether the origin is an Amazon S3 bucket or an HTTP server\. However, for CloudFront to get your objects from an HTTP server, the objects must remain publicly accessible\. When the objects are publicly accessible, anyone who has the URL for an object on your HTTP server can access the object without logging in or paying for your content\. If you use signed URLs or signed cookies and your origin is an HTTP server, do not give the URLs for the objects on your HTTP server to your customers or to others outside your organization\.