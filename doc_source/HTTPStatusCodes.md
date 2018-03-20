# How CloudFront Processes and Caches HTTP 4xx and 5xx Status Codes from Your Origin<a name="HTTPStatusCodes"></a>

**Topics**
+ [How CloudFront Processes Errors When You Have Configured Custom Error Pages](#HTTPStatusCodes-custom-error-pages)
+ [How CloudFront Processes Errors When You Have Not Configured Custom Error Pages](#HTTPStatusCodes-no-custom-error-pages)
+ [HTTP 4xx and 5xx Status Codes that CloudFront Caches](#HTTPStatusCodes-cached-errors)

When CloudFront requests an object from your Amazon S3 bucket or custom origin server, your origin sometimes returns an HTTP 4xx or 5xx status code, which indicates an error has occurred\. CloudFront behavior depends on:
+ Whether you have configured custom error pages\.
+ Whether you have configured how long you want CloudFront to cache error responses from your origin \(error caching minimum TTL\)\.
+ The status code\.
+ For 5xx status codes, whether the requested object is currently in the CloudFront edge cache\.

CloudFront always caches responses to `GET` and `HEAD` requests\. You can also configure CloudFront to cache responses to `OPTIONS` requests\. CloudFront does not cache responses to requests that use the other methods\.

For information about settings for custom error pages in the CloudFront console, see [Custom Error Pages and Error Caching](distribution-web-values-specify.md#DownloadDistValuesErrorPages)\. For information about the error caching minimum TTL in the CloudFront console, see [Error Caching Minimum TTL](distribution-web-values-specify.md#DownloadDistValuesErrorCachingMinTTL)\.

For a list of the HTTP status codes that CloudFront caches, see [HTTP 4xx and 5xx Status Codes that CloudFront Caches](#HTTPStatusCodes-cached-errors)\.

If you have enabled logging, CloudFront writes the results to the logs regardless of the HTTP status code\.

## How CloudFront Processes Errors When You Have Configured Custom Error Pages<a name="HTTPStatusCodes-custom-error-pages"></a>

If you have configured custom error pages, CloudFront behavior depends on whether the requested object is in the edge cache\.

### The Requested Object Is Not in the Edge Cache<a name="HTTPStatusCodes-custom-error-pages-not-in-cache"></a>

CloudFront continues to try to get the requested object from your origin when all of the following are true:
+ A viewer requests an object
+ The object isn't in the edge cache
+ Your origin returns an HTTP 4xx or 5xx status code instead of returning a 304 status code \(Not Modified\) or an updated version of the object

CloudFront does the following:

1. In the CloudFront edge cache that received the viewer request, CloudFront checks your distribution configuration and gets the path of the custom error page that corresponds with the status code that your origin returned\. 

1. CloudFront finds the first cache behavior in your distribution that has a path pattern that matches the path of the custom error page\.

1. The CloudFront edge location sends a request for the custom error page to the origin that is specified in the cache behavior\.

1. The origin returns the custom error page to the edge location\.

1. CloudFront returns the custom error page to the viewer that made the request, and also caches the custom error page for the amount of time specified by the error caching minimum TTL \(five minutes by default\)\.

1. After the error caching minimum TTL has elapsed, CloudFront tries again to get the requested object by forwarding another request to your origin\. CloudFront continues to retry at intervals specified by the error caching minimum TTL\. 

### The Requested Object Is in the Edge Cache<a name="HTTPStatusCodes-custom-error-pages-in-cache"></a>

CloudFront continues to serve the object that is currently in the edge cache when all of the following are true:
+ A viewer requests an object
+ The object is in the edge cache but it has expired
+ Your origin returns an HTTP 5xx status code instead of returning a 304 status code \(Not Modified\) or an updated version of the object

CloudFront does the following:

1. If your origin returns a 5xx status code, CloudFront serves the object even though it has expired\. For the duration of the error caching minimum TTL, CloudFront continues to respond to viewer requests by serving the object from the edge cache\. 

   If your origin returns a 4xx status code, CloudFront returns the status code, not the requested object, to the viewer\. 

1. After the error caching minimum TTL has elapsed, CloudFront tries again to get the requested object by forwarding another request to your origin\. Note that if the object is not requested frequently, CloudFront might evict it from the edge cache while your origin server is still returning 5xx responses\. For information about how long objects stay in CloudFront edge caches, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

## How CloudFront Processes Errors When You Have Not Configured Custom Error Pages<a name="HTTPStatusCodes-no-custom-error-pages"></a>

If you have not configured custom error pages, CloudFront behavior depends on whether the requested object is in the edge cache\.

### The Requested Object Is Not in the Edge Cache<a name="HTTPStatusCodes-no-custom-error-pages-not-in-cache"></a>

CloudFront continues to try to get the requested object from your origin when all of the following are true:
+ A viewer requests an object
+ The object isn't in the edge cache
+ Your origin returns an HTTP 4xx or 5xx status code instead of returning a 304 status code \(Not Modified\) or an updated version of the object

CloudFront does the following:

1. CloudFront returns the 4xx or 5xx status code to the viewer\.

1. CloudFront also caches the status code in the edge cache that received the request\.

1. For the duration of the error caching minimum TTL \(five minutes by default\), CloudFront responds to subsequent viewer requests for the same object with the cached 4xx or 5xx status code\. 

1. After the error caching minimum TTL has elapsed, CloudFront tries again to get the requested object by forwarding another request to your origin\.

### The Requested Object Is in the Edge Cache<a name="HTTPStatusCodes-no-custom-error-pages-in-cache"></a>

CloudFront continues to serve the object that is currently in the edge cache when all of the following are true:
+ A viewer requests an object
+ The object is in the edge cache but it has expired
+ Your origin returns an HTTP 5xx status code instead of returning a 304 status code \(Not Modified\) or an updated version of the object

CloudFront does the following:

1. If your origin returns a 5xx error code, CloudFront serves the object even though it has expired\. For the duration of the error caching minimum TTL \(five minutes by default\), CloudFront continues to respond to viewer requests by serving the object from the edge cache\. 

   If your origin returns a 4xx status code, CloudFront returns the status code, not the requested object, to the viewer\. 

1. After the error caching minimum TTL has elapsed, CloudFront tries again to get the requested object by forwarding another request to your origin\. Note that if the object is not requested frequently, CloudFront might evict it from the edge cache while your origin server is still returning 5xx responses\. For information about how long objects stay in CloudFront edge caches, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

## HTTP 4xx and 5xx Status Codes that CloudFront Caches<a name="HTTPStatusCodes-cached-errors"></a>

CloudFront caches the following HTTP 4xx and 5xx status codes returned by Amazon S3 or your custom origin server\. If you have configured a custom error page for an HTTP status code, CloudFront caches the custom error page\. 


****  

|  |  | 
| --- |--- |
| 400 | Bad Request | 
| 403 | Forbidden | 
| 404 | Not Found | 
| 405 | Method Not Allowed | 
| 414 | Request\-URI Too Large | 
| 500 | Internal Server Error | 
| 501 | Not Implemented | 
| 502 | Bad Gateway | 
| 503 | Service Unavailable | 
| 504 | Gateway Time\-out | 