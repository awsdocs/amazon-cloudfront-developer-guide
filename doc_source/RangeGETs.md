# How CloudFront processes partial requests for an object \(range GETs\)<a name="RangeGETs"></a>

For a large object, the viewer \(web browser or other client\) can make multiple `GET` requests and use the `Range` request header to download the object in smaller parts\. These requests for ranges of bytes, sometimes known as `Range GET` requests, improve the efficiency of partial downloads and the recovery from partially failed transfers\. 

When CloudFront receives a `Range GET` request, it checks the cache in the edge location that received the request\. If the cache in that edge location already contains the entire object or the requested part of the object, CloudFront immediately serves the requested range from the cache\.

If the cache doesn’t contain the requested range, CloudFront forwards the request to the origin\. \(To optimize performance, CloudFront may request a larger range than the client requested in the `Range GET`\.\) What happens next depends on whether the origin supports `Range GET` requests:
+ **If the origin supports `Range GET` requests:** It returns the requested range\. CloudFront serves the requested range and also caches it for future requests\. \(Amazon S3 supports `Range GET` requests, as do many HTTP servers\.\)
+ **If the origin doesn’t support `Range GET` requests:** It returns the entire object\. CloudFront serves the current request by sending the entire object while also caching it for future requests\. After CloudFront caches the entire object in an edge cache, it responds to new `Range GET` requests by serving the requested range\.

In either case, CloudFront begins to serve the requested range or object to the end user as soon as the first byte arrives from the origin\.

**Note**  
If the viewer makes a `Range GET` request and the origin returns `Transfer-Encoding: chunked`, CloudFront returns the entire object to the viewer instead of the requested range\.

CloudFront generally follows the RFC specification for the `Range` header\. However, if your `Range` headers don’t adhere to the following requirements, CloudFront returns HTTP status code `200` with the full object instead of status code `206` with the specified ranges:
+ The ranges must be listed in ascending order\. For example, `100-200,300-400` is valid, `300-400,100-200` is not valid\.
+ The ranges must not overlap\. For example, `100-200,150-250` is not valid\.
+ All of the ranges specifications must be valid\. For example, you can’t specify a negative value as part of a range\.

For more information about the `Range` request header, see [Range Requests](https://httpwg.org/specs/rfc7233.html#range.requests) in RFC 7233, or [Range](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Range) in the MDN Web Docs\.

## Use range requests to cache large objects<a name="cache-large-objects-with-range-requests"></a>

When caching is enabled, CloudFront doesn’t retrieve or cache an object that is larger than 30 GB\. When an origin indicates that the object is larger than 30 GB \(in the `Content-Length` response header\), CloudFront closes the connection to the origin and returns an error to the viewer\. \(With caching disabled, CloudFront can retrieve an object that is larger than 30 GB from the origin and pass it along to the viewer\. However, CloudFront doesn’t cache the object\.\)

However, with range requests, you can use CloudFront to cache an object that is larger than CloudFront’s [maximum cacheable file size of 30 GB](cloudfront-limits.md#limits-web-distributions)\. For example, consider an origin with a 100 GB object\. With caching enabled, CloudFront doesn’t retrieve or cache an object this large\. However, the viewer can send multiple range requests to retrieve this object in parts, with each part smaller than 30 GB\. For example, the viewer can request the object in 20 GB parts by sending a request with the header `Range: bytes=0-21474836480` to retrieve the first part, another request with the header `Range: bytes=21474836481-42949672960` to retrieve the next part, and so on\. When the viewer has received all of the parts, it can combine them to construct the original 100 GB object\. In this case, CloudFront caches each of the 20 GB parts of the object and can respond to subsequent requests for the same part from the cache\.