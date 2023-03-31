# Understanding response headers policies<a name="understanding-response-headers-policies"></a>

You can use a response headers policy to specify the HTTP headers that Amazon CloudFront removes or adds in responses that it sends to viewers\. For more information about response headers policies and reasons to use them, see [Adding or removing HTTP headers in CloudFront responses](modifying-response-headers.md)\.

The following topics explain the settings in a response headers policy\. The settings are grouped into categories, which are represented in the following topics\.

**Topics**
+ [Policy details \(metadata\)](#understanding-response-headers-policies-details)
+ [CORS headers](#understanding-response-headers-policies-cors)
+ [Security headers](#understanding-response-headers-policies-security)
+ [Custom headers](#understanding-response-headers-policies-custom)
+ [Remove headers](#understanding-response-headers-policies-remove-headers)
+ [Server\-Timing header](#server-timing-header)

## Policy details \(metadata\)<a name="understanding-response-headers-policies-details"></a>

The policy details settings contain metadata about a response headers policy\.
+ **Name** – A name to identify the response headers policy\. In the console, you use the name to attach the policy to a cache behavior\.
+ **Description** \(optional\) – A comment to describe the response headers policy\. This is optional, but it can help you identify the purpose of the policy\.

## CORS headers<a name="understanding-response-headers-policies-cors"></a>

The cross\-origin resource sharing \(CORS\) settings allow you to add and configure CORS headers in a response headers policy\.

This list focuses on how to specify settings and valid values in a response headers policy\. For more information about each of these headers and how they're used for real\-world CORS requests and responses, see [cross\-origin resource sharing](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) in the MDN Web Docs and the [CORS protocol spec](https://fetch.spec.whatwg.org/#http-cors-protocol)\.

**Access\-Control\-Allow\-Credentials**  
This is a Boolean setting \(`true` or `false`\) that determines if CloudFront adds the `Access-Control-Allow-Credentials` header in responses to CORS requests\. When this setting is set to `true`, CloudFront adds the `Access-Control-Allow-Credentials: true` header in responses to CORS requests\. Otherwise, CloudFront doesn't add this header to responses\.

**Access\-Control\-Allow\-Headers**  
Specifies the header names that CloudFront uses as values for the `Access-Control-Allow-Headers` header in responses to CORS preflight requests\. Valid values for this setting include HTTP header names or the wildcard character \(`*`\), which indicates that all headers are allowed\. See the following table for examples:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/understanding-response-headers-policies.html)

**Access\-Control\-Allow\-Methods**  
Specifies the HTTP methods that CloudFront uses as values for the `Access-Control-Allow-Methods` header in responses to CORS preflight requests\. Valid values are `GET`, `DELETE`, `HEAD`, `OPTIONS`, `PATCH`, `POST`, `PUT`, and `ALL`\. `ALL` is a special value that includes all of the listed HTTP methods\.

**Access\-Control\-Allow\-Origin**  
Specifies the values that CloudFront can use in the `Access-Control-Allow-Origin` response header\. Valid values for this setting include a specific origin \(such as `http://www.example.com`\) or the wildcard character \(`*`\), which indicates that all origins are allowed\. See the following table for examples:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/understanding-response-headers-policies.html)
The wildcard character \(`*`\) is allowed as the leftmost part of the domain \(`*.example.org`\)\. The wildcard character \(`*`\) is not allowed in the following positions:  
+ Top\-level domains \(`example.*`\)
+ To the right of sub\-domains \(`test.*.example.org`\)
+ Inside of terms \(`exa*mple.org)`

**Access\-Control\-Expose\-Headers**  
Specifies the header names that CloudFront uses as values for the `Access-Control-Expose-Headers` header in responses to CORS requests\. Valid values for this setting include HTTP header names or the wildcard character \(`*`\)\.

**Access\-Control\-Max\-Age**  
A number of seconds, which CloudFront uses as the value for the `Access-Control-Max-Age` header in responses to CORS preflight requests\.

**Origin override**  
This is a Boolean setting \(`true` or `false`\) that determines how CloudFront behaves when the response from the origin contains one of the CORS headers that's also in the policy\.  
When this setting is set to `true` and the origin response contains a CORS header that's also in the policy, CloudFront adds the CORS header in the policy to the response that it sends to the viewer\. It ignores the header that it received from the origin\.  
When this setting is `false` and the origin response contains a CORS header that's also in the policy, CloudFront includes the CORS header it received from the origin in the response it sends to the viewer\.  
When the origin response doesn't contain a CORS header that's in the policy, CloudFront adds the CORS header in the policy to the response it sends to the viewer\. CloudFront does this when this setting is set to `true` or `false`\.

## Security headers<a name="understanding-response-headers-policies-security"></a>

You can use the security headers settings to add and configure several security\-related HTTP response headers in a response headers policy\.

This list describes how you can specify settings and valid values in a response headers policy\. For more information about each of these headers and how they're used in real\-world HTTP responses, see the links to the MDN Web Docs\.

**Content\-Security\-Policy**  
Specifies the content security policy directives that CloudFront uses as values for the `Content-Security-Policy` response header\.  
For more information about this header and valid policy directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) in the MDN Web Docs\.  
The `Content-Security-Policy` header value is limited to 1783 characters\.

**Referrer\-Policy**  
Specifies the referrer policy directive that CloudFront uses as the value for the `Referrer-Policy` response header\. Valid values for this setting are `no-referrer`, `no-referrer-when-downgrade`, `origin`, `origin-when-cross-origin`, `same-origin`, `strict-origin`, `strict-origin-when-cross-origin`, and `unsafe-url`\.  
For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy) in the MDN Web Docs\.

**Strict\-Transport\-Security**  
Specifies the directives and settings that CloudFront uses as the value for the `Strict-Transport-Security` response header\. For this setting, you separately specify:  
+ A number of seconds, which CloudFront uses as the value for the `max-age` directive of this header
+ A Boolean setting \(`true` or `false`\) for `preload`, which determines whether CloudFront includes the `preload` directive in the value of this header
+ A Boolean setting \(`true` or `false`\) for `includeSubDomains`, which determines whether CloudFront includes the `includeSubDomains` directive in the value of this header
For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security) in the MDN Web Docs\.

**X\-Content\-Type\-Options**  
This is a Boolean setting \(`true` or `false`\) that determines if CloudFront adds the `X-Content-Type-Options` header to responses\. When this setting is `true`, CloudFront adds the `X-Content-Type-Options: nosniff` header to responses\. Otherwise CloudFront doesn't add this header\.  
For more information about this header, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Content-Type-Options) in the MDN Web Docs\.

**X\-Frame\-Options**  
Specifies the directive that CloudFront uses as the value for the `X-Frame-Options` response header\. Valid values for this setting are `DENY` or `SAMEORIGIN`\.  
For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) in the MDN Web Docs\.

