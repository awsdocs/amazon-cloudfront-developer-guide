# Serving Private Content with Signed URLs and Signed Cookies<a name="PrivateContent"></a>

Many companies that distribute content over the internet want to restrict access to documents, business data, media streams, or content that is intended for selected users, for example, users who have paid a fee\. To securely serve this private content by using CloudFront, you can do the following:
+ Require that your users access your private content by using special CloudFront signed URLs or signed cookies\. 
+ Require that your users access your Amazon S3 content by using CloudFront URLs, not Amazon S3 URLs\. Requiring CloudFront URLs isn't necessary, but we recommend it to prevent users from bypassing the restrictions that you specify in signed URLs or signed cookies\.

**Important**  
If you use an Amazon S3 bucket configured as a website endpoint, you must set it up with CloudFront as a custom origin and you can't use the origin access identity feature described in this topic\. You can restrict access to content on a custom origin by using custom headers\. For more information, see [Using Custom Headers to Restrict Access to Your Content on a Custom Origin](forward-custom-headers.md#forward-custom-headers-restrict-access)\.

**Topics**
+ [Overview of Private Content](#private-content-overview)
+ [Using an HTTP Server for Private Content](#private-content-overview-choosing-origin)
+ [Task List: Serving Private Content](private-content-task-list.md)
+ [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)
+ [Choosing Between Signed URLs and Signed Cookies](private-content-choosing-signed-urls-cookies.md)
+ [Using Signed URLs](private-content-signed-urls.md)
+ [Using Signed Cookies](private-content-signed-cookies.md)
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)

## Overview of Private Content<a name="private-content-overview"></a>

You can control user access to your private content in two ways, as shown in the following illustration:

1. Restrict access to files in CloudFront edge caches

1. Restrict access to files in your Amazon S3 bucket \(unless you've configured it as a website endpoint\)

![\[Basic flow for access logs\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

### Restricting Access to Files in CloudFront Edge Caches<a name="private-content-overview-edge-caches"></a>

You can configure CloudFront to require that users access your files using either **signed URLs** or **signed cookies**\. You then develop your application either to create and distribute signed URLs to authenticated users or to send `Set-Cookie` headers that set signed cookies on the viewers for authenticated users\. \(To give a few users long\-term access to a limited number of files, you can also create signed URLs manually\.\) 

When you create signed URLs or signed cookies to control access to your files, you can specify the following restrictions:
+ An ending date and time, after which the URL is no longer valid\. 
+ \(Optional\) The date and time that the URL becomes valid\.
+ \(Optional\) The IP address or range of addresses of the computers that can be used to access your content\. 

One part of a signed URL or a signed cookie is hashed and signed using the private key from a public/private key pair\. When someone uses a signed URL or signed cookie to access a file, CloudFront compares the signed and unsigned portions of the URL or cookie\. If they don't match, CloudFront doesn't serve the file\.

### Restricting Access to Files in Amazon S3 Buckets<a name="private-content-overview-s3"></a>

You can optionally secure the content in your Amazon S3 bucket so users can access it through CloudFront but cannot access it directly by using Amazon S3 URLs\. This prevents anyone from bypassing CloudFront and using the Amazon S3 URL to get content that you want to restrict access to\. This step isn't required to use signed URLs, but we recommend it\. Be aware that this option is only available if you have not set up your Amazon S3 bucket as as website endpoint\.

To require that users access your content through CloudFront URLs, you perform the following tasks:
+ Create a special CloudFront user called an **origin access identity**\.
+ Give the origin access identity permission to read the files in your bucket\.
+ Remove permission for anyone else to use Amazon S3 URLs to read the files\.

For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

## Using an HTTP Server for Private Content<a name="private-content-overview-choosing-origin"></a>

You can use signed URLs or signed cookies for any CloudFront distribution, regardless of whether the origin is an Amazon S3 bucket or an HTTP server\. However, for CloudFront to get your files from an HTTP server, the files must remain publicly accessible\. When the files are publicly accessible, anyone who has the URL for a file on your HTTP server can access the file without logging in or paying for your content\. If you use signed URLs or signed cookies and your origin is an HTTP server, do not give the URLs for the files on your HTTP server to your customers or to others outside your organization\.