# Adding and Accessing Content That CloudFront Distributes<a name="AddingObjects"></a>

When you want CloudFront to distribute content \(objects\), you add files to one of the origins that you specified for the distribution, and you expose a CloudFront link to the files\. A CloudFront edge location doesn't fetch the new files from an origin until the edge location receives viewer requests for them\. For more information, see [How CloudFront delivers content](HowCloudFrontWorks.md)\. 

When you add a file that you want CloudFront to distribute, make sure that you add it to one of the Amazon S3 buckets specified in your distribution or, for a custom origin, to a directory in the specified domain\. In addition, confirm that the path pattern in the applicable cache behavior sends requests to the correct origin\. 

For example, suppose the path pattern for a cache behavior is `*.html`\. If you don't have any other cache behaviors configured to forward requests to that origin, CloudFront will only forward `*.html` files\. In this scenario, for example, CloudFront will never distribute \.jpg files that you upload to the origin, because you haven't created a cache behavior that includes \.jpg files\.

CloudFront servers don't determine the MIME type for the objects that they serve\. When you upload a file to your origin, we recommend that you set the `Content-Type` header field for it\.