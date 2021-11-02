# Understanding response headers policies<a name="understanding-response-headers-policies"></a>

You can use a response headers policy to specify the HTTP headers that Amazon CloudFront adds to responses that it sends to viewers \(web browsers or clients\)\. For more information about response headers policies and why to use them, see [Adding HTTP headers to CloudFront responses](adding-response-headers.md)\.

The following topics explain the settings in a response headers policy\. The settings are grouped into categories, which are represented in the following topics\.

**Topics**
+ [Policy details \(metadata\)](#understanding-response-headers-policies-details)
+ [CORS headers](#understanding-response-headers-policies-cors)
+ [Security headers](#understanding-response-headers-policies-security)
+ [Custom headers](#understanding-response-headers-policies-custom)

## Policy details \(metadata\)<a name="understanding-response-headers-policies-details"></a>

The policy details settings contain metadata about a response headers policy\.
+ **Name** – A name to identify the response headers policy\. In the console, you use the name to attach the policy to a cache behavior\.
+ **Description** \(optional\) – A comment to describe the response headers policy\. This is optional, but it can help you identify the purpose of the policy\.

## CORS headers<a name="understanding-response-headers-policies-cors"></a>

The cross\-origin resource sharing \(CORS\) settings allow you to add and configure CORS headers in a response headers policy\.

This list focuses on how to specify setting and valid values in a response headers policy\. For more information about each of these headers and how they’re used for real\-world CORS requests and responses, see [cross\-origin resource sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) in the MDN Web Docs and the [CORS protocol spec](https://fetch.spec.whatwg.org/#http-cors-protocol)\.
+ `Access-Control-Allow-Credentials` – This is a Boolean setting \(`true` or `false`\) that determines whether or not CloudFront adds the `Access-Control-Allow-Credentials` header in responses to CORS requests\. When this setting is `true`, CloudFront adds the `Access-Control-Allow-Credentials: true` header in responses to CORS requests\. Otherwise CloudFront doesn’t add this header to responses\.
+ `Access-Control-Allow-Headers` – Specifies the header names that CloudFront uses as values for the `Access-Control-Allow-Headers` header in responses to CORS preflight requests\. Valid values for this setting include HTTP header names, or the wildcard character \(`*`\)\.
+ `Access-Control-Allow-Methods` – Specifies the HTTP methods that CloudFront uses as values for the `Access-Control-Allow-Methods` header in responses to CORS preflight requests\. Valid values include `GET`, `DELETE`, `HEAD`, `OPTIONS`, `PATCH`, `POST`, `PUT`, or `ALL`\. `ALL` is a special value that includes all of the listed HTTP methods\.
+ `Access-Control-Allow-Origin` – Specifies the values that CloudFront can use in the `Access-Control-Allow-Origin` response header\. Valid values for this setting include domain names such as `example.com`, or the wildcard character \(`*`\) which means all domains \(allow all origins\)\.
+ `Access-Control-Expose-Headers` – Specifies the header names that CloudFront uses as values for the `Access-Control-Expose-Headers` header in responses to CORS requests\. Valid values for this setting include HTTP header names, or the wildcard character \(`*`\)\.
+ `Access-Control-Max-Age` – A number of seconds, which CloudFront uses as the value for the `Access-Control-Max-Age` header in responses to CORS preflight requests\.
+ **Origin override** – This is a Boolean setting \(`true` or `false`\) that determines how CloudFront behaves when the response from the origin contains one of the CORS headers that’s also in the policy\.

  When this setting is `true` and the origin response contains a CORS header that’s also in the policy, CloudFront adds the CORS header in the policy to the response it sends to the viewer, ignoring the header it received from the origin\.

  When this setting is `false` and the origin response contains a CORS header that’s also in the policy, CloudFront includes the CORS header it received from the origin in the response it sends to the viewer\.

  When the origin response doesn’t contain a CORS header that’s in the policy, CloudFront adds the CORS header in the policy to the response it sends to the viewer, regardless of whether this setting is `true` or `false`\.

## Security headers<a name="understanding-response-headers-policies-security"></a>

The security headers settings allow you to add and configure several security\-related HTTP response headers in a response headers policy\.

This list focuses on how to specify setting and valid values in a response headers policy\. For more information about each of these headers and how they’re used in real\-world HTTP responses, see the links to the MDN Web Docs\.
+ `Content-Security-Policy` – Specifies the content security policy directives that CloudFront uses as values for the `Content-Security-Policy` response header\.

  For more information about this header and valid policy directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) in the MDN Web Docs\.
+ `Referrer-Policy` – Specifies the referrer policy directive that CloudFront uses as the value for the `Referrer-Policy` response header\. Valid values for this setting are `no-referrer`, `no-referrer-when-downgrade`, `origin`, `origin-when-cross-origin`, `same-origin`, `strict-origin`, `strict-origin-when-cross-origin`, or `unsafe-url`\.

  For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy) in the MDN Web Docs\.
+ `Strict-Transport-Security` – Specifies the directives and settings that CloudFront uses as the value for the `Strict-Transport-Security` response header\. For this setting, you separately specify:
  + A number of seconds, which CloudFront uses as the value for this header’s `max-age` directive
  + A Boolean setting \(`true` or `false`\) for `preload`, which determines whether CloudFront includes the `preload` directive in this header’s value
  + A Boolean setting \(`true` or `false`\) for `includeSubDomains`, which determines whether CloudFront includes the `includeSubDomains` directive in this header’s value

  For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security) in the MDN Web Docs\.
+ `X-Content-Type-Options` – This is a Boolean setting \(`true` or `false`\) that determines whether or not CloudFront adds the `X-Content-Type-Options` header to responses\. When this setting is `true`, CloudFront adds the `X-Content-Type-Options: nosniff` header to responses\. Otherwise CloudFront doesn’t add this header\.

  For more information about this header, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options) in the MDN Web Docs\.
+ `X-Frame-Options` – Specifies the directive that CloudFront uses as the value for the `X-Frame-Options` response header\. Valid values for this setting are `DENY` or `SAMEORIGIN`\.

  For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) in the MDN Web Docs\.
