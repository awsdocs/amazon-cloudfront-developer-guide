# Increasing the proportion of requests that are served directly from the CloudFront caches \(cache hit ratio\)<a name="cache-hit-ratio"></a>

You can improve performance by increasing the proportion of your viewer requests that are served directly from the CloudFront cache instead of going to your origin servers for content\. This is known as improving the cache hit ratio\.

The following sections explain how to improve your cache hit ratio\.

**Topics**
+ [Specifying how long CloudFront caches your objects](#cache-hit-ratio-duration)
+ [Using Origin Shield](#cache-hit-ratio-use-origin-shield)
+ [Caching based on query string parameters](#cache-hit-ratio-query-string-parameters)
+ [Caching based on cookie values](#cache-hit-ratio-cookies)
+ [Caching based on request headers](#cache-hit-ratio-request-headers)
+ [Remove Accept\-Encoding header when compression is not needed](#cache-hit-ratio-remove-accept-encoding)
+ [Serving media content by using HTTP](#cache-hit-ratio-http-streaming)

## Specifying how long CloudFront caches your objects<a name="cache-hit-ratio-duration"></a>

To increase your cache hit ratio, you can configure your origin to add a [Cache\-Control max\-age](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control) directive to your objects, and specify the longest practical value for `max-age`\. The shorter the cache duration, the more frequently CloudFront sends requests to your origin to determine if an object has changed and to get the latest version\. For more information, see [Managing how long content stays in the cache \(expiration\)](Expiration.md)\.

## Using Origin Shield<a name="cache-hit-ratio-use-origin-shield"></a>

CloudFront Origin Shield can help improve the cache hit ratio of your CloudFront distribution, because it provides an additional layer of caching in front of your origin\. When you use Origin Shield, all requests from all of CloudFront’s caching layers to your origin come from a single location\. CloudFront can retrieve each object using a single origin request from Origin Shield, and all other layers of the CloudFront cache \(edge locations and [regional edge caches](HowCloudFrontWorks.md#CloudFrontRegionaledgecaches)\) can retrieve the object from Origin Shield\.

For more information, see [Using Amazon CloudFront Origin Shield](origin-shield.md)\.

## Caching based on query string parameters<a name="cache-hit-ratio-query-string-parameters"></a>

If you configure CloudFront to cache based on query string parameters, you can improve caching if you do the following:
+ Configure CloudFront to forward only the query string parameters for which your origin will return unique objects\.
+ Use the same case \(uppercase or lowercase\) for all instances of the same parameter\. For example, if one request contains `parameter1=A` and another contains `parameter1=a`, CloudFront forwards separate requests to your origin when a request contains `parameter1=A` and when a request contains `parameter1=a`\. CloudFront then separately caches the corresponding objects returned by your origin separately even if the objects are identical\. If you use just `A` or `a`, CloudFront forwards fewer requests to your origin\.
+ List parameters in the same order\. As with differences in case, if one request for an object contains the query string `parameter1=a&parameter2=b` and another request for the same object contains `parameter2=b&parameter1=a`, CloudFront forwards both requests to your origin and separately caches the corresponding objects even if they're identical\. If you always use the same order for parameters, CloudFront forwards fewer requests to your origin\.

For more information, see [Caching content based on query string parameters](QueryStringParameters.md)\. If you want to review the query strings that CloudFront forwards to your origin, see the values in the `cs-uri-query` column of your CloudFront log files\. For more information, see [Configuring and using standard logs \(access logs\)](AccessLogs.md)\.

## Caching based on cookie values<a name="cache-hit-ratio-cookies"></a>

If you configure CloudFront to cache based on cookie values, you can improve caching if you do the following:
+ Configure CloudFront to forward only specified cookies instead of forwarding all cookies\. For the cookies that you configure CloudFront to forward to your origin, CloudFront forwards every combination of cookie name and value\. It then separately caches the objects that your origin returns, even if they're all identical\.

  For example, suppose that viewers include two cookies in every request, that each cookie has three possible values, and that all combinations of cookie values are possible\. CloudFront forwards up to six different requests to your origin for each object\. If your origin returns different versions of an object based on only one of the cookies, then CloudFront is forwarding more requests to your origin than necessary and is needlessly caching multiple identical versions of the object\.
+ Create separate cache behaviors for static and dynamic content, and configure CloudFront to forward cookies to your origin only for dynamic content\.

  For example, suppose you have just one cache behavior for your distribution and that you're using the distribution both for dynamic content, such as `.js` files, and for `.css` files that rarely change\. CloudFront caches separate versions of your `.css` files based on cookie values, so each CloudFront edge location forwards a request to your origin for every new cookie value or combination of cookie values\.

  If you create a cache behavior for which the path pattern is `*.css` and for which CloudFront doesn't cache based on cookie values, then CloudFront forwards requests for `.css` files to your origin for only the first request that an edge location receives for a given `.css` file and for the first request after a `.css` file expires\.
+ If possible, create separate cache behaviors for dynamic content when cookie values are unique for each user \(such as a user ID\), and dynamic content that varies based on a smaller number of unique values\.

For more information, see [Caching content based on cookies](Cookies.md)\. If you want to review the cookies that CloudFront forwards to your origin, see the values in the `cs(Cookie)` column of your CloudFront log files\. For more information, see [Configuring and using standard logs \(access logs\)](AccessLogs.md)\.

## Caching based on request headers<a name="cache-hit-ratio-request-headers"></a>

If you configure CloudFront to cache based on request headers, you can improve caching if you do the following:
+ Configure CloudFront to forward and cache based on only specified headers instead of forwarding and caching based on all headers\. For the headers that you specify, CloudFront forwards every combination of header name and value\. It then separately caches the objects that your origin returns even if they're all identical\.
**Note**  
CloudFront always forwards to your origin the headers specified in the following topics:  
How CloudFront Processes and Forwards Requests to Your Amazon S3 Origin Server > [HTTP request headers that CloudFront removes or updates](RequestAndResponseBehaviorS3Origin.md#request-s3-removed-headers)
How CloudFront Processes and Forwards Requests to Your Custom Origin Server > [HTTP request headers and CloudFront behavior \(custom and Amazon S3 origins\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)

  When you configure CloudFront to cache based on request headers, you don't change the headers that CloudFront forwards, only whether CloudFront caches objects based on the header values\.
+ Try to avoid caching based on request headers that have large numbers of unique values\.

  For example, if you want to serve different sizes of an image based on the user's device, then don't configure CloudFront to cache based on the `User-Agent` header, which has an enormous number of possible values\. Instead, configure CloudFront to cache based on the CloudFront device\-type headers `CloudFront-Is-Desktop-Viewer`, `CloudFront-Is-Mobile-Viewer`, `CloudFront-Is-SmartTV-Viewer`, and `CloudFront-Is-Tablet-Viewer`\. In addition, if you're returning the same version of the image for tablets and desktops, then forward only the `CloudFront-Is-Tablet-Viewer` header, not the `CloudFront-Is-Desktop-Viewer` header\.

For more information, see [Caching content based on request headers](header-caching.md)\.

## Remove Accept\-Encoding header when compression is not needed<a name="cache-hit-ratio-remove-accept-encoding"></a>

If compression is not enabled—because the origin doesn’t support it, CloudFront doesn’t support it, or the content is not compressible—you can increase the cache hit ratio by associating a cache behavior in your distribution to an origin that sets the Custom Origin Header as follows:
+ **Header name**: `Accept-Encoding`
+ **Header value**: \(Keep blank\)

When you use this configuration, CloudFront removes the `Accept-Encoding` header from the cache key and doesn’t include the header in origin requests\. This configuration applies to all content that CloudFront serves with the distribution from that origin\.

## Serving media content by using HTTP<a name="cache-hit-ratio-http-streaming"></a>

For information about optimizing video on demand \(VOD\) and streaming video content, see [Video on Demand and Live Streaming Video with CloudFront](on-demand-streaming-video.md)\.