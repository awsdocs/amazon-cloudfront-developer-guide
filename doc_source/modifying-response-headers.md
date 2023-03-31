# Adding or removing HTTP headers in CloudFront responses<a name="modifying-response-headers"></a>

You can configure CloudFront to modify the HTTP headers in the responses that it sends to viewers\. CloudFront can remove headers that it received from the origin, or add headers to the response, before sending the response to viewers\. Making these changes doesn't require writing code or changing the origin\.

For example, you can remove headers such as `X-Powered-By` and `Vary` so that CloudFront doesn't include these headers in the responses that it sends to viewers\. Or, you can add HTTP headers such as the following:
+ A `Cache-Control` header to control browser caching\.
+ An `Access-Control-Allow-Origin` header to enable cross\-origin resource sharing \(CORS\)\. You can also add other CORS headers\.
+ A set of common security headers, such as `Strict-Transport-Security`, `Content-Security-Policy`, and `X-Frame-Options`\.
+ A `Server-Timing` header to see information that's related to the performance and routing of both the request and response through CloudFront\.

To specify the headers that CloudFront adds or removes in HTTP responses, you use a *response headers policy*\. You attach a response headers policy to one more *cache behaviors*, and CloudFront modifies the headers in the responses that it sends to requests that match the cache behavior\. CloudFront modifies the headers in the responses that it serves from the cache and the ones that it forwards from the origin\. If the origin response includes one or more of the headers that are added in a response headers policy, the policy can specify if CloudFront uses the header it received from the origin or overwrites that header with the one in the response headers policy\.

CloudFront provides predefined response headers policies, known as *managed policies*, for common use cases\. You can [use these managed policies](using-managed-response-headers-policies.md) or create your own policies\. You can attach a single response headers policy to multiple cache behaviors in multiple distributions in your AWS account\.

For more information, see the following topics\.

**Topics**
+ [Creating response headers policies](creating-response-headers-policies.md)
+ [Using the managed response headers policies](using-managed-response-headers-policies.md)
+ [Understanding response headers policies](understanding-response-headers-policies.md)