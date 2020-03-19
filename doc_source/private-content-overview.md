# Overview of Serving Private Content<a name="private-content-overview"></a>

You can control user access to your private content in two ways, as shown in the following illustration:

1. Restrict access to files in CloudFront edge caches

1. Restrict access to files in your origin by doing one of the following:
   + Set up an origin access identity \(OAI\) for your Amazon S3 bucket
   + Configure custom headers for a private HTTP server \(a custom origin\)

![\[The two ways to restrict file access in CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/PrivateContent_TwoPartsNew.png)

## Restricting Access to Files in CloudFront Edge Caches<a name="private-content-overview-edge-caches"></a>

You can configure CloudFront to require that users access your files using either *signed URLs* or *signed cookies*\. You then develop your application either to create and distribute signed URLs to authenticated users or to send `Set-Cookie` headers that set signed cookies on the viewers for authenticated users\. \(To give a few users long\-term access to a small number of files, you can also create signed URLs manually\.\) 

When you create signed URLs or signed cookies to control access to your files, you can specify the following restrictions:
+ An ending date and time, after which the URL is no longer valid\. 
+ \(Optional\) The date and time that the URL becomes valid\.
+ \(Optional\) The IP address or range of addresses of the computers that can be used to access your content\. 

One part of a signed URL or a signed cookie is hashed and signed using the private key from a public/private key pair\. When someone uses a signed URL or signed cookie to access a file, CloudFront compares the signed and unsigned portions of the URL or cookie\. If they don't match, CloudFront doesn't serve the file\.

You must use RSA\-SHA1 for signing URLs or cookies\. CloudFront doesn't accept other algorithms\.

## Restricting Access to Files in Amazon S3 Buckets<a name="private-content-overview-s3"></a>

You can optionally secure the content in your Amazon S3 bucket so that users can access it through CloudFront but cannot access it directly by using Amazon S3 URLs\. This prevents someone from bypassing CloudFront and using the Amazon S3 URL to get content that you want to restrict access to\. This step isn't required to use signed URLs, but we recommend it\.

To require that users access your content through CloudFront URLs, you do the following tasks:
+ Create a special CloudFront user called an *origin access identity* and associate it with your CloudFront distribution\.
+ Give the origin access identity permission to read the files in your bucket\.
+ Remove permission for anyone else to use Amazon S3 URLs to read the files\.

For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

## Restricting Access to Files on Custom Origins<a name="forward-custom-headers-restrict-access"></a>

If you use a custom origin, you can optionally set up custom headers to restrict access\. For CloudFront to get your files from a custom origin, the files must be publicly accessible\. But by using custom headers, you can restrict access to your content so that users can access it only through CloudFront, not directly\. This step isn't required to use signed URLs, but we recommend it\.

To require that users access content through CloudFront, change the following settings in your CloudFront distributions:

**Origin Custom Headers**  
Configure CloudFront to forward custom headers to your origin\. See [Configuring CloudFront to Add Custom Headers to Origin Requests](add-origin-custom-headers.md#add-origin-custom-headers-configure)\.

**Viewer Protocol Policy**  
Configure your distribution to require viewers to use HTTPS to access CloudFront\. See [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\. 

**Origin Protocol Policy**  
Configure your distribution to require CloudFront to use the same protocol as viewers to forward requests to the origin\. See [Origin Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesOriginProtocolPolicy)\. 

After you've made these changes, update your application on your custom origin to only accept requests that include these headers\.

The combination of **Viewer Protocol Policy** and **Origin Protocol Policy** ensure that your custom headers are encrypted between the viewer and your origin\. However, we recommend that you periodically do the following to rotate the custom headers that CloudFront forwards to your origin:

1. Update your CloudFront distribution to begin forwarding a new header to your custom origin\.

1. Update your application to accept the new header as confirmation that the request is coming from CloudFront\.

1. When viewer requests no longer include the header that you're replacing, update your application to no longer accept the old header as confirmation that the request is coming from CloudFront\.