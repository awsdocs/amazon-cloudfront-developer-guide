# Overview of serving private content<a name="private-content-overview"></a>

You can control user access to your private content in two ways:
+ [Restrict access to files in CloudFront caches](#private-content-overview-edge-caches)\.
+ Restrict access to files in your origin by doing one of the following:
  + [Set up an origin access control \(OAC\) for your Amazon S3 bucket](private-content-restricting-access-to-s3.md)\.
  + [Configure custom headers for a private HTTP server \(a custom origin\)](#forward-custom-headers-restrict-access)\.

## Restricting access to files in CloudFront caches<a name="private-content-overview-edge-caches"></a>

You can configure CloudFront to require that users access your files using either *signed URLs* or *signed cookies*\. You then develop your application either to create and distribute signed URLs to authenticated users or to send `Set-Cookie` headers that set signed cookies for authenticated users\. \(To give a few users long\-term access to a small number of files, you can also create signed URLs manually\.\) 

When you create signed URLs or signed cookies to control access to your files, you can specify the following restrictions:
+ An ending date and time, after which the URL is no longer valid\. 
+ \(Optional\) The date and time that the URL becomes valid\.
+ \(Optional\) The IP address or range of addresses of the computers that can be used to access your content\. 

One part of a signed URL or a signed cookie is hashed and signed using the private key from a public–private key pair\. When someone uses a signed URL or signed cookie to access a file, CloudFront compares the signed and unsigned portions of the URL or cookie\. If they don't match, CloudFront doesn't serve the file\.

You must use RSA\-SHA1 for signing URLs or cookies\. CloudFront doesn't accept other algorithms\.

## Restricting access to files in Amazon S3 buckets<a name="private-content-overview-s3"></a>

You can optionally secure the content in your Amazon S3 bucket so that users can access it through the specified CloudFront distribution but cannot access it directly by using Amazon S3 URLs\. This prevents someone from bypassing CloudFront and using the Amazon S3 URL to get content that you want to restrict access to\. This step isn't required to use signed URLs, but we recommend it\.

To require that users access your content through CloudFront URLs, you do the following tasks:
+ Give a CloudFront *origin access control* permission to read the files in the S3 bucket\.
+ Create the origin access control and associate it with your CloudFront distribution\.
+ Remove permission for anyone else to use Amazon S3 URLs to read the files\.

For more information, see [Restricting access to an Amazon S3 origin](private-content-restricting-access-to-s3.md)\.

## Restricting access to files on custom origins<a name="forward-custom-headers-restrict-access"></a>

If you use a custom origin, you can optionally set up custom headers to restrict access\. For CloudFront to get your files from a custom origin, the files must be accessible by CloudFront using a standard HTTP \(or HTTPS\) request\. But by using custom headers, you can further restrict access to your content so that users can access it only through CloudFront, not directly\. This step isn't required to use signed URLs, but we recommend it\.

To require that users access content through CloudFront, change the following settings in your CloudFront distributions:

**Origin Custom Headers**  
Configure CloudFront to forward custom headers to your origin\. See [Configuring CloudFront to add custom headers to origin requests](add-origin-custom-headers.md#add-origin-custom-headers-configure)\.

**Viewer Protocol Policy**  
Configure your distribution to require viewers to use HTTPS to access CloudFront\. See [Viewer protocol policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\. 

**Origin Protocol Policy**  
Configure your distribution to require CloudFront to use the same protocol as viewers to forward requests to the origin\. See [Protocol \(custom origins only\)](distribution-web-values-specify.md#DownloadDistValuesOriginProtocolPolicy)\. 

After you've made these changes, update your application on your custom origin to only accept requests that include the custom headers that you’ve configured CloudFront to send\.

The combination of **Viewer Protocol Policy** and **Origin Protocol Policy** ensure that the custom headers are encrypted in transit\. However, we recommend that you periodically do the following to rotate the custom headers that CloudFront forwards to your origin:

1. Update your CloudFront distribution to begin forwarding a new header to your custom origin\.

1. Update your application to accept the new header as confirmation that the request is coming from CloudFront\.

1. When requests no longer include the header that you're replacing, update your application to no longer accept the old header as confirmation that the request is coming from CloudFront\.