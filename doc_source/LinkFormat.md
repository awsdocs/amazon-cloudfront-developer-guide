# Customizing the URL format for files in CloudFront<a name="LinkFormat"></a>

After you set up your origin with the objects \(content\) that you want CloudFront to serve to your viewers, you must use the correct URLs to reference those objects in your website or application code so that CloudFront can serve it\.

The domain name that you use in the URLs for objects on your web pages or in your web application can be either of the following:
+ The domain name, such as `d111111abcdef8.cloudfront.net`, that CloudFront automatically assigns when you create a distribution
+ Your own domain name, such as `example.com`

For example, you might use one of the following URLs to return the file `image.jpg`:

`https://d111111abcdef8.cloudfront.net/images/image.jpg`

`https://example.com/images/image.jpg`

You use the same URL format whether you store the content in Amazon S3 buckets or at a custom origin, like one of your own web servers\.

**Note**  
The URL format depends in part on the value that you specify for **Origin Path** in your distribution\. This value gives CloudFront a top directory path for your objects\. For more information about setting the origin path when you create a distribution, see [Origin path](distribution-web-values-specify.md#DownloadDistValuesOriginPath)\.

For more information about URL formats, see the following sections\.

## Using your own domain name \(example\.com\)<a name="LinkFormat_OwnDomain"></a>

Instead of using the default domain name that CloudFront assigns for you when you create a distribution, you can [add an alternate domain name](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesCNAME) that's easier to work with, like `example.com`\. By setting up your own domain name with CloudFront, you can use a URL like this for objects in your distribution:

`https://example.com/images/image.jpg`

If you plan to use HTTPS between viewers and CloudFront, see [Using alternate domain names and HTTPS](using-https-alternate-domain-names.md)\.

## Using a trailing slash \(/\) in URLs<a name="LinkFormat_TrailingSlash"></a>

When you specify URLs for directories in your CloudFront distribution, choose either to always use a trailing slash or to never use a trailing slash\. For example, choose only one of the following formats for all of your URLs:

`https://d111111abcdef8.cloudfront.net/images/`

`https://d111111abcdef8.cloudfront.net/images`

**Why does it matter?**

Both formats work to link to CloudFront objects, but being consistent can help prevent issues when you want to invalidate a directory later\. CloudFront stores URLs exactly as they are defined, including trailing slashes\. So if your format is inconsistent, you'll need to invalidate directory URLs with and without the slash, to ensure that CloudFront removes the directory\. 

It’s inconvenient to have to invalidate both URL formats, and it can lead to additional costs\. That’s because if you must double up invalidations to cover both types of URLs, you might exceed the maximum number of free invalidations allowed for the month\. And if that happens, you'll have to pay for all the invalidations, even if only one format for each directory URL exists in CloudFront\.

## Creating signed URLs for restricted content<a name="LinkFormat_SignedURLs"></a>

If you have content that you want to restrict access to, you can create signed URLs\. For example, if you want to distribute your content only to users who have authenticated, you can create URLs that are valid only for a specified time period or that are available only from a specified IP address\. For more information, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.