+ `X-XSS-Protection` – Specifies the directives and settings that CloudFront uses as the value for the `X-XSS-Protection` response header\. For this setting, you separately specify:
  + An `X-XSS-Protection` setting of `0` \(disables XSS filtering\) or `1` \(enables XSS filtering\)
  + A Boolean setting \(`true` or `false`\) for `block`, which determines whether CloudFront includes the `mode=block` directive in this header’s value
  + A reporting URI, which determines whether CloudFront includes the `report=reporting URI` directive in this header’s value

  You can specify `true` for `block`, or you can specify a reporting URI, but you cannot specify both together\. For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection) in the MDN Web Docs\.
+ **Origin override** – Each of these security headers settings contains a Boolean setting \(`true` or `false`\) that determines how CloudFront behaves when the response from the origin contains that header\.

  When this setting is `true` and the origin response contains the header, CloudFront adds the header in the policy to the response it sends to the viewer, ignoring the header it received from the origin\.

  When this setting is `false` and the origin response contains the header, CloudFront includes the header it received from the origin in the response it sends to the viewer\.

  When the origin response doesn’t contain the header, CloudFront adds the header in the policy to the response it sends to the viewer, regardless of whether this setting is `true` or `false`\.

## Custom headers<a name="understanding-response-headers-policies-custom"></a>

The custom headers settings allow you to add and configure custom HTTP headers in a response headers policy\. CloudFront adds these headers to every response that it returns to viewers\. For each custom header, you also specify the header’s value, though values are optional \(CloudFront can add a response header with no value\)\.

Each custom header also has its own **Origin override** setting:
+ When this setting is `true` and the origin response contains the custom header that’s in the policy, CloudFront adds the custom header in the policy to the response it sends to the viewer, ignoring the header it received from the origin\.
+ When this setting is `false` and the origin response contains the custom header that’s in the policy, CloudFront includes the custom header it received from the origin in the response it sends to the viewer\.
+ When the origin response doesn’t contain the custom header that’s in the policy, CloudFront adds the custom header in the policy to the response it sends to the viewer, regardless of whether this setting is `true` or `false`\.