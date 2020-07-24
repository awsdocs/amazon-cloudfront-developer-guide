# Understanding the cache key<a name="understanding-the-cache-key"></a>

The *cache key* determines whether a viewer request to a CloudFront edge location results in a *cache hit*\. The cache key is the unique identifier for an object in the cache\. Each object in the cache has a unique cache key\.

A cache hit occurs when a viewer request generates the same cache key as a prior request, and the object for that cache key is in the edge location’s cache and valid\. When there’s a cache hit, the requested object is served to the viewer from a CloudFront edge location, which has the following benefits:
+ Reduced load on your origin server
+ Reduced latency for the viewer

You can get better performance from your website or application when you have a higher *cache hit ratio* \(a higher proportion of viewer requests that result in a cache hit\)\. One way to improve your cache hit ratio is to include only the minimum necessary values in the cache key\. For more information, see the following sections\.

You can modify the values \(URL query strings, HTTP headers, and cookies\) in the cache key by using a [cache policy](controlling-the-cache-key.md)\. \(You can also modify the cache key using a [Lambda@Edge function](lambda-at-the-edge.md)\.\) Before modifying the cache key, it’s important to understand how your application is designed and when and how it might serve different responses based on characteristics of the viewer request\. When a value in the viewer request determines the response that your origin returns, you should include that value in the cache key\. But if you include a value in the cache key that doesn’t affect the response that your origin returns, you might end up caching duplicate objects\.

## The default cache key<a name="cache-key-default"></a>

By default, the cache key for a CloudFront distribution includes the following information:
+ The domain name of the CloudFront distribution \(for example, d111111abcdef8\.cloudfront\.net\)
+ The URL path of the requested object \(for example, `/content/stories/example-story.html`\)

Other values from the viewer request are not included in the cache key, by default\. Consider the following HTTP request from a web browser\.

```
GET /content/stories/example-story.html?ref=0123abc&split-pages=false HTTP/1.1
Host: d111111abcdef8.cloudfront.net
User-Agent: Mozilla/5.0 Gecko/20100101 Firefox/68.0
Accept: text/html,*/*
Accept-Language: en-US,en
Cookie: session_id=01234abcd
Referer: https://news.example.com/
```

When a viewer request like this one comes in to a CloudFront edge location, CloudFront uses the cache key to determine if there’s a cache hit\. By default, only the information shown in bold is included in the cache key\. If the requested object is not in the cache \(a cache miss\), then CloudFront sends a request to the origin to get the object\. After getting the object, CloudFront returns it to the viewer and stores it in the edge location’s cache\.

When CloudFront receives another request for the same object, as determined by the cache key, CloudFront serves the cached object to the viewer immediately, without sending a request to the origin\. For example, consider the following HTTP request that comes in after the previous request\.

```
GET /content/stories/example-story.html?ref=xyz987&split-pages=true HTTP/1.1
Host: d111111abcdef8.cloudfront.net
User-Agent: Mozilla/5.0 AppleWebKit/537.36 Chrome/83.0.4103.116
Accept: text/html,*/*
Accept-Language: en-US,en
Cookie: session_id=wxyz9876
Referer: https://rss.news.example.net/
```

This request is for the same object as the previous request, but is different from the previous request\. It has a different URL query string, different `User-Agent` and `Referer` headers, and a different `session_id` cookie\. However, none of these values are part of the cache key by default, so this second request results in a cache hit\.

## Customizing the cache key<a name="cache-key-custom"></a>

In some cases, you might want to include more information in the cache key, even though doing so might result in fewer cache hits\. You specify what to include in the cache key by using a [cache policy](controlling-the-cache-key.md)\.

For example, if your origin server uses the `Accept-Language` HTTP header in viewer requests to return different content based on the viewer’s language, you might want to include this header in the cache key\. When you do that, CloudFront uses this header to determine cache hits, and includes the header in *origin requests* \(requests that CloudFront sends to the origin when there’s a cache miss\)\.

One potential consequence of including additional values in the cache key is that CloudFront might end up caching duplicate objects because of the variation that can occur in viewer requests\. For example, viewers might send any of the following values for the `Accept-Language` header:
+ `en-US,en`
+ `en,en-US`
+ `en-US, en`
+ `en-US`

All of these different values indicate that the viewer’s language is English, but the variation can cause CloudFront to cache the same object multiple times\. This can reduce cache hits and increase the number of origin requests\. You could avoid this duplication by not including the `Accept-Language` header in the cache key, and instead configuring your website or application to use different URLs for content in different languages \(for example, `/en-US/content/stories/example-story.html`\)\.

For any given value that you intend to include in the cache key, you should make sure that you understand how many different variations of that value might appear in viewer requests\. For certain request values, it rarely makes sense to include them in the cache key\. For example, the `User-Agent` header can have thousands of unique variations, so it’s generally not a good candidate for including in the cache key\. Cookies that have user\-specific or session\-specific values and are unique across thousands \(or even millions\) of requests are also not good candidates for cache key inclusion\. If you do include these values in the cache key, each unique variation results in another copy of the object in the cache\. If these copies of the object are not unique, or if you end up with such a large number of slightly different objects that each object only gets a small number of cache hits, you might want to consider a different approach\. You can exclude these highly variable values from the cache key, or you can mark objects as non\-cacheable\.

Use caution when customizing the cache key\. Sometimes it’s desirable, but it can have unintended consequences such as caching duplicate objects, lowering your cache hit ratio, and increasing the number of origin requests\. If your origin website or application needs to receive certain values from viewer requests for analytics, telemetry, or other uses, but these values don’t change the object that the origin returns, use an [origin request policy](controlling-origin-requests.md) to include these values in origin requests but *not* include them in the cache key\.