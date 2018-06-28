# Adding and Accessing Content That CloudFront Distributes<a name="AddingObjects"></a>

When you want CloudFront to distribute objects \(content\), you add files to one of the origins that you specified for the distribution, and you expose a CloudFront link to the files\. A CloudFront edge location doesn't fetch the new objects from an origin until the edge location receives viewer requests for the objects\. For more information, see [How CloudFront Delivers Content](HowCloudFrontWorks.md)\. 

When you add an object that you want CloudFront to distribute, ensure that you add it to one of the Amazon S3 buckets specified in your distribution or, for a custom origin, to a directory in the specified domain\. In addition, confirm that the path pattern in the applicable cache behavior sends requests to the correct origin\. For example, suppose the path pattern for a cache behavior is `*.html`\. If no other cache behaviors are configured to forward requests to that origin, CloudFront will never distribute \.jpg files that you upload to the origin\.

CloudFront servers don't determine the MIME type for the objects they serve\. When you upload an object to your origin, you should set the `Content-Type` header field for the object\.

**Topics**