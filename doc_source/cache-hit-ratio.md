# Increasing the Proportion of Requests that Are Served from CloudFront Edge Caches \(Cache Hit Ratio\)<a name="cache-hit-ratio"></a>

You can improve performance by increasing the proportion of your viewer requests that are served from CloudFront edge caches instead of going to your origin servers for content\. This is known as improving the cache hit ratio for your distribution\.

The following sections explain how to improve your cache hit ratio\.

**Topics**
+ [Specifying How Long CloudFront Caches Your Objects](#cache-hit-ratio-duration)
+ [Caching Based on Query String Parameters](#cache-hit-ratio-query-string-parameters)
+ [Caching Based on Cookie Values](#cache-hit-ratio-cookies)
+ [Caching Based on Request Headers](#cache-hit-ratio-request-headers)
+ [Remove Accept\-Encoding Header When Compression is Not Needed](#cache-hit-ratio-remove-accept-encoding)
+ [Serving Media Content by Using HTTP](#cache-hit-ratio-http-streaming)

## Specifying How Long CloudFront Caches Your Objects<a name="cache-hit-ratio-duration"></a>

To increase your cache hit ratio, you can configure your origin to add a `Cache-Control max-age` directive to your objects, and specify the longest practical value for `max-age`\. The shorter the cache duration, the more frequently CloudFront forwards requests to your origin to determine if an object has changed and to get the latest version\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

## Caching Based on Query String Parameters<a name="cache-hit-ratio-query-string-parameters"></a>

If you configure CloudFront to cache based on query string parameters, you can improve caching if you do the following:
+ Configure CloudFront to forward only the query string parameters for which your origin will return unique objects\.
+ Use the same case \(uppercase or lowercase\) for all instances of the same parameter\. For example, if one request contains `parameter1=A` and another contains `parameter1=a`, CloudFront forwards separate requests to your origin when a request contains `parameter1=A` and when a request contains `parameter1=a`\. CloudFront then separately caches the corresponding objects returned by your origin separately even if the objects are identical\. If you use just `A` or `a`, CloudFront forwards fewer requests to your origin\.
+ List parameters in the same order\. As with differences in case, if one request for an object contains the query string `parameter1=a&parameter2=b` and another request for the same object contains `parameter2=b&parameter1=a`, CloudFront forwards both requests to your origin and separately caches the corresponding objects even if they're identical\. If you always use the same order for parameters, CloudFront forwards fewer requests to your origin\.

For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\. If you want to review the query strings that CloudFront forwards to your origin, see the values in the `cs-uri-query` column of your CloudFront log files\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

## Caching Based on Cookie Values<a name="cache-hit-ratio-cookies"></a>

If you configure CloudFront to cache based on cookie values, you can improve caching if you do the following:
+ Configure CloudFront to forward only specified cookies instead of forwarding all cookies\. For the cookies that you configure CloudFront to forward to your origin, CloudFront forwards every combination of cookie name and value\. It then separately caches the objects that your origin returns, even if they're all identical\.

  For example, suppose that viewers include two cookies in every request, that each cookie has three possible values, and that all combinations of cookie values are possible\. CloudFront forwards up to six different requests to your origin for each object\. If your origin returns different versions of an object based on only one of the cookies, then CloudFront is forwarding more requests to your origin than necessary and is needlessly caching multiple identical versions of the object\.
+ Create separate cache behaviors for static and dynamic content, and configure CloudFront to forward cookies to your origin only for dynamic content\.

  For example, suppose you have just one cache behavior for your distribution and that you're using the distribution both for dynamic content, such as `.js` files, and for `.css` files that rarely change\. CloudFront caches separate versions of your `.css` files based on cookie values, so each CloudFront edge location forwards a request to your origin for every new cookie value or combination of cookie values\.

  If you create a cache behavior for which the path pattern is `*.css` and for which CloudFront doesn't cache based on cookie values, then CloudFront forwards requests for `.css` files to your origin for only the first request that an edge location receives for a given `.css` file and for the first request after a `.css` file expires\.
+ If possible, create separate cache behaviors for dynamic content when cookie values are unique for each user \(such as a user ID\), and dynamic content that varies based on a smaller number of unique values\.

For more information, see [Caching Content Based on Cookies](Cookies.md)\. If you want to review the cookies that CloudFront forwards to your origin, see the values in the `cs(Cookie)` column of your CloudFront log files\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

## Caching Based on Request Headers<a name="cache-hit-ratio-request-headers"></a>

If you configure CloudFront to cache based on request headers, you can improve caching if you do the following:
+ Configure CloudFront to forward and cache based on only specified headers instead of forwarding and caching based on all headers\. For the headers that you specify, CloudFront forwards every combination of header name and value\. It then separately caches the objects that your origin returns even if they're all identical\.
**Note**  
CloudFront always forwards to your origin the headers specified in the following topics:  
How CloudFront Processes and Forwards Requests to Your Amazon S3 Origin Server > [HTTP Request Headers That CloudFront Removes or Updates](RequestAndResponseBehaviorS3Origin.md#request-s3-removed-headers)
How CloudFront Processes and Forwards Requests to Your Custom Origin Server > [HTTP Request Headers and CloudFront Behavior \(Custom and S3 Origins\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)

  When you configure CloudFront to cache based on request headers, you don't change the headers that CloudFront forwards, only whether CloudFront caches objects based on the header values\.
+ Try to avoid caching based on request headers that have large numbers of unique values\.

  For example, if you want to serve different sizes of an image based on the user's device, then don't configure CloudFront to cache based on the `User-Agent` header, which has an enormous number of possible values\. Instead, configure CloudFront to cache based on the CloudFront device\-type headers `CloudFront-Is-Desktop-Viewer`, `CloudFront-Is-Mobile-Viewer`, `CloudFront-Is-SmartTV-Viewer`, and `CloudFront-Is-Tablet-Viewer`\. In addition, if you're returning the same version of the image for tablets and desktops, then forward only the `CloudFront-Is-Tablet-Viewer` header, not the `CloudFront-Is-Desktop-Viewer` header\.

For more information, see [Caching Content Based on Request Headers](header-caching.md)\.

## Remove Accept\-Encoding Header When Compression is Not Needed<a name="cache-hit-ratio-remove-accept-encoding"></a>

If compression is not enabled—because the origin doesn’t support it, CloudFront doesn’t support it, or the content is not compressible—you can increase the cache hit ratio by associating a cache behavior in your distribution to an origin that sets the Custom Origin Header as follows:
+ **Header name**: `Accept-Encoding`
+ **Header value**: \(Keep blank\)

When you use this configuration, CloudFront removes the `Accept-Encoding` header from the cache key and doesn’t include the header in origin requests\. This configuration applies to all content that CloudFront serves with the distribution from that origin\.

## Serving Media Content by Using HTTP<a name="cache-hit-ratio-http-streaming"></a>

For information about optimizing video on demand \(VOD\) and streaming video content, see [Video on Demand and Live Streaming Video with CloudFront](on-demand-streaming-video.md)\.