# How CloudFront Processes HTTP 3xx Status Codes from Your Origin<a name="http-3xx-status-codes"></a>

When CloudFront requests an object from your Amazon S3 bucket or custom origin server, your origin sometimes returns an HTTP 3xx status code\. This typically indicates one of the following:
+ The object’s URL has changed \(for example, status codes 301, 302, 307, or 308\)
+ The object hasn’t changed since the last time CloudFront requested it \(status code 304\)

CloudFront caches 3xx responses according to the settings in your CloudFront distribution and the headers in the response\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

If your origin returns a redirect status code \(for example, 301 or 307\), CloudFront doesn’t follow the redirect\. CloudFront passes along the 301 or 307 response to the viewer, who can follow the redirect by sending a new request\.