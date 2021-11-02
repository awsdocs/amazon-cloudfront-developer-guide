# Adding HTTP headers to CloudFront responses<a name="adding-response-headers"></a>

You can configure CloudFront to add one or more HTTP headers to the responses that it sends to viewers \(web browsers or other clients\), without making any changes to the origin or writing any code\. For example, you can:
+ Add a `Cache-Control` header to control browser caching\.
+ Add an `Access-Control-Allow-Origin` header to enable cross\-origin resource sharing \(CORS\)\. You can also add other CORS headers\.
+ Add a set of common security headers, such as `Strict-Transport-Security`, `Content-Security-Policy`, `X-Content-Type-Options`, `X-Frame-Options`, and more\.

To specify the headers that CloudFront adds to HTTP responses, you use a *response headers policy*\. CloudFront adds the headers regardless of whether it serves the object from the cache or has to retrieve the object from the origin\. If the origin response includes one or more of the headers that’s in a response headers policy, the policy can specify whether CloudFront uses the header it received from the origin or overwrites it with the one in the policy\.

CloudFront provides predefined response headers policies, known as *managed policies*, for common use cases\. You can [use these managed policies](using-managed-response-headers-policies.md) or you can create your own policies that are specific to your needs\. You can attach a single response headers policy to multiple cache behaviors in multiple distributions in your AWS account\.

For more information, see the following topics\.

**Topics**
+ [Creating response headers policies](creating-response-headers-policies.md)
+ [Using the managed response headers policies](using-managed-response-headers-policies.md)
+ [Understanding response headers policies](understanding-response-headers-policies.md)