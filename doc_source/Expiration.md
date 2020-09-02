# Managing How Long Content Stays in an Edge Cache \(Expiration\)<a name="Expiration"></a>

You can control how long your files stay in a CloudFront cache before CloudFront forwards another request to your origin\. Reducing the duration allows you to serve dynamic content\. Increasing the duration means that your users get better performance because your files are more likely to be served directly from the edge cache\. A longer duration also reduces the load on your origin\.

Typically, CloudFront serves a file from an edge location until the cache duration that you specified passes—that is, until the file expires\. After it expires, the next time the edge location gets a user request for the file, CloudFront forwards the request to the origin server to verify that the cache contains the latest version of the file\. The response from the origin depends on whether the file has changed:
+ If the CloudFront cache already has the latest version, the origin returns a status code `304 Not Modified`\.
+ If the CloudFront cache does not have the latest version, the origin returns a status code `200 OK` and the latest version of the file\.

If a file in an edge location isn't frequently requested, CloudFront might evict the file—remove the file before its expiration date—to make room for files that have been requested more recently\.

By default, each file automatically expires after 24 hours, but you can change the default behavior in two ways:
+ To change the cache duration for all files that match the same path pattern, you can change the CloudFront settings for **Minimum TTL**, **Maximum TTL**, and **Default TTL** for a cache behavior\. For information about the individual settings, see [Minimum TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesMinTTL), [Maximum TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesMaxTTL), and [Default TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesDefaultTTL) in [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\. To use these settings, you must choose the **Customize** option for the **Object Caching** setting when you create or update your CloudFront distribution\. For more information, see [Object Caching](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesObjectCaching) in [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\. 
+ To change the cache duration for an individual file, you can configure your origin to add a `Cache-Control max-age` or `Cache-Control s-maxage` directive, or an `Expires` header field to the file\. For more information, see [Using Headers to Control Cache Duration for Individual Objects](#expiration-individual-objects)\.

For more information about how **Minimum TTL**, **Default TTL**, and **Maximum TTL** interact with `Cache-Control max-age` and `Cache-Control s-maxage` directives and the `Expires` header field, see [Specifying the Amount of Time that CloudFront Caches Objects for Web Distributions](#ExpirationDownloadDist)\.

You can also control how long errors \(for example, `404, Not Found`\) stay in a CloudFront cache before CloudFront tries again to get the requested object by forwarding another request to your origin\. For more information, see [How CloudFront Processes and Caches HTTP 4xx and 5xx Status Codes from Your Origin](HTTPStatusCodes.md)\.

**Topics**
+ [Using Headers to Control Cache Duration for Individual Objects](#expiration-individual-objects)
+ [Specifying the Amount of Time that CloudFront Caches Objects for Web Distributions](#ExpirationDownloadDist)
+ [Specifying the Minimum Time that CloudFront Caches Objects for RTMP Distributions](#expiration-rtmp-distribution)
+ [Adding Headers to Your Objects Using the Amazon S3 Console](#ExpirationAddingHeadersInS3)

## Using Headers to Control Cache Duration for Individual Objects<a name="expiration-individual-objects"></a>

You can use the `Cache-Control` and `Expires` headers to control how long objects stay in the cache\. Settings for **Minimum TTL**, **Default TTL**, and **Maximum TTL ** also affect cache duration, but here's an overview of how headers can affect cache duration: 
+ The `Cache-Control max-age` directive lets you specify how long \(in seconds\) that you want an object to remain in the cache before CloudFront gets the object again from the origin server\. The minimum expiration time CloudFront supports is 0 seconds for web distributions and 3600 seconds for RTMP distributions\. The maximum value is 100 years\. Specify the value in the following format:

  `Cache-Control: max-age=`*seconds*

  For example, the following directive tells CloudFront to keep the associated object in the cache for 3600 seconds \(one hour\):

  `Cache-Control: max-age=3600`

  If you want objects to stay in CloudFront edge caches for a different duration than they stay in browser caches, you can use the `Cache-Control max-age` and `Cache-Control s-maxage` directives together\. For more information, see [Specifying the Amount of Time that CloudFront Caches Objects for Web Distributions](#ExpirationDownloadDist)\.
+ The `Expires` header field lets you specify an expiration date and time using the format specified in [RFC 2616, Hypertext Transfer Protocol \-\- HTTP/1\.1 Section 3\.3\.1, Full Date](http://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.3.1), for example:

  `Sat, 27 Jun 2015 23:59:59 GMT`

We recommend that you use the `Cache-Control max-age` directive instead of the `Expires` header field to control object caching\. If you specify values both for `Cache-Control max-age` and for `Expires`, CloudFront uses only the value of `Cache-Control max-age`\.

For more information, see [Specifying the Amount of Time that CloudFront Caches Objects for Web Distributions](#ExpirationDownloadDist)\.

You cannot use the HTTP `Cache-Control` or `Pragma` header fields in a `GET` request from a viewer to force CloudFront to go back to the origin server for the object\. CloudFront ignores those header fields in viewer requests\.

For more information about the `Cache-Control` and `Expires` header fields, see the following sections in *RFC 2616, Hypertext Transfer Protocol \-\- HTTP/1\.1*: 
+ [Section 14\.9 Cache Control](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
+ [Section 14\.21 Expires](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.21)

For an example of how to add `Cache-Control` and `Expires` header fields using the AWS SDK for PHP, see [Upload an Object Using the AWS SDK for PHP](https://docs.aws.amazon.com/AmazonS3/latest/dev/UploadObjSingleOpPHP.html) in the *Amazon Simple Storage Service Developer Guide*\. Some third\-party tools are also able to add these fields\.

## Specifying the Amount of Time that CloudFront Caches Objects for Web Distributions<a name="ExpirationDownloadDist"></a>

For web distributions, you can use `Cache-Control` or `Expires` headers, and CloudFront minimum, maximum, and default TTL values to control the amount of time in seconds that CloudFront keeps an object in the cache before forwarding another request to the origin\. Header values also determine how long a browser keeps an object in the cache before forwarding another request to CloudFront\. 

**Important**  
If you configure CloudFront to forward all headers to your origin for a cache behavior, CloudFront never caches the associated objects\. Instead, CloudFront forwards all requests for those objects to the origin\. In that configuration, the value of minimum TTL must be 0\. For more information, see [Caching Content Based on Request Headers](header-caching.md)\.

To specify values for [Minimum TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesMinTTL), [Maximum TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesMaxTTL), and [Default TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesDefaultTTL), you must choose the **Customize** option for the [Object Caching](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesObjectCaching) setting\.


****  

| Origin Configuration | Minimum TTL = 0 Seconds | Minimum TTL > 0 Seconds | 
| --- | --- | --- | 
|  **The origin adds a `Cache-Control max-age` directive to objects**  |  **CloudFront caching** CloudFront caches objects for the lesser of the value of the `Cache-Control max-age` directive or the value of the CloudFront maximum TTL\. **Browser caching** Browsers cache objects for the value of the `Cache-Control max-age` directive\.  |  **CloudFront caching** CloudFront caching depends on the values of the CloudFront minimum TTL and maximum TTL and the `Cache-Control max-age` directive: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) **Browser caching** Browsers cache objects for the value of the `Cache-Control max-age` directive\.  | 
|  **The origin does not add a `Cache-Control max-age` directive to objects**  |  **CloudFront caching** CloudFront caches objects for the value of the CloudFront default TTL\. **Browser caching** Depends on the browser\.  |  **CloudFront caching** CloudFront caches objects for the greater of the value of the CloudFront minimum TTL or default TTL\. **Browser caching** Depends on the browser\.  | 
|  **The origin adds `Cache-Control max-age` and `Cache-Control s-maxage` directives to objects**  |  **CloudFront caching** CloudFront caches objects for the lesser of the value of the `Cache-Control s-maxage` directive or the value of the CloudFront maximum TTL\. **Browser caching** Browsers cache objects for the value of the `Cache-Control max-age` directive\.  |  **CloudFront caching** CloudFront caching depends on the values of the CloudFront minimum TTL and maximum TTL and the `Cache-Control s-maxage` directive: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) **Browser caching** Browsers cache objects for the value of the `Cache-Control max-age` directive\.  | 
|  **The origin adds an `Expires` header to objects**  |  **CloudFront caching** CloudFront caches objects until the date in the `Expires` header or for the value of the CloudFront maximum TTL, whichever is sooner\. **Browser caching** Browsers cache objects until the date in the `Expires` header\.  |  **CloudFront caching** CloudFront caching depends on the values of the CloudFront minimum TTL and maximum TTL and the `Expires` header: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) **Browser caching** Browsers cache objects until the date and time in the `Expires` header\.  | 
|  **Origin adds `Cache-Control: no-cache`, `no-store`, and/or `private` directives to objects**  |  CloudFront and browsers respect the headers\. For an exception to how CloudFront handles the `Cache-Control: no-cache` header, see [Simultaneous Requests for the Same Object \(Traffic Spikes\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-traffic-spikes)\.  |  **CloudFront caching** CloudFront caches objects for the value of the CloudFront minimum TTL\. **Browser caching** Browsers respect the headers\.  | 

For information about how to change settings for web distributions using the CloudFront console, see [Updating a Distribution](HowToUpdateDistribution.md)\. For information about how to change settings for web distributions using the CloudFront API, see [PUT Config](https://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html)\. 

## Specifying the Minimum Time that CloudFront Caches Objects for RTMP Distributions<a name="expiration-rtmp-distribution"></a>

For RTMP distributions, CloudFront keeps objects in edge caches for 24 hours by default\. You can add `Cache-Control` or `Expires` headers to your objects to change the amount of time that CloudFront keeps objects in edge caches before it forwards another request to the origin\. The minimum duration is 3600 seconds \(one hour\)\. If you specify a lower value, CloudFront uses 3600 seconds\.

## Adding Headers to Your Objects Using the Amazon S3 Console<a name="ExpirationAddingHeadersInS3"></a>

**Note**  
On the Amazon S3 console, you can only add headers to one object at a time, but with some third\-party tools, you can add headers to multiple Amazon S3 objects at a time\. For more information about third\-party tools that support Amazon S3, search on the web for **AWS S3 third party tools**\.

**To add a `Cache-Control` or `Expires` header field to Amazon S3 objects using the Amazon S3 console**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3](https://console.aws.amazon.com/s3)\.

1. In the Amazon S3 console, in the **Bucket name** list, choose the name of the bucket that contains the files\.

1. In the **Name** list, choose the name of the object that you want to add a header to\.

1. Choose **Properties**, and then choose **Metadata**\.

1. Choose **Add Metadata**, and then in the **Key** menu, choose **Cache\-Control** or **Expires**\. 

1. In the **Value** field, type one of the following:
   + For a `Cache-Control` field, type:

     `max-age=number of seconds that you want objects to stay in a CloudFront edge cache`
   + For an **Expires** field, type a date and time in HTML format\.

1. Choose **Save**\.