**X\-XSS\-Protection**  
Specifies the directives and settings that CloudFront uses as the value for the `X-XSS-Protection` response header\. For this setting, you separately specify:  
+ An `X-XSS-Protection` setting of `0` \(disables XSS filtering\) or `1` \(enables XSS filtering\)
+ A Boolean setting \(`true` or `false`\) for `block`, which determines whether CloudFront includes the `mode=block` directive in the value for this header
+ A reporting URI, which determines whether CloudFront includes the `report=reporting URI` directive in the value for this header
You can specify `true` for `block`, or you can specify a reporting URI, but you can't specify both together\. For more information about this header and these directives, see [https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection) in the MDN Web Docs\.

**Origin override**  
Each of these security headers settings contains a Boolean setting \(`true` or `false`\) that determines how CloudFront behaves when the response from the origin contains that header\.  
When this setting is set to `true` and the origin response contains the header, CloudFront adds the header in the policy to the response that it sends to the viewer\. It ignores the header that it received from the origin\.  
When this setting is set to `false` and the origin response contains the header, CloudFront includes the header that it received from the origin in the response that it sends to the viewer\.  
When the origin response doesn't contain the header, CloudFront adds the header in the policy to the response that it sends to the viewer\. CloudFront does this when this setting is set to `true` or `false`\.

## Custom headers<a name="understanding-response-headers-policies-custom"></a>

You can use custom headers settings to add and configure custom HTTP headers in a response headers policy\. CloudFront adds these headers to every response that it returns to viewers\. For each custom header, you also specify the value for the header, though specifying a value is optional\. This is because CloudFront can add a response header with no value\.

