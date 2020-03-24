# Amazon CloudFront Related Information<a name="Resources"></a>

Although fairly simple to use, CloudFront is rich in functionality\. The information and resources listed here can help you learn more about CloudFront\.

**Topics**
+ [Additional Amazon CloudFront Documentation](#resources-additional-cloudfront-documentation)
+ [Getting Support](#resources-cloudfront-support)
+ [CloudFront Developer Tools and SDKs](#resources-developer-tools)
+ [Tips from the Amazon Web Services Blog](#resources-aws-blog-tips)
+ [Invalidating Objects](#resources-invalidation-tools)
+ [Tools and Code Examples for Configuring Private Content](#resources-distributing-private-content)

## Additional Amazon CloudFront Documentation<a name="resources-additional-cloudfront-documentation"></a>

The following related resources can help you as you work with this service\.
+ [Amazon CloudFront API Reference](https://docs.aws.amazon.com/cloudfront/latest/APIReference/) – Gives complete descriptions of the API actions, parameters, and data types, and a list of errors that the service returns\.
+ [CloudFront What's New](https://aws.amazon.com/cloudfront/whats-new/) – Announcements of new CloudFront features and recently added edge locations\. 
+ [Technical documentation for the Amazon Simple Storage Service \(S3\)](https://docs.aws.amazon.com/s3/) – A detailed discussion of the Amazon S3 service, including the basics of getting started, an overview of the service, a programming reference, and an API reference\. 
+ [Amazon CloudFront product information](http://aws.amazon.com/cloudfront/) – The primary web page for information about CloudFront, including features and pricing information\.

## Getting Support<a name="resources-cloudfront-support"></a>

Support for CloudFront is available in a number of forms\.
+ [Discussion forums](https://forums.aws.amazon.com/forum.jspa?forumID=46) – A community\-based forum for developers to discuss technical questions related to CloudFront\.
+ [AWS Support Center](https://console.aws.amazon.com/support/home) – This site brings together information about your recent support cases and results from AWS Trusted Advisor and health checks, as well as providing links to discussion forums, technical FAQs, the service health dashboard, and information about AWS support plans\.
+ [AWS Premium Support Information](https://aws.amazon.com/premiumsupport/) – The primary web page for information about AWS Premium Support, a one\-on\-one, fast\-response support channel to help you build and run applications on AWS Infrastructure Services\.
+ [Contact Us](http://aws.amazon.com/contact-us/) – Links for inquiring about your billing or account\. For technical questions, use the discussion forums or support links above\.

## CloudFront Developer Tools and SDKs<a name="resources-developer-tools"></a>

See the [Developer Tools](https://aws.amazon.com/developertools/) page for links to developer resources that provide documentation, code samples, release notes, and other information to help you build innovative applications with AWS\. 

In addition, Amazon Web Services provides software development kits for accessing CloudFront programmatically\. The SDK libraries automate a number of common tasks, including cryptographically signing your service requests, retrying requests, and handling error responses\.
+ [AWS SDK for Java](https://aws.amazon.com/sdkforjava/) – Setup and other documentation
+ [AWS SDK for \.NET](https://aws.amazon.com/sdkfornet/) – Setup and other documentation
+ [AWS SDK for PHP](https://aws.amazon.com/sdkforphp/) – Setup and other documentation
+ [AWS SDK for Ruby](https://aws.amazon.com/sdkforruby/) – Setup and other documentation

## Tips from the Amazon Web Services Blog<a name="resources-aws-blog-tips"></a>

The AWS Blog has a number of posts to help you use CloudFront\. For example, see the following blog posts about using Drupal and WordPress with CloudFront\.
+ [ Accelerating your Drupal Content with Amazon CloudFront](https://aws.amazon.com/blogs/networking-and-content-delivery/accelerating-your-drupal-content-with-amazon-cloudfront/)
+ [ How to accelerate your WordPress site with Amazon CloudFront](https://aws.amazon.com/blogs/startups/how-to-accelerate-your-wordpress-site-with-amazon-cloudfront/)

## Invalidating Objects<a name="resources-invalidation-tools"></a>

In addition to the invalidation methods provided by CloudFront, you can use the following third\-party tools to invalidate objects\.

**Note**  
These tools were developed by third\-party vendors who are not associated with Amazon Web Services\. For information on how to use these tools, please refer to the vendor's documentation or contact the vendor\. 
+ CloudBerry Explorer – [http://cloudberrylab\.com](http://cloudberrylab.com)
+ Ylastic – [http://ylastic\.com](http://ylastic.com)
+ Cyberduck – [http://cyberduck\.io](http://cyberduck.io)
+ CloudFront Invalidator – [https://github\.com/swook/jQuery\-CloudFront\-Invalidator](https://github.com/swook/jQuery-CloudFront-Invalidator)
+ CDN Planet CloudFront Purge Tool – [http://www\.cdnplanet\.com/tools/cloudfront\-purge\-tool/](http://www.cdnplanet.com/tools/cloudfront-purge-tool/)

You can also search for code samples on Github, [https://github\.com](https://github.com)\. Search for the phrase *CloudFront invalidation*\.

## Tools and Code Examples for Configuring Private Content<a name="resources-distributing-private-content"></a>

In addition to the methods provided by CloudFront, the following third\-party tools provide web forms for configuring your distribution for private content\. Some of the tools also provide web forms for creating signed URLs\.
+ **CloudBerry** – Supports configuring a distribution for private content and supports creating signed URLs\.

  For information about using [CloudBerry](http://www.cloudberrylab.com/) for CloudFront private content, go to [How to Configure Private Content for CloudFront Streaming with CloudBerry](http://blog.cloudberrylab.com/2010/03/how-to-configure-private-content-for.html)\.

  For information on setting a default root object, see [How to set CloudFront Default Object with CloudBerry S3 Explorer](http://blog.cloudberrylab.com/2010/08/how-to-set-cloudfront-default-object.html)\.

For more information about private content, see the blog post [New Amazon CloudFront Feature: Private Content](https://aws.amazon.com/blogs/aws/new-amazon-cloudfront-feature-private-content/) on the AWS blog\.

For an example of how to use signed cookies with your custom domain name and still use the SSL/TLS certificate for the cloudfront\.net domain, see the Space Vatican blog post [Using CloudFront Signed Cookies](https://www.spacevatican.org/2015/5/1/using-cloudfront-signed-cookies/)\. This allows viewers \(clients\) that don’t support server name indication \(SNI\) to use an alternate domain name with HTTPS without incurring the cost of dedicated IP addresses, as documented in [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.