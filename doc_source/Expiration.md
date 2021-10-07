# Managing how long content stays in the cache \(expiration\)<a name="Expiration"></a>

You can control how long your files stay in a CloudFront cache before CloudFront forwards another request to your origin\. Reducing the duration allows you to serve dynamic content\. Increasing the duration means that your users get better performance because your files are more likely to be served directly from the edge cache\. A longer duration also reduces the load on your origin\.

Typically, CloudFront serves a file from an edge location until the cache duration that you specified passes—that is, until the file expires\. After it expires, the next time the edge location gets a user request for the file, CloudFront forwards the request to the origin server to verify that the cache contains the latest version of the file\. The response from the origin depends on whether the file has changed:
+ If the CloudFront cache already has the latest version, the origin returns a status code `304 Not Modified`\.
+ If the CloudFront cache does not have the latest version, the origin returns a status code `200 OK` and the latest version of the file\.

If a file in an edge location isn't frequently requested, CloudFront might evict the file—remove the file before its expiration date—to make room for files that have been requested more recently\.

By default, each file automatically expires after 24 hours, but you can change the default behavior in two ways:
+ To change the cache duration for all files that match the same path pattern, you can change the CloudFront settings for **Minimum TTL**, **Maximum TTL**, and **Default TTL** for a cache behavior\. For information about the individual settings, see [Minimum TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesMinTTL), [Maximum TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesMaxTTL), and [Default TTL](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesDefaultTTL) in [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\. To use these settings, you must choose the **Customize** option for the **Object Caching** setting when you create or update your CloudFront distribution\. For more information, see [Object Caching](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesObjectCaching) in [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\. 
+ To change the cache duration for an individual file, you can configure your origin to add a `Cache-Control max-age` or `Cache-Control s-maxage` directive, or an `Expires` header field to the file\. For more information, see [Using headers to control cache duration for individual objects](#expiration-individual-objects)\.

For more information about how **Minimum TTL**, **Default TTL**, and **Maximum TTL** interact with `Cache-Control max-age` and `Cache-Control s-maxage` directives and the `Expires` header field, see [Specifying the amount of time that CloudFront caches objects](#ExpirationDownloadDist)\.

You can also control how long errors \(for example, `404, Not Found`\) stay in a CloudFront cache before CloudFront tries again to get the requested object by forwarding another request to your origin\. For more information, see [How CloudFront Processes and Caches HTTP 4xx and 5xx Status Codes from Your Origin](HTTPStatusCodes.md)\.

**Topics**
+ [Using headers to control cache duration for individual objects](#expiration-individual-objects)
+ [Specifying the amount of time that CloudFront caches objects](#ExpirationDownloadDist)
+ [Adding headers to your objects using the Amazon S3 console](#ExpirationAddingHeadersInS3)

## Using headers to control cache duration for individual objects<a name="expiration-individual-objects"></a>

You can use the `Cache-Control` and `Expires` headers to control how long objects stay in the cache\. Settings for **Minimum TTL**, **Default TTL**, and **Maximum TTL ** also affect cache duration, but here's an overview of how headers can affect cache duration: 
+ The `Cache-Control max-age` directive lets you specify how long \(in seconds\) that you want an object to remain in the cache before CloudFront gets the object again from the origin server\. The minimum expiration time CloudFront supports is 0 seconds\. The maximum value is 100 years\. Specify the value in the following format:

  `Cache-Control: max-age=`*seconds*

  For example, the following directive tells CloudFront to keep the associated object in the cache for 3600 seconds \(one hour\):

  `Cache-Control: max-age=3600`

  If you want objects to stay in CloudFront edge caches for a different duration than they stay in browser caches, you can use the `Cache-Control max-age` and `Cache-Control s-maxage` directives together\. For more information, see [Specifying the amount of time that CloudFront caches objects](#ExpirationDownloadDist)\.
+ The `Expires` header field lets you specify an expiration date and time using the format specified in [RFC 2616, Hypertext Transfer Protocol \-\- HTTP/1\.1 Section 3\.3\.1, Full Date](http://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.3.1), for example:

  `Sat, 27 Jun 2015 23:59:59 GMT`

We recommend that you use the `Cache-Control max-age` directive instead of the `Expires` header field to control object caching\. If you specify values both for `Cache-Control max-age` and for `Expires`, CloudFront uses only the value of `Cache-Control max-age`\.

For more information, see [Specifying the amount of time that CloudFront caches objects](#ExpirationDownloadDist)\.

You cannot use the HTTP `Cache-Control` or `Pragma` header fields in a `GET` request from a viewer to force CloudFront to go back to the origin server for the object\. CloudFront ignores those header fields in viewer requests\.

For more information about the `Cache-Control` and `Expires` header fields, see the following sections in *RFC 2616, Hypertext Transfer Protocol \-\- HTTP/1\.1*: 
+ [Section 14\.9 Cache Control](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
+ [Section 14\.21 Expires](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.21)

For an example of how to add `Cache-Control` and `Expires` header fields using the AWS SDK for PHP, see [Upload an Object Using the AWS SDK for PHP](https://docs.aws.amazon.com/AmazonS3/latest/dev/UploadObjSingleOpPHP.html) in the *Amazon Simple Storage Service User Guide*\. Some third\-party tools are also able to add these fields\.

## Specifying the amount of time that CloudFront caches objects<a name="ExpirationDownloadDist"></a>

To control the amount of time that CloudFront keeps an object in the cache before sending another request to the origin, you can:
+ Set the minimum, maximum, and default TTL values in a CloudFront distribution’s cache behavior\. You can set these values in a [cache policy](controlling-the-cache-key.md) attached to the cache behavior \(recommended\), or in the legacy cache settings\.
+ Include the `Cache-Control` or `Expires` headers in responses from the origin\. These headers also help determine how long a browser keeps an object in the browser cache before sending another request to CloudFront\.

The following table explains how the `Cache-Control` and `Expires` headers sent from the origin work together with the TTL settings in a cache behavior to affect caching\.


****  

| Origin headers | Minimum TTL = 0 seconds | Minimum TTL > 0 seconds | 
| --- | --- | --- | 
|  **The origin adds a `Cache-Control: max-age` directive to the object**  |  **CloudFront caching** CloudFront caches the object for the lesser of the value of the `Cache-Control: max-age` directive or the value of the CloudFront maximum TTL\. **Browser caching** Browsers cache the object for the value of the `Cache-Control: max-age` directive\.  |  **CloudFront caching** CloudFront caching depends on the values of the CloudFront minimum TTL and maximum TTL and the `Cache-Control max-age` directive: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) **Browser caching** Browsers cache the object for the value of the `Cache-Control: max-age` directive\.  | 
|  **The origin does not add a `Cache-Control: max-age` directive to the object**  |  **CloudFront caching** CloudFront caches the object for the value of the CloudFront default TTL\. **Browser caching** Depends on the browser\.  |  **CloudFront caching** CloudFront caches the object for the greater of the value of the CloudFront minimum TTL or default TTL\. **Browser caching** Depends on the browser\.  | 
|  **The origin adds `Cache-Control: max-age` and `Cache-Control: s-maxage` directives to the object**  |  **CloudFront caching** CloudFront caches the object for the lesser of the value of the `Cache-Control: s-maxage` directive or the value of the CloudFront maximum TTL\. **Browser caching** Browsers cache the object for the value of the `Cache-Control max-age` directive\.  |  **CloudFront caching** CloudFront caching depends on the values of the CloudFront minimum TTL and maximum TTL and the `Cache-Control: s-maxage` directive: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) **Browser caching** Browsers cache the object for the value of the `Cache-Control: max-age` directive\.  | 
|  **The origin adds an `Expires` header to the object**  |  **CloudFront caching** CloudFront caches the object until the date in the `Expires` header or for the value of the CloudFront maximum TTL, whichever is sooner\. **Browser caching** Browsers cache the object until the date in the `Expires` header\.  |  **CloudFront caching** CloudFront caching depends on the values of the CloudFront minimum TTL and maximum TTL and the `Expires` header: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html) **Browser caching** Browsers cache the object until the date and time in the `Expires` header\.  | 
|  **Origin adds `Cache-Control: no-cache`, `no-store`, and/or `private` directives to the object**  |  CloudFront and browsers respect the headers\. For an exception to how CloudFront handles the `Cache-Control: no-cache` header, see [Simultaneous Requests for the Same Object \(Traffic Spikes\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-traffic-spikes)\.  |  **CloudFront caching** CloudFront caches the object for the value of the CloudFront minimum TTL\. [See the warning below this table\.](#stale-if-error) **Browser caching** Browsers respect the headers\.  | 

**Warning**  
If CloudFront gets an object from the origin that includes the `Cache-Control: no-cache`, `no-store`, and/or `private` directives, and then later CloudFront gets another viewer request for the same object, CloudFront tries to contact the origin to fulfill the viewer request\.  
If the origin is reachable, CloudFront gets the object from the origin and returns it to the viewer\.  
If the origin is unreachable *and* minimum TTL is greater than 0, CloudFront serves the object that it got from the origin previously\. To avoid this behavior, include the `Cache-Control: stale-if-error=0` directive with the object returned from the origin\. This causes CloudFront to return an error in response to future requests if the origin is unreachable, rather than returning the object that it got from the origin previously\.

For information about how to change settings for distributions using the CloudFront console, see [Updating a Distribution](HowToUpdateDistribution.md)\. For information about how to change settings for distributions using the CloudFront API, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.

## Adding headers to your objects using the Amazon S3 console<a name="ExpirationAddingHeadersInS3"></a>

**To add a `Cache-Control` or `Expires` header field to Amazon S3 objects using the Amazon S3 console**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the list of buckets, choose the name of the bucket that contains the files that you are adding headers to\.

1. Select the check box next to the name of the file or folder that you are adding headers to\. When you add headers to a folder, it affects all the files inside that folder\.

1. Choose **Actions**, then choose **Edit metadata**\.

1. In the **Add metadata** panel, do the following:

   1. Choose **Add metadata**\.

   1. For **Type**, choose **System defined**\.

   1. For **Key**, choose the name of the header that you are adding \(**Cache\-Control** or **Expires**\)\.

   1. For **Value**, enter a header value\. For example, for a `Cache-Control` header, you could enter `max-age=86400`\. For `Expires`, you could enter an expiration date and time such as `Wed, 30 Jun 2021 09:28:00 GMT`\.

1. At the bottom of the page, choose **Edit metadata**\.