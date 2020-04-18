# Caching Content Based on Request Headers<a name="header-caching"></a>

For web distributions, CloudFront lets you choose whether you want CloudFront to forward headers to your origin and to cache separate versions of a specified object based on the header values in viewer requests\. This allows you to serve different versions of your content based on the device the user is using, the location of the viewer, the language the viewer is using, and a variety of other criteria\. For RTMP distributions, you cannot configure CloudFront to cache based on header values\.

**Note**  
For RTMP distributions, you cannot configure CloudFront to cache your content based on the headers in viewer requests\.

**Topics**
+ [Headers and Distributions \- Overview](#header-caching-web)
+ [Selecting the Headers to Base Caching On](#header-caching-web-selecting)
+ [Configuring CloudFront to Respect CORS Settings](#header-caching-web-cors)
+ [Configuring Caching Based on the Device Type](#header-caching-web-device)
+ [Configuring Caching Based on the Language of the Viewer](#header-caching-web-language)
+ [Configuring Caching Based on the Location of the Viewer](#header-caching-web-location)
+ [Configuring Caching Based on the Protocol of the Request](#header-caching-web-protocol)
+ [Configuring Caching For Compressed Files](#header-caching-web-compressed)
+ [How Caching Based on Headers Affects Performance](#header-caching-web-performance)
+ [How the Case of Headers and Header Values Affects Caching](#header-caching-web-case)
+ [Headers that CloudFront Returns to the Viewer](#header-caching-web-response)

## Headers and Distributions \- Overview<a name="header-caching-web"></a>

By default, CloudFront doesn't consider headers when caching your objects in edge locations\. If your origin returns two objects and they differ only by the values in the request headers, CloudFront caches only one version of the object\.

You can configure CloudFront to forward headers to the origin, which causes CloudFront to cache multiple versions of an object based on the values in one or more request headers\. To configure CloudFront to cache objects based on the values of specific headers, you specify cache behavior settings for your distribution\. For more information, see [ Cache Based on Selected Request Headers](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesForwardHeaders)\.

 For example, suppose viewer requests for `logo.jpg` contain a custom `Product` header that has a value of either `Acme` or `Apex`\. When you configure CloudFront to cache your objects based on the value of the `Product` header, CloudFront forwards requests for `logo.jpg` to the origin and includes the `Product` header and header values\. CloudFront caches `logo.jpg` once for requests in which the value of the `Product` header is `Acme` and once for requests in which the value is `Apex`\.

You can configure each cache behavior in a web distribution to do one of the following: 
+ Forward all headers to your origin
**Important**  
If you configure CloudFront to forward all headers to your origin, CloudFront doesn't cache the objects associated with this cache behavior\. Instead, it sends every request to the origin\.
+ Forward a whitelist of headers that you specify\. CloudFront caches your objects based on the values in all of the specified headers\. CloudFront also forwards the headers that it forwards by default, but it caches your objects based only on the headers that you specify\. 
+ Forward only the default headers\. In this configuration, CloudFront doesn't cache your objects based on the values in the request headers\.

For the current quota on the number of headers that you can whitelist for each cache behavior or to request a higher quota, see [Quotas on Custom Headers \(Web Distributions Only\)](cloudfront-limits.md#limits-custom-headers)\.

For information about using the CloudFront console to update a distribution so CloudFront forwards headers to the origin, see [Updating a Distribution](HowToUpdateDistribution.md)\. For information about using the CloudFront API to update an existing distribution, see [Update Distribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.

## Selecting the Headers to Base Caching On<a name="header-caching-web-selecting"></a>

The headers that you can forward to the origin and that CloudFront bases caching on depend on whether your origin is an Amazon S3 bucket or a custom origin\.
+ **Amazon S3 – **You can configure CloudFront to forward and to cache your objects based on a number of specific headers \(see the following list of exceptions\)\. However, we recommend that you avoid whitelisting headers with an Amazon S3 origin unless you need to implement cross\-origin resource sharing \(CORS\) or you want to personalize content by using Lambda@Edge in origin\-facing events\.
  + To configure CORS, you must forward headers that allow CloudFront to distribute content for websites that are enabled for cross\-origin resource sharing \(CORS\)\. For more information, see [Configuring CloudFront to Respect CORS Settings](#header-caching-web-cors)\. 
  + To personalize content by using headers that you forward to your Amazon S3 origin, you write and add Lambda@Edge functions and associate them with your CloudFront distribution to be triggered by an origin\-facing event\. For more information about working with headers to personalize content, see [Personalize Content by Country or Device Type Headers \- Examples](lambda-examples.md#lambda-examples-redirecting-examples)\.

    We recommend that you avoid whitelisting headers that you aren’t using to personalize content because forwarding extra headers can reduce your cache hit ratio\. That is, CloudFront can’t serve as many requests from edge caches, as a proportion of all requests\.
+ **Custom origin ** – You can configure CloudFront to cache based on the value of any request header except the following:
  + `Connection`
  + `Cookie` – If you want to forward and cache based on cookies, you use a separate setting in your distribution\. For more information, see [Caching Content Based on Cookies](Cookies.md)\.
  + `Host (for Amazon S3 origins)`
  + `Proxy-Authorization`
  + `TE`
  + `Upgrade`

  You can configure CloudFront to cache objects based on values in the `Date` and `User-Agent` headers, but we don’t recommend it\. These headers have numerous possible values, and caching based on their values could cause CloudFront to forward significantly more requests to your origin\.

For a full list of HTTP request headers and how CloudFront processes them, see [HTTP Request Headers and CloudFront Behavior \(Custom and S3 Origins\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)\.

## Configuring CloudFront to Respect CORS Settings<a name="header-caching-web-cors"></a>

If you have enabled cross\-origin resource sharing \(CORS\) on an Amazon S3 bucket or a custom origin, you must choose specific headers to forward, to respect the CORS settings\. The headers that you must forward differ depending on the origin \(Amazon S3 or custom\) and whether you want to cache `OPTIONS` responses\.

**Amazon S3**
+ If you want `OPTIONS` responses to be cached, do the following:
  + Choose the options for default cache behavior settings that enable caching for `OPTIONS` responses\. 
  + Configure CloudFront to forward the following headers: `Origin`, `Access-Control-Request-Headers`, and `Access-Control-Request-Method`\.
+ If you don't want `OPTIONS` responses to be cached, configure CloudFront to forward the `Origin` header, together with any other headers required by your origin \(for example, `Access-Control-Request-Headers`, `Access-Control-Request-Method`, or others\)\.

**Custom origins** – Forward the `Origin` header along with any other headers required by your origin\.

You configure CloudFront to forward headers by whitelisting the headers in a cache behavior for your CloudFront distribution\. For more information about working with header forwarding, see [Headers and Distributions \- Overview](#header-caching-web)\.

For more information about CORS and Amazon S3, see [Enabling Cross\-Origin Resource Sharing](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Configuring Caching Based on the Device Type<a name="header-caching-web-device"></a>

If you want CloudFront to cache different versions of your objects based on the device a user is using to view your content, configure CloudFront to forward the applicable headers to your custom origin:
+ `CloudFront-Is-Desktop-Viewer`
+ `CloudFront-Is-Mobile-Viewer`
+ `CloudFront-Is-SmartTV-Viewer`
+ `CloudFront-Is-Tablet-Viewer`

Based on the value of the `User-Agent` header, CloudFront sets the value of these headers to `true` or `false` before forwarding the request to your origin\. If a device falls into more than one category, more than one value might be `true`\. For example, for some tablet devices, CloudFront might set both `CloudFront-Is-Mobile-Viewer` and `CloudFront-Is-Tablet-Viewer` to `true`\.

## Configuring Caching Based on the Language of the Viewer<a name="header-caching-web-language"></a>

If you want CloudFront to cache different versions of your objects based on the language specified in the request, configure CloudFront to forward the `Accept-Language` header to your origin\.

## Configuring Caching Based on the Location of the Viewer<a name="header-caching-web-location"></a>

If you want CloudFront to cache different versions of your objects based on the country that the request came from, configure CloudFront to forward the `CloudFront-Viewer-Country` header to your origin\. CloudFront automatically converts the IP address that the request came from into a two\-letter country code\. For an easy\-to\-use list of country codes, sortable by code and by country name, see the Wikipedia entry [ISO 3166\-1 alpha\-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)\.

## Configuring Caching Based on the Protocol of the Request<a name="header-caching-web-protocol"></a>

If you want CloudFront to cache different versions of your objects based on the protocol of the request, HTTP or HTTPS, configure CloudFront to forward the `CloudFront-Forwarded-Proto` header to your origin\.

## Configuring Caching For Compressed Files<a name="header-caching-web-compressed"></a>

If your origin supports brotli compression, you can whitelist the `Accept-Encoding` header and cache based on the header\. Configure caching based on `Accept-Encoding` only if your origin serves different content based on the header\.

## How Caching Based on Headers Affects Performance<a name="header-caching-web-performance"></a>

When you configure CloudFront to cache based on one or more headers and the headers have more than one possible value, CloudFront forwards more requests to your origin server for the same object\. This slows performance and increases the load on your origin server\. If your origin server returns the same object regardless of the value of a given header, we recommend that you don't configure CloudFront to cache based on that header\. 

If you configure CloudFront to forward more than one header, the order of the headers in viewer requests doesn't affect caching as long as the values are the same\. For example, if one request contains the headers A:1,B:2 and another request contains B:2,A:1, CloudFront caches just one copy of the object\.

## How the Case of Headers and Header Values Affects Caching<a name="header-caching-web-case"></a>

When CloudFront caches based on header values, it doesn't consider the case of the header name, but it does consider the case of the header value:
+ If viewer requests include both `Product:Acme` and `product:Acme`, CloudFront caches an object only once\. The only difference between them is the case of the header name, which doesn't affect caching\.
+ If viewer requests include both `Product:Acme` and `Product:acme`, CloudFront caches an object twice, because the value is `Acme` in some requests and `acme` in others\.

## Headers that CloudFront Returns to the Viewer<a name="header-caching-web-response"></a>

Configuring CloudFront to forward and cache headers does not affect which headers CloudFront returns to the viewer\. CloudFront returns all of the headers that it gets from the origin with a few exceptions\. For more information, see the applicable topic:
+ **Amazon S3 origins – ** See [HTTP Response Headers That CloudFront Removes or Updates](RequestAndResponseBehaviorS3Origin.md#response-s3-removed-headers)\.
+ **Custom origins – ** See [HTTP Response Headers that CloudFront Removes or Replaces](RequestAndResponseBehaviorCustomOrigin.md#ResponseCustomRemovedHeaders)\.