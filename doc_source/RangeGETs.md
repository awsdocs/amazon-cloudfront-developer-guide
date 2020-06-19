# How CloudFront Processes Partial Requests for an Object \(Range GETs\)<a name="RangeGETs"></a>

For a large object, an end user's browser or client might make multiple `GET` requests and use the `Range` request header to download the object in smaller units\. These requests for ranges of bytes, sometimes known as `Range GET` requests, improve the efficiency of partial downloads and the recovery from partially failed transfers\. 

When CloudFront receives a `Range GET` request, it checks the cache in the edge location that received the request\. If the cache in that edge location already contains the entire object or the requested portion of the object, CloudFront immediately serves the requested range from the cache\.

If the cache doesn't contain the requested range, CloudFront forwards the request to the origin\. \(To optimize performance, CloudFront may request a larger range than the client requested in the `Range GET`\.\) What happens next depends on whether the origin supports `Range GET` requests:
+ **If the origin supports `Range GET` requests:** It returns the requested range\. CloudFront serves the requested range and also caches it for future requests\. \(Amazon S3 supports `Range GET` requests, as do some HTTP servers, for example, Apache and IIS\. For information about whether your HTTP server does, see the documentation for your HTTP server\.\)
+ **If the origin doesn't support `Range GET` requests:** It returns the entire object\. CloudFront serves the current request by sending the entire object while also caching it for future requests\. After CloudFront caches the entire object in an edge cache, it responds to new `Range GET` requests by serving the requested range\.

In either case, CloudFront begins to serve the requested range or object to the end user as soon as the first byte arrives from the origin\.

**Note**  
If the viewer makes a `Range GET` request and the origin returns `Transfer-Encoding: chunked`, CloudFront returns the entire object to the viewer instead of the requested range\.

CloudFront generally follows the RFC specification for the `Range` header\. However, if your `Range` headers don’t adhere to the following requirements, CloudFront returns HTTP status code `200` with the full object instead of status code `206` with the specified ranges:
+ The ranges must be listed in ascending order\. For example, `100-200,300-400` is valid, `300-400,100-200` is not valid\.
+ The ranges must not overlap\. For example, `100-200,150-250` is not valid\.
+ All of the ranges specifications must be valid\. For example, you can't specify a negative value as part of a range\.

For more information about the `Range` request header, see "Section 14\.35 Range" in *Hypertext Transfer Protocol \-\- HTTP/1\.1* at [http://www\.w3\.org/Protocols/rfc2616/rfc2616\-sec14\.html\#sec14\.35](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.35)\. 