Each custom header also has its own **Origin override** setting:
+ When this setting is set to `true` and the origin response contains the custom header that's in the policy, CloudFront adds the custom header in the policy to the response that it sends to the viewer\. It ignores the header that it received from the origin\.
+ When this setting is `false` and the origin response contains the custom header that's in the policy, CloudFront includes the custom header that it received from the origin in the response that it sends to the viewer\.
+ When the origin response doesn't contain the custom header that's in the policy, CloudFront adds the custom header in the policy to the response that it sends to the viewer\. CloudFront does this when this setting is set to `true` or `false`\.

## Remove headers<a name="understanding-response-headers-policies-remove-headers"></a>

You can specify headers that you want CloudFront to remove from the responses it receives from the origin so the headers are not included in the responses that CloudFront sends to viewers\. CloudFront removes the headers from every response it sends to viewers, whether the objects is served from CloudFront's cache or from the origin\. For example, you can remove headers that are of no use to browsers, such as `X-Powered-By` or `Vary`, so that CloudFront removes these headers from the responses that it sends to viewers\.

When you specify headers to remove using a response headers policy, CloudFront removes the headers first and then adds any headers that are specified in other sections of the response headers policy \(CORS headers, security headers, custom headers, etc\.\)\. If you specify a header to remove but also add the same header in another section of the policy, CloudFront includes the header in the responses that it sends to viewers\.

**Note**  
You can use a response headers policy to remove the `Server` and `Date` headers that CloudFront received from the origin, so that these headers \(as received from the origin\) are not included in the responses that CloudFront sends to viewers\. However, if you do that, CloudFront adds its own version of these headers to responses that it sends to viewers\. For the `Server` header that CloudFront adds, the header's value is `CloudFront`\.

### Headers that you can't remove<a name="remove-headers-disallowed"></a>

You cannot remove the following headers using a response headers policy\. If you specify these headers in the **Remove headers** section of a response headers policy \(`ResponseHeadersPolicyRemoveHeadersConfig` in the API\), you receive an error\.
+ `Connection`
+ `Content-Encoding`
+ `Content-Length`
+ `Expect`
+ `Host`
+ `Keep-Alive`
+ `Proxy-Authenticate`
+ `Proxy-Authorization`
+ `Proxy-Connection`
+ `Trailer`
+ `Transfer-Encoding`
+ `Upgrade`
+ `Via`
+ `Warning`
+ `X-Accel-Buffering`
+ `X-Accel-Charset`
+ `X-Accel-Limit-Rate`
+ `X-Accel-Redirect`
+ `X-Amz-Cf-.*`
+ `X-Amzn-Auth`
+ `X-Amzn-Cf-Billing`
+ `X-Amzn-Cf-Id`
+ `X-Amzn-Cf-Xff`
+ `X-Amzn-ErrorType`
+ `X-Amzn-Fle-Profile`
+ `X-Amzn-Header-Count`
+ `X-Amzn-Header-Order`
+ `X-Amzn-Lambda-Integration-Tag`
+ `X-Amzn-RequestId`
+ `X-Cache`
+ `X-Edge-.*`
+ `X-Forwarded-Proto`
+ `X-Real-Ip`

## Server\-Timing header<a name="server-timing-header"></a>

Use the `Server-Timing` header setting to enable the `Server-Timing` header in HTTP responses sent from CloudFront\. You can use this header to view metrics that can help you gain insights about the behavior and performance of CloudFront and your origin\. For example, you can see which cache layer served a cache hit\. Or, you can see the first byte latency from the origin if there's a cache miss\. The metrics in the `Server-Timing` header can help you troubleshoot issues or test the efficiency of your CloudFront or origin configuration\.

For more information about using the `Server-Timing` header with CloudFront, see the following topics\.

To enable the `Server-Timing` header, [create \(or edit\) a response headers policy](creating-response-headers-policies.md)\.

