# Caching Content Based on Cookies<a name="Cookies"></a>

By default, CloudFront doesn’t consider cookies when processing requests and responses, or when caching your objects in edge locations\. If CloudFront receives two requests that are identical except for what’s in the `Cookie` header, then, by default, CloudFront treats the requests as identical and returns the same object for both requests\.

You can configure CloudFront to forward to your origin some or all of the cookies in viewer requests, and to cache separate versions of your objects based on the cookie values that it forwards\. When you do this, CloudFront uses some or all of the cookies in viewer requests—whichever ones it’s configured to forward—to uniquely identify an object in the cache\. \(For RTMP distributions, you cannot configure CloudFront to process cookies and CloudFront does not cache cookies in edge caches\.\)

For example, suppose that requests for `locations.html` contain a `country` cookie that has a value of either `uk` or `fr`\. When you configure CloudFront to cache your objects based on the value of the `country` cookie, CloudFront forwards requests for `locations.html` to the origin and includes the `country` cookie and its value\. Your origin returns `locations.html`, and CloudFront caches the object once for requests in which the value of the `country` cookie is `uk` and once for requests in which the value is `fr`\.

**Important**  
Amazon S3 and some HTTP servers don’t process cookies\. Don’t configure CloudFront to forward cookies to an origin that doesn’t process cookies or doesn’t vary its response based on cookies\. That can cause CloudFront to forward more requests to the origin for the same object, which slows performance and increases the load on the origin\. If, considering the previous example, your origin doesn’t process the `country` cookie or always returns the same version of `locations.html` to CloudFront regardless of the value of the `country` cookie, don’t configure CloudFront to forward that cookie\.  
Conversely, if your custom origin depends on a particular cookie or sends different responses based on a cookie, make sure you configure CloudFront to forward that cookie to the origin\. Otherwise, CloudFront removes the cookie before forwarding the request to your origin\.

To configure cookie forwarding, you update your distribution’s cache behavior\. For more information about cache behaviors, see [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior), particularly the [Forward Cookies](distribution-web-values-specify.md#DownloadDistValuesForwardCookies) and [Whitelist Cookies](distribution-web-values-specify.md#DownloadDistValuesWhitelistCookies) sections\.

You can configure each cache behavior to do one of the following:
+ **Forward all cookies to your origin – ** CloudFront includes all cookies sent by the viewer when it forwards requests to the origin\. When your origin returns a response, CloudFront caches the response using the cookie names and values in the viewer request\. If the origin response includes `Set-Cookie` headers, CloudFront returns them to the viewer with the requested object\. CloudFront also caches the `Set-Cookie` headers with the object returned from the origin, and sends those `Set-Cookie` headers to viewers on all cache hits\.
+ **Forward a whitelist of cookies that you specify – ** CloudFront removes any cookies that the viewer sends that aren’t on the whitelist before it forwards a request to the origin\. CloudFront caches the response using the whitelisted cookies names and values in the viewer request\. If the origin response includes `Set-Cookie` headers, CloudFront returns them to the viewer with the requested object\. CloudFront also caches the `Set-Cookie` headers with the object returned from the origin, and sends those `Set-Cookie` headers to viewers on all cache hits\.

  For information about specifying wildcards in cookie names in a whitelist, see [Whitelist Cookies](distribution-web-values-specify.md#DownloadDistValuesWhitelistCookies)\.

  For the current quota on the number of cookie names that you can whitelist for each cache behavior, or to request a higher quota, see [Quotas on Whitelisted Query Strings \(Web Distributions Only\)](cloudfront-limits.md#limits-whitelisted-query-strings)\.
+ **Don’t forward cookies to your origin – **CloudFront doesn’t cache your objects based on cookie sent by the viewer\. In addition, CloudFront removes cookies before forwarding requests to your origin, and removes `Set-Cookie` headers from responses before returning responses to your viewers\.

Note the following about specifying the cookies that you want to forward:

**Access logs**  
If you configure CloudFront to log requests and to log cookies, CloudFront logs all cookies and all cookie attributes, even if you configure CloudFront not to forward cookies to your origin or if you configure CloudFront to forward only a whitelist of specific cookies\. For more information about CloudFront logging, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**Case sensitivity**  
Cookie names and values are both case\-sensitive\. For example, if CloudFront is configured to forward all cookies, and two viewer requests for the same object have cookies that are identical except for case, CloudFront caches the object twice\.

**CloudFront sorts cookies**  
If CloudFront is configured to forward cookies \(all or a whitelist\), CloudFront sorts the cookies in natural order by cookie name before forwarding the request to your origin\.

**If\-Modified\-Since and If\-None\-Match**  
`If-Modified-Since` and `If-None-Match` conditional requests are not supported when CloudFront is configured to forward cookies \(all or a whitelist\)\.

**Standard name–value pair format is required**  
CloudFront forwards a cookie header only if the value conforms to the [standard name–value pair format](https://tools.ietf.org/html/rfc6265#section-4.1.1), for example: `"Cookie: cookie1=value1; cookie2=value2"`

**Disable caching of `Set-Cookie` headers**  
If CloudFront is configured to forward cookies to the origin \(all or a whitelist\), it also caches the `Set-Cookie` headers received in the origin response\. CloudFront includes these `Set-Cookie` headers in its response to the original viewer, and also includes them in subsequent responses that are served from the CloudFront cache\.  
If you want to receive cookies at your origin but you don’t want CloudFront to cache the `Set-Cookie` headers in your origin’s responses, configure your origin to add a `Cache-Control` header with a `no-cache` directive that specifies `Set-Cookie` as a field name\. For example: `Cache-Control: no-cache="Set-Cookie"`\. For more information, see [Response Cache\-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.2) in the *Hypertext Transfer Protocol \(HTTP/1\.1\): Caching* standard\.

**Maximum length of cookie names**  
If you configure CloudFront to forward a whitelist of specific cookies to your origin, the total number of bytes in all of the cookie names that you configure CloudFront to forward can’t exceed 512 minus the number of cookies that you’re forwarding\. For example, if you configure CloudFront to forward 10 cookies to your origin, the combined length of the names of the 10 cookies can’t exceed 502 bytes \(512 – 10\)\.  
If you configure CloudFront to forward all cookies to your origin, the length of cookie names doesn’t matter\.

For information about using the CloudFront console to update a distribution so CloudFront forwards cookies to the origin, see [Updating a Distribution](HowToUpdateDistribution.md)\. For information about using the CloudFront API to update a distribution, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.