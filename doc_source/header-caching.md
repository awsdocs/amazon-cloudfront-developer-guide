# Configuring CloudFront to Cache Objects Based on Request Headers<a name="header-caching"></a>

For web distributions, CloudFront lets you choose whether you want CloudFront to forward headers to your origin and to cache separate versions of a specified object based on the header values in viewer requests\. This allows you to serve different versions of your content based on the device the user is using, the location of the viewer, the language the viewer is using, and a variety of other criteria\. For RTMP distributions, you cannot configure CloudFront to cache based on header values\.


+ [Headers and Web Distributions](#header-caching-web)
+ [Headers and RTMP Distributions](#header-caching-rtmp)

## Headers and Web Distributions<a name="header-caching-web"></a>

By default, CloudFront doesn't consider headers when caching your objects in edge locations\. If your origin returns two objects and they differ only by the values in the request headers, CloudFront caches only one version of the object\.

You can configure CloudFront to forward headers to the origin, which causes CloudFront to cache multiple versions of an object based on the values in one or more request headers\. For example, suppose viewer requests for `logo.jpg` contain a custom `Product` header that has a value of either `Acme` or `Apex`\. When you configure CloudFront to cache your objects based on the value of the `Product` header, CloudFront forwards requests for `logo.jpg` to the origin and includes the `Product` header and header values\. CloudFront caches `logo.jpg` once for requests in which the value of the `Product` header is `Acme` and once for requests in which the value is `Apex`\.

You can configure each cache behavior in a web distribution to do one of the following:

+ Forward all headers to your origin
**Important**  
If you configure CloudFront to forward all headers to your origin, CloudFront doesn't cache the objects associated with this cache behavior\. Instead, it sends every request to the origin\.

+ Forward a whitelist of headers that you specify\. CloudFront caches your objects based on the values in all of the specified headers\. CloudFront also forwards the headers that it forwards by default, but it caches your objects based only on the headers that you specify\. 

+ Forward only the default headers\. In this configuration, CloudFront doesn't cache your objects based on the values in the request headers\.

For the current limit on the number of headers that you can whitelist for each cache behavior, see [Amazon CloudFront Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_cloudfront) in the *Amazon Web Services General Reference*\. To request a higher limit, go to [https://console\.aws\.amazon\.com/support/home\#/case/create?issueType=service\-limit\-increase&limitType=service\-code\-cloudfront\-distributions](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.

For information about using the CloudFront console to update a distribution so CloudFront forwards headers to the origin, see [Listing, Viewing, and Updating CloudFront Distributions](HowToUpdateDistribution.md)\. For information about using the CloudFront API to update an existing web distribution, see [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) in the *Amazon CloudFront API Reference*\.


+ [Selecting the Headers on Which You Want CloudFront to Base Caching](#header-caching-web-selecting)
+ [Configuring CloudFront to Respect Cross\-Origin Resource Sharing \(CORS\) Settings](#header-caching-web-cors)
+ [Configuring CloudFront to Cache Objects Based on the Device Type](#header-caching-web-device)
+ [Configuring CloudFront to Cache Objects Based on the Language of the Viewer](#header-caching-web-language)
+ [Configuring CloudFront to Cache Objects Based on the Location of the Viewer](#header-caching-web-location)
+ [Configuring CloudFront to Cache Objects Based on the Protocol of the Request](#header-caching-web-protocol)
+ [How Caching Based on Headers Affects Performance](#header-caching-web-performance)
+ [How the Case of Headers and Header Values Affects Caching](#header-caching-web-case)
+ [Headers that CloudFront Returns to the Viewer](#header-caching-web-response)

### Selecting the Headers on Which You Want CloudFront to Base Caching<a name="header-caching-web-selecting"></a>

The headers that you can forward to the origin and that CloudFront bases caching on depend on whether you're using an Amazon S3 bucket or a custom origin\.

+ **Amazon S3 – **You can configure CloudFront to forward and to cache your objects based only on three headers: `Access-Control-Request-Headers`, `Access-Control-Request-Method`, and `Origin`\. Forwarding these headers allows CloudFront to distribute content for websites that are enabled for cross\-origin resource sharing \(CORS\)\. You can't configure CloudFront to forward custom headers to Amazon S3\. For more information, see [Configuring CloudFront to Respect Cross\-Origin Resource Sharing \(CORS\) Settings](#header-caching-web-cors)\.

+ **Custom origin ** – You can configure CloudFront to cache based on the value of any request header except the following:

  + `Accept-Encoding`

  + `Connection`

  + `Cookie` – If you want to forward and cache based on cookies, you use a separate setting in your distribution\. For more information, see [Configuring CloudFront to Cache Objects Based on Cookies](Cookies.md)\.

  + `Proxy-Authorization`

  + `TE`

  + `Upgrade`

  You can configure CloudFront to cache objects based on values in the `Date` and `User-Agent` headers, but we don't recommend it\. These headers have a lot of possible values, and caching based on their values would cause CloudFront to forward significantly more requests to your origin\. 

For a full list of HTTP request headers and how CloudFront processes them, see the applicable topic:

+ **Amazon S3 origin** – [HTTP Request Headers That CloudFront Removes or Updates](RequestAndResponseBehaviorS3Origin.md#request-s3-removed-headers)

+ **Custom origin** – [HTTP Request Headers and CloudFront Behavior](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)

### Configuring CloudFront to Respect Cross\-Origin Resource Sharing \(CORS\) Settings<a name="header-caching-web-cors"></a>

If you enabled cross\-origin resource sharing \(CORS\) on an Amazon S3 bucket or a custom origin, you can configure CloudFront to respect the CORS settings\. Configure CloudFront to forward a whitelist of headers and include the applicable headers in the list of headers to forward, depending on whether you're using Amazon S3 or a custom origin:

**Amazon S3**

+ When you want `OPTIONS` responses to be cached, configure CloudFront to forward the following headers: `Origin`, `Access-Control-Request-Headers`, and `Access-Control-Request-Method`\.

+ When you don't want `OPTIONS` responses to be cached, configure CloudFront to forward the `Origin` header\. You can also configure CloudFront to forward `Access-Control-Request-Headers` and `Access-Control-Request-Method` headers, but it's not required\.

**Custom origins** – Forward the `Origin` header along with any other headers required by your origin\.

For more information about CORS and Amazon S3, see [Enabling Cross\-Origin Resource Sharing](http://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the *Amazon Simple Storage Service Developer Guide*\.

### Configuring CloudFront to Cache Objects Based on the Device Type<a name="header-caching-web-device"></a>

If you want CloudFront to cache different versions of your objects based on the device a user is using to view your content, configure CloudFront to forward the applicable headers to your custom origin:

+ `CloudFront-Is-Desktop-Viewer`

+ `CloudFront-Is-Mobile-Viewer`

+ `CloudFront-Is-SmartTV-Viewer`

+ `CloudFront-Is-Tablet-Viewer`

Based on the value of the `User-Agent` header, CloudFront sets the value of these headers to `true` or `false` before forwarding the request to your origin\. If a device falls into more than one category, more than one value might be `true`\. For example, for some tablet devices, CloudFront might set both `CloudFront-Is-Mobile-Viewer` and `CloudFront-Is-Tablet-Viewer` to `true`\.

### Configuring CloudFront to Cache Objects Based on the Language of the Viewer<a name="header-caching-web-language"></a>

If you want CloudFront to cache different versions of your objects based on the language specified in the request, program your application to include the language in the `Accept-Language` header, and configure CloudFront to forward the `Accept-Language` header to your origin\.

### Configuring CloudFront to Cache Objects Based on the Location of the Viewer<a name="header-caching-web-location"></a>

If you want CloudFront to cache different versions of your objects based on the country that the request came from, configure CloudFront to forward the `CloudFront-Viewer-Country` header to your origin\. CloudFront automatically converts the IP address that the request came from into a two\-letter country code\. For an easy\-to\-use list of country codes, sortable by code and by country name, see the Wikipedia entry [ISO 3166\-1 alpha\-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)\.

### Configuring CloudFront to Cache Objects Based on the Protocol of the Request<a name="header-caching-web-protocol"></a>

If you want CloudFront to cache different versions of your objects based on the protocol of the request, HTTP or HTTPS, configure CloudFront to forward the `CloudFront-Forwarded-Proto` header to your origin\.

### How Caching Based on Headers Affects Performance<a name="header-caching-web-performance"></a>

When you configure CloudFront to cache based on one or more headers and the headers have more than one possible value, CloudFront forwards more requests to your origin server for the same object\. This slows performance and increases the load on your origin server\. If your origin server returns the same object regardless of the value of a given header, we recommend that you don't configure CloudFront to cache based on that header\. 

If you configure CloudFront to forward more than one header, the order of the headers in viewer requests doesn't affect caching as long as the values are the same\. For example, if one request contains the headers A:1,B:2 and another request contains B:2,A:1, CloudFront caches just one copy of the object\.

### How the Case of Headers and Header Values Affects Caching<a name="header-caching-web-case"></a>

When CloudFront caches based on header values, it doesn't consider the case of the header name, but it does consider the case of the header value:

+ If viewer requests include both `Product:Acme` and `product:Acme`, CloudFront caches an object only once\. The only difference between them is the case of the header name, which doesn't affect caching\.

+ If viewer requests include both `Product:Acme` and `Product:acme`, CloudFront caches an object twice, because the value is `Acme` in some requests and `acme` in others\.

### Headers that CloudFront Returns to the Viewer<a name="header-caching-web-response"></a>

Configuring CloudFront to forward and cache headers does not affect which headers CloudFront returns to the viewer\. CloudFront returns all of the headers that it gets from the origin with a few exceptions\. For more information, see the applicable topic:

+ **Amazon S3 origins – ** See [HTTP Response Headers That CloudFront Removes or Updates](RequestAndResponseBehaviorS3Origin.md#response-s3-removed-headers)\.

+ **Custom origins – ** See [HTTP Response Headers that CloudFront Removes or Updates](RequestAndResponseBehaviorCustomOrigin.md#ResponseCustomRemovedHeaders)\.

## Headers and RTMP Distributions<a name="header-caching-rtmp"></a>

For RTMP distributions, you cannot configure CloudFront to cache your content based on the headers in viewer requests\. 