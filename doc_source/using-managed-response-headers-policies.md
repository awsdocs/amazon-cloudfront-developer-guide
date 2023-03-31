# Using the managed response headers policies<a name="using-managed-response-headers-policies"></a>

With a CloudFront response headers policy, you can specify the HTTP headers that Amazon CloudFront removes or adds in responses that it sends to viewers\. For more information about response headers policies and reasons to use them, see [Adding or removing HTTP headers in CloudFront responses](modifying-response-headers.md)\.

CloudFront provides managed response headers policies that you can attach to cache behaviors in your CloudFront distributions\. With a managed response headers policy, you don't need to write or maintain your own policy\. The managed policies contain sets of HTTP response headers for common use cases\.

**Topics**
+ [Attaching a managed response headers policy](#attaching-managed-response-headers-policies)
+ [Available managed response headers policies](#available-managed-response-headers-policies)

## Attaching a managed response headers policy<a name="attaching-managed-response-headers-policies"></a>

To use a managed response headers policy, you attach it to a cache behavior in your distribution\. The process is the same as when you create a custom response headers policy\. However, instead of creating a new policy, you attach one of the managed policies\. You attach the policy either by name \(with the console\) or by ID \(with AWS CloudFormation, the AWS CLI, or the AWS SDKs\)\. The names and IDs are listed in the following section\.

For more information, see [Creating response headers policies](creating-response-headers-policies.md)\.

## Available managed response headers policies<a name="available-managed-response-headers-policies"></a>

The following topics describe the managed response headers policies that you can use\.

**Topics**
+ [CORS\-and\-SecurityHeadersPolicy](#managed-response-headers-policies-cors-security)
+ [CORS\-With\-Preflight](#managed-response-headers-policies-cors-preflight)
+ [CORS\-with\-preflight\-and\-SecurityHeadersPolicy](#managed-response-headers-policies-cors-preflight-security)
+ [SecurityHeadersPolicy](#managed-response-headers-policies-security)
+ [SimpleCORS](#managed-response-headers-policies-cors)

### CORS\-and\-SecurityHeadersPolicy<a name="managed-response-headers-policies-cors-security"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders/e61eb60c-9c35-4d20-a928-2b84e02af89c)

Use this managed policy to allow simple CORS requests from any origin\. This policy also adds a set of security headers to all responses that CloudFront sends to viewers\. This policy combines the [SimpleCORS](#managed-response-headers-policies-cors) and [SecurityHeadersPolicy](#managed-response-headers-policies-security) policies into one\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`e61eb60c-9c35-4d20-a928-2b84e02af89c`


**Policy settings**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-response-headers-policies.html)

### CORS\-With\-Preflight<a name="managed-response-headers-policies-cors-preflight"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders/5cc3b908-e619-4b99-88e5-2cf7f45965bd)

Use this managed policy to allow CORS requests from any origin, including preflight requests\. For preflight requests \(using the HTTP `OPTIONS` method\), CloudFront adds all three of the following headers to the response\. For simple CORS requests, CloudFront adds only the `Access-Control-Allow-Origin` header\.

If the response that CloudFront receives from the origin includes any of these headers, CloudFront uses the received header \(and its value\) in its response to the viewer\. CloudFront doesn't use the header in this policy\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`5cc3b908-e619-4b99-88e5-2cf7f45965bd`


**Policy settings**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-response-headers-policies.html)

### CORS\-with\-preflight\-and\-SecurityHeadersPolicy<a name="managed-response-headers-policies-cors-preflight-security"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders/eaab4381-ed33-4a86-88ca-d9558dc6cd63)

Use this managed policy to allow CORS requests from any origin\. This includes preflight requests\. This policy also adds a set of security headers to all responses that CloudFront sends to viewers\. This policy combines the [CORS\-With\-Preflight](#managed-response-headers-policies-cors-preflight) and [SecurityHeadersPolicy](#managed-response-headers-policies-security) policies into one\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`eaab4381-ed33-4a86-88ca-d9558dc6cd63`


**Policy settings**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-response-headers-policies.html)

### SecurityHeadersPolicy<a name="managed-response-headers-policies-security"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders/67f7725c-6f97-4210-82d7-5512b31e9d03)

Use this managed policy to add a set of security headers to all responses that CloudFront sends to viewers\. For more information about these security headers, see [Mozilla's web security guidelines](https://infosec.mozilla.org/guidelines/web_security)\.

With this response headers policy, CloudFront adds `X-Content-Type-Options: nosniff` to all responses\. This is the case when the response that CloudFront received from the origin included this header and when it didn't\. For all other headers in this policy, if the response that CloudFront receives from the origin includes the header, CloudFront uses the received header \(and its value\) in its response to the viewer\. It doesn't use the header in this policy\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`67f7725c-6f97-4210-82d7-5512b31e9d03`


**Policy settings**  
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-response-headers-policies.html)

### SimpleCORS<a name="managed-response-headers-policies-cors"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders/60669652-455b-4ae9-85a4-c4c02393f86c)

Use this managed policy to allow [simple CORS requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests) from any origin\. With this policy, CloudFront adds the header `Access-Control-Allow-Origin: *` to all responses for simple CORS requests\.

If the response that CloudFront receives from the origin includes the `Access-Control-Allow-Origin` header, CloudFront uses that header \(and its value\) in its response to the viewer\. CloudFront doesn't use the header in this policy\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`60669652-455b-4ae9-85a4-c4c02393f86c`


**Policy settings**  

|  | Header name | Header value | Override origin? | 
| --- | --- | --- | --- | 
| CORS headers: | Access\-Control\-Allow\-Origin | \* | No | 