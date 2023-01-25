# Add security headers to the response<a name="example-function-add-security-headers"></a>

**Note**  
Consider using [CloudFront Response Headers Policies](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-cloudfront-introduces-response-headers-policies/) instead of CloudFront Functions to configure CORS, security, and custom HTTP response headers\.

The following example function adds several common security\-related HTTP headers to the response\. For more information, see the following pages on the MDN Web Docs website:
+ [Strict\-Transport\-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security)
+ [Content\-Security\-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)
+ [X\-Content\-Type\-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options)
+ [X\-Frame\-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options)
+ [X\-XSS\-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection)

This is a viewer response function\.

[See this example on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions/tree/main/add-security-headers)\.

```
function handler(event) {
    var response = event.response;
    var headers = response.headers;

    // Set HTTP security headers
    // Since JavaScript doesn't allow for hyphens in variable names, we use the dict["key"] notation 
    headers['strict-transport-security'] = { value: 'max-age=63072000; includeSubdomains; preload'}; 
    headers['content-security-policy'] = { value: "default-src 'none'; img-src 'self'; script-src 'self'; style-src 'self'; object-src 'none'"}; 
    headers['x-content-type-options'] = { value: 'nosniff'}; 
    headers['x-frame-options'] = {value: 'DENY'}; 
    headers['x-xss-protection'] = {value: '1; mode=block'}; 

    // Return the response to viewers 
    return response;
}
```