**Topics**
+ [Sampling rate and Pragma request header](#server-timing-header-sampling-rate)
+ [Server\-Timing header from the origin](#server-timing-header-origin)
+ [Server\-Timing header metrics](#server-timing-header-metrics)
+ [Server\-Timing header examples](#server-timing-header-examples)

### Sampling rate and Pragma request header<a name="server-timing-header-sampling-rate"></a>

When you enable the `Server-Timing` header in a response headers policy, you also specify the *sampling rate*\. The sampling rate is a number 0–100 \(inclusive\) that specifies the percentage of responses that you want CloudFront to add the `Server-Timing` header to\. When you set the sampling rate to 100, CloudFront adds the `Server-Timing` header to the HTTP response for every request that matches the cache behavior that the response headers policy is attached to\. When you set it to 50, CloudFront adds the header to 50% of the responses for requests that match the cache behavior\. You can set the sampling rate to any number 0–100 with up to four decimal places\.

When the sampling rate is set to a number lower than 100, you can't control which responses CloudFront adds the `Server-Timing` header to, only the percentage\. However, you can add the `Pragma` header with a value set to `server-timing` in an HTTP request to receive the `Server-Timing` header in the response to that request\. This works no matter what the sampling rate is set to\. Even when the sampling rate is set to zero \(0\), CloudFront adds the `Server-Timing` header to the response if the request contains the `Pragma: server-timing` header\.

### Server\-Timing header from the origin<a name="server-timing-header-origin"></a>

When there is a cache miss and CloudFront forwards the request to the origin, the origin might include a `Server-Timing` header in its response to CloudFront\. In this case, CloudFront adds its [metrics](#server-timing-header-metrics) to the `Server-Timing` header that it received from the origin\. The response that CloudFront sends to the viewer contains a single `Server-Timing` header that includes the value that came from the origin and the metrics that CloudFront added\. The header value from the origin might be at the end, or in between two sets of metrics that CloudFront adds to the header\.

When there is a cache hit, the response that CloudFront sends to the viewer contains a single `Server-Timing` header that includes only the CloudFront metrics in the header value \(the value from the origin is not included\)\.

### Server\-Timing header metrics<a name="server-timing-header-metrics"></a>

When CloudFront adds the `Server-Timing` header to an HTTP response, the value of the header contains one or more metrics that can help you gain insights about the behavior and performance of CloudFront and your origin\. The following list contains all the metrics and their potential values\. A `Server-Timing` header contains only some of these metrics, depending on the nature of the request and response through CloudFront\.

Some of these metrics are included in the `Server-Timing` header with a name only \(no value\)\. Others are a name and a value\. When a metric has a value, the name and value are separated by a semicolon \(`;`\)\. When the header contains more than one metric, the metrics are separated by a comma \(`,`\)\.

**cdn\-cache\-hit**  
CloudFront provided a response from the cache without making a request to the origin\.

**cdn\-cache\-refresh**  
CloudFront provided a response from the cache after sending a request to the origin to verify that the cached object is still valid\. In this case, CloudFront didn't retrieve the full object from the origin\.

**cdn\-cache\-miss**  
CloudFront didn't provide the response from the cache\. In this case, CloudFront requested the full object from the origin before returning the response\.

**cdn\-pop**  
Contains a value that describes which CloudFront point of presence \(POP\) handled the request\.

**cdn\-rid**  
Contains a value with the CloudFront unique identifier for the request\. You can use this request identifier \(RID\) when troubleshooting issues with AWS Support

**cdn\-hit\-layer**  
This metric is present when CloudFront provides a response from the cache without making a request to the origin\. It contains one of the following values:  
+ **EDGE** – CloudFront provided the cached response from a POP location\.
+ **REC** – CloudFront provided the cached response from a [regional edge cache](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches) \(REC\) location\.
+ **Origin Shield** – CloudFront provided the cached response from the REC that's acting as [Origin Shield](origin-shield.md)\.

**cdn\-upstream\-layer**  
When CloudFront requests the full object from the origin, this metric is present and contains one of the following values:  
+ **EDGE** – A POP location sent the request directly to the origin\.
+ **REC** – A REC location sent the request directly to the origin\.
+ **Origin Shield** – The REC that's acting as [Origin Shield](origin-shield.md) sent the request directly to the origin\.

**cdn\-upstream\-dns**  
Contains a value with the number of milliseconds that were spent retrieving the DNS record for the origin\. A value of zero \(0\) indicates that CloudFront used a cached DNS result or reused an existing connection\.

**cdn\-upstream\-connect**  
Contains a value with the number of milliseconds between when the origin DNS request completed and a TCP \(and TLS, if applicable\) connection to the origin completed\. A value of zero \(0\) indicates that CloudFront reused an existing connection\.

**cdn\-upstream\-fbl**  
Contains a value with the number of milliseconds between when the origin HTTP request is completed and when the first byte is received in the response from the origin \(first byte latency\)\.

**cdn\-downstream\-fbl**  
Contains a value with the number of milliseconds between when the edge location finished receiving the request and when it sent the first byte of the response to the viewer\.

### Server\-Timing header examples<a name="server-timing-header-examples"></a>

The following are examples of a `Server-Timing` header that a viewer might receive from CloudFront when the `Server-Timing` header setting is enabled\.

**Example – cache miss**  
The following example shows a `Server-Timing` header that a viewer might receive when the requested object is not in the CloudFront cache\.  

```
Server-Timing: cdn-upstream-layer;desc="EDGE",cdn-upstream-dns;dur=0,cdn-upstream-connect;dur=114,cdn-upstream-fbl;dur=177,cdn-cache-miss,cdn-pop;desc="PHX50-C2",cdn-rid;desc="yNPsyYn7skvTzwWkq3Wcc8Nj_foxUjQUe9H1ifslzWhb0w7aLbFvGg==",cdn-downstream-fbl;dur=436
```
This `Server-Timing` header indicates the following:  
+ The origin request was sent from a CloudFront point of presence \(POP\) location \(`cdn-upstream-layer;desc="EDGE"`\)\.
+ CloudFront used a cached DNS result for the origin \(`cdn-upstream-dns;dur=0`\)\.
+ It took 114 milliseconds for CloudFront to complete the TCP \(and TLS, if applicable\) connection to the origin \(`cdn-upstream-connect;dur=114`\)\.
+ It took 177 milliseconds for CloudFront to receive the first byte of the response from the origin, after completing the request \(`cdn-upstream-fbl;dur=177`\)\.
+ The requested object wasn't in CloudFront's cache \(`cdn-cache-miss`\)\.
+ The request was received at the edge location identified by the code `PHX50-C2` \(`cdn-pop;desc="PHX50-C2"`\)\.
+ The CloudFront unique ID for this request was `yNPsyYn7skvTzwWkq3Wcc8Nj_foxUjQUe9H1ifslzWhb0w7aLbFvGg==` \(`cdn-rid;desc="yNPsyYn7skvTzwWkq3Wcc8Nj_foxUjQUe9H1ifslzWhb0w7aLbFvGg=="`\)\.
+ It took 436 milliseconds for CloudFront to send the first byte of the response to the viewer, after receiving the viewer request \(`cdn-downstream-fbl;dur=436`\)\.

**Example – cache hit**  
The following example shows a `Server-Timing` header that a viewer might receive when the requested object is in CloudFront's cache\.  

```
Server-Timing: cdn-cache-hit,cdn-pop;desc="SEA19-C1",cdn-rid;desc="nQBz4aJU2kP9iC3KHEq7vFxfMozu-VYBwGzkW9diOpeVc7xsrLKj-g==",cdn-hit-layer;desc="REC",cdn-downstream-fbl;dur=137
```
This `Server-Timing` header indicates the following:  
+ The requested object was in the cache \(`cdn-cache-hit`\)\.
+ The request was received at the edge location identified by the code `SEA19-C1` \(`cdn-pop;desc="SEA19-C1"`\)\.
+ The CloudFront unique ID for this request was `nQBz4aJU2kP9iC3KHEq7vFxfMozu-VYBwGzkW9diOpeVc7xsrLKj-g==` \(`cdn-rid;desc="nQBz4aJU2kP9iC3KHEq7vFxfMozu-VYBwGzkW9diOpeVc7xsrLKj-g=="`\)\.
+ The requested object was cached in a regional edge cache \(REC\) location \(`cdn-hit-layer;desc="REC"`\)\.
+ It took 137 milliseconds for CloudFront to send the first byte of the response to the viewer, after receiving the viewer request \(`cdn-downstream-fbl;dur=137`\)\.