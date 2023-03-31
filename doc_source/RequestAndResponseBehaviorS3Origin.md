# Request and response behavior for Amazon S3 origins<a name="RequestAndResponseBehaviorS3Origin"></a>

**Topics**
+ [How CloudFront processes HTTP and HTTPS requests](HTTPandHTTPSRequests.md)
+ [How CloudFront processes and forwards requests to your Amazon S3 origin](#RequestBehaviorS3Origin)
+ [How CloudFront processes responses from your Amazon S3 origin](#ResponseBehaviorS3Origin)

## How CloudFront processes and forwards requests to your Amazon S3 origin<a name="RequestBehaviorS3Origin"></a>

This topic contains information about how CloudFront processes viewer requests and forwards the requests to your Amazon S3 origin\.

**Topics**
+ [Caching duration and minimum TTL](#RequestS3Caching)
+ [Client IP addresses](#RequestS3IPAddresses)
+ [Conditional GETs](#RequestS3ConditionalGETs)
+ [Cookies](#RequestS3Cookies)
+ [Cross\-origin resource sharing \(CORS\)](#RequestS3-cors)
+ [GET requests that include a body](#RequestS3-get-body)
+ [HTTP methods](#RequestS3HTTPMethods)
+ [HTTP request headers that CloudFront removes or updates](#request-s3-removed-headers)
+ [Maximum length of a request and maximum length of a URL](#RequestS3MaxRequestStringLength)
+ [OCSP stapling](#request-s3-ocsp-stapling)
+ [Protocols](#RequestS3Protocol)
+ [Query strings](#RequestS3QueryStrings)
+ [Origin connection timeout and attempts](#s3-origin-timeout-attempts)
+ [Origin response timeout](#RequestS3RequestTimeout)
+ [Simultaneous requests for the same object \(request collapsing\)](#request-s3-traffic-spikes)

### Caching duration and minimum TTL<a name="RequestS3Caching"></a>

To control how long your objects stay in a CloudFront cache before CloudFront forwards another request to your origin, you can:
+ Configure your origin to add a `Cache-Control` or an `Expires` header field to each object\.
+ Specify a value for Minimum TTL in CloudFront cache behaviors\.
+ Use the default value of 24 hours\.

For more information, see [Managing how long content stays in the cache \(expiration\)](Expiration.md)\.

### Client IP addresses<a name="RequestS3IPAddresses"></a>

If a viewer sends a request to CloudFront and does not include an `X-Forwarded-For` request header, CloudFront gets the IP address of the viewer from the TCP connection, adds an `X-Forwarded-For` header that includes the IP address, and forwards the request to the origin\. For example, if CloudFront gets the IP address `192.0.2.2` from the TCP connection, it forwards the following header to the origin:

`X-Forwarded-For: 192.0.2.2`

If a viewer sends a request to CloudFront and includes an `X-Forwarded-For` request header, CloudFront gets the IP address of the viewer from the TCP connection, appends it to the end of the `X-Forwarded-For` header, and forwards the request to the origin\. For example, if the viewer request includes `X-Forwarded-For: 192.0.2.4,192.0.2.3` and CloudFront gets the IP address `192.0.2.2` from the TCP connection, it forwards the following header to the origin:

`X-Forwarded-For: 192.0.2.4,192.0.2.3,192.0.2.2`

**Note**  
The `X-Forwarded-For` header contains IPv4 addresses \(such as 192\.0\.2\.44\) and IPv6 addresses \(such as 2001:0db8:85a3::8a2e:0370:7334\)\.

### Conditional GETs<a name="RequestS3ConditionalGETs"></a>

When CloudFront receives a request for an object that has expired from an edge cache, it forwards the request to the Amazon S3 origin either to get the latest version of the object or to get confirmation from Amazon S3 that the CloudFront edge cache already has the latest version\. When Amazon S3 originally sent the object to CloudFront, it included an `ETag` value and a `LastModified` value in the response\. In the new request that CloudFront forwards to Amazon S3, CloudFront adds one or both of the following:
+ An `If-Match` or `If-None-Match` header that contains the `ETag` value for the expired version of the object\.
+ An `If-Modified-Since` header that contains the `LastModified` value for the expired version of the object\.

Amazon S3 uses this information to determine whether the object has been updated and, therefore, whether to return the entire object to CloudFront or to return only an HTTP 304 status code \(not modified\)\.

### Cookies<a name="RequestS3Cookies"></a>

Amazon S3 doesn't process cookies\. If you configure a cache behavior to forward cookies to an Amazon S3 origin, CloudFront forwards the cookies, but Amazon S3 ignores them\. All future requests for the same object, regardless if you vary the cookie, are served from the existing object in the cache\.

### Cross\-origin resource sharing \(CORS\)<a name="RequestS3-cors"></a>

If you want CloudFront to respect Amazon S3 cross\-origin resource sharing settings, configure CloudFront to forward selected headers to Amazon S3\. For more information, see [Caching content based on request headers](header-caching.md)\.

### GET requests that include a body<a name="RequestS3-get-body"></a>

If a viewer `GET` request includes a body, CloudFront returns an HTTP status code 403 \(Forbidden\) to the viewer\.

### HTTP methods<a name="RequestS3HTTPMethods"></a>

If you configure CloudFront to process all of the HTTP methods that it supports, CloudFront accepts the following requests from viewers and forwards them to your Amazon S3 origin:
+ `DELETE`
+ `GET`
+ `HEAD`
+ `OPTIONS`
+ `PATCH`
+ `POST`
+ `PUT`

CloudFront always caches responses to `GET` and `HEAD` requests\. You can also configure CloudFront to cache responses to `OPTIONS` requests\. CloudFront does not cache responses to requests that use the other methods\.

If you want to use multi\-part uploads to add objects to an Amazon S3 bucket, you must add a CloudFront origin access control \(OAC\) to your distribution and give the OAC the needed permissions\. For more information, see [Restricting access to an Amazon S3 origin](private-content-restricting-access-to-s3.md)\.

**Important**  
If you configure CloudFront to accept and forward to Amazon S3 all of the HTTP methods that CloudFront supports, you must create a CloudFront origin access control \(OAC\) to restrict access to your Amazon S3 content and give the OAC the required permissions\. For example, if you configure CloudFront to accept and forward these methods because you want to use `PUT`, you must configure Amazon S3 bucket policies to handle `DELETE` requests appropriately so viewers can't delete resources that you don't want them to\. For more information, see [Restricting access to an Amazon S3 origin](private-content-restricting-access-to-s3.md)\.

For information about the operations supported by Amazon S3, see the [ Amazon S3 documentation](https://docs.aws.amazon.com/s3/index.html)\.

### HTTP request headers that CloudFront removes or updates<a name="request-s3-removed-headers"></a>

CloudFront removes or updates some headers before forwarding requests to your Amazon S3 origin\. For most headers this behavior is the same as for custom origins\. For a full list of HTTP request headers and how CloudFront processes them, see [HTTP request headers and CloudFront behavior \(custom and Amazon S3 origins\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)\.

### Maximum length of a request and maximum length of a URL<a name="RequestS3MaxRequestStringLength"></a>

The maximum length of a request, including the path, the query string \(if any\), and headers, is 20,480 bytes\.

CloudFront constructs a URL from the request\. The maximum length of this URL is 8192 bytes\.

If a request or a URL exceeds these maximums, CloudFront returns HTTP status code 413, Request Entity Too Large, to the viewer, and then terminates the TCP connection to the viewer\.

### OCSP stapling<a name="request-s3-ocsp-stapling"></a>

When a viewer submits an HTTPS request for an object, either CloudFront or the viewer must confirm with the certificate authority \(CA\) that the SSL certificate for the domain has not been revoked\. OCSP stapling speeds up certificate validation by allowing CloudFront to validate the certificate and to cache the response from the CA, so the client doesn't need to validate the certificate directly with the CA\.

The performance improvement of OCSP stapling is more pronounced when CloudFront receives a lot of HTTPS requests for objects in the same domain\. Each server in a CloudFront edge location must submit a separate validation request\. When CloudFront receives a lot of HTTPS requests for the same domain, every server in the edge location soon has a response from the CA that it can "staple" to a packet in the SSL handshake; when the viewer is satisfied that the certificate is valid, CloudFront can serve the requested object\. If your distribution doesn't get much traffic in a CloudFront edge location, new requests are more likely to be directed to a server that hasn't validated the certificate with the CA yet\. In that case, the viewer separately performs the validation step and the CloudFront server serves the object\. That CloudFront server also submits a validation request to the CA, so the next time it receives a request that includes the same domain name, it has a validation response from the CA\.

### Protocols<a name="RequestS3Protocol"></a>

CloudFront forwards HTTP or HTTPS requests to the origin server based on the protocol of the viewer request, either HTTP or HTTPS\.

**Important**  
If your Amazon S3 bucket is configured as a website endpoint, you cannot configure CloudFront to use HTTPS to communicate with your origin because Amazon S3 doesn't support HTTPS connections in that configuration\.

### Query strings<a name="RequestS3QueryStrings"></a>

You can configure whether CloudFront forwards query string parameters to your Amazon S3 origin\. For more information, see [Caching content based on query string parameters](QueryStringParameters.md)\.

### Origin connection timeout and attempts<a name="s3-origin-timeout-attempts"></a>

*Origin connection timeout* is the number of seconds that CloudFront waits when trying to establish a connection to the origin\.

*Origin connection attempts* is the number of times that CloudFront attempts to connect to the origin\.

Together, these settings determine how long CloudFront tries to connect to the origin before failing over to the secondary origin \(in the case of an origin group\) or returning an error response to the viewer\. By default, CloudFront waits as long as 30 seconds \(3 attempts of 10 seconds each\) before attempting to connect to the secondary origin or returning an error response\. You can reduce this time by specifying a shorter connection timeout, fewer attempts, or both\.

For more information, see [Controlling origin timeouts and attempts](high_availability_origin_failover.md#controlling-attempts-and-timeouts)\.

### Origin response timeout<a name="RequestS3RequestTimeout"></a>

The *origin response timeout*, also known as the *origin read timeout* or *origin request timeout*, applies to both of the following:
+ The amount of time, in seconds, that CloudFront waits for a response after forwarding a request to the origin\.
+ The amount of time, in seconds, that CloudFront waits after receiving a packet of a response from the origin and before receiving the next packet\.

CloudFront behavior depends on the HTTP method of the viewer request:
+ `GET` and `HEAD` requests – If the origin doesn’t respond within 30 seconds or stops responding for 30 seconds, CloudFront drops the connection\. If the specified number of [origin connection attempts](distribution-web-values-specify.md#origin-connection-attempts) is more than 1, CloudFront tries again to get a complete response\. CloudFront tries up to 3 times, as determined by the value of the *origin connection attempts* setting\. If the origin doesn’t respond during the final attempt, CloudFront doesn’t try again until it receives another request for content on the same origin\.
+ `DELETE`, `OPTIONS`, `PATCH`, `PUT`, and `POST` requests – If the origin doesn’t respond within 30 seconds, CloudFront drops the connection and doesn’t try again to contact the origin\. The client can resubmit the request if necessary\.

You can’t change the response timeout for an Amazon S3 origin \(an S3 bucket that is *not* configured with static website hosting\)\.

### Simultaneous requests for the same object \(request collapsing\)<a name="request-s3-traffic-spikes"></a>

When a CloudFront edge location receives a request for an object and the object isn't in the cache or the cached object is expired, CloudFront immediately sends the request to the origin\. However, if there are simultaneous requests for the same object—that is, if additional requests for the same object \(with the same cache key\) arrive at the edge location before CloudFront receives the response to the first request—CloudFront pauses before forwarding the additional requests to the origin\. This brief pause helps to reduce the load on the origin\. CloudFront sends the response from the original request to all the requests that it received while it was paused\. This is called *request collapsing*\. In CloudFront logs, the first request is identified as a `Miss` in the `x-edge-result-type` field, and the collapsed requests are identified as a `Hit`\. For more information about CloudFront logs, see [CloudFront and edge function logging](logging.md)\.

CloudFront only collapses requests that share a [*cache key*](understanding-the-cache-key.md)\. If the additional requests do not share the same cache key because, for example, you configured CloudFront to cache based on request headers or cookies or query strings, CloudFront forwards all the requests with a unique cache key to your origin\.

If you would like to prevent all request collapsing, you can do one of the following:
+ Use the managed cache policy `CachingDisabled`, which also prevents caching\. For more information, see [Using the managed cache policies](using-managed-cache-policies.md)\.
+ Forward cookies to the origin, either by using a cache policy to cache based on the `Cookie` header, or by using an origin request policy to include the `Cookie` header in origin requests\.

If you would like to prevent request collapsing for specific objects, you can set the minimum TTL for the cache behavior to 0 *and* configure the origin to send `Cache-Control: private`, `Cache-Control: no-store`, `Cache-Control: no-cache`, `Cache-Control: max-age=0`, or `Cache-Control: s-maxage=0`\. These configurations will increase the load on your origin and introduce additional latency for the simultaneous requests that are paused while CloudFront waits for the response to the first request\.

## How CloudFront processes responses from your Amazon S3 origin<a name="ResponseBehaviorS3Origin"></a>

This topic contains information about how CloudFront processes responses from your Amazon S3 origin\.

**Topics**
+ [Canceled requests](#response-s3-canceled-requests)
+ [HTTP response headers that CloudFront removes or updates](#response-s3-removed-headers)
+ [Maximum cacheable file size](#ResponseS3MaxFileSize)
+ [Redirects](#ResponseS3Redirects)

### Canceled requests<a name="response-s3-canceled-requests"></a>

If an object is not in the edge cache, and if a viewer terminates a session \(for example, closes a browser\) after CloudFront gets the object from your origin but before it can deliver the requested object, CloudFront does not cache the object in the edge location\.

### HTTP response headers that CloudFront removes or updates<a name="response-s3-removed-headers"></a>

CloudFront removes or updates the following header fields before forwarding the response from your Amazon S3 origin to the viewer: 
+ `Set-Cookie` – If you configure CloudFront to forward cookies, it will forward the `Set-Cookie` header field to clients\. For more information, see [Caching content based on cookies](Cookies.md)\.
+ `Trailer`
+ `Transfer-Encoding` – If your Amazon S3 origin returns this header field, CloudFront sets the value to `chunked` before returning the response to the viewer\.
+ `Upgrade`
+ `Via` – CloudFront sets the value to the following in the response to the viewer:

  `Via: `*http\-version* *alphanumeric\-string*`.cloudfront.net (CloudFront)`

  For example, if the client makes a request over HTTP/1\.1, the value is something like the following:

  `Via: 1.1 1026589cc7887e7a0dc7827b4example.cloudfront.net (CloudFront)`

### Maximum cacheable file size<a name="ResponseS3MaxFileSize"></a>

The maximum size of a response body that CloudFront saves in its cache is 30 GB\. This includes chunked transfer responses that don't specify the `Content-Length` header value\.

You can use CloudFront to cache an object that is larger than 30 GB by using range requests to request the objects in parts that are each 30 GB or smaller\. CloudFront caches these parts because each of them is 30 GB or smaller\. After the viewer retrieves all the parts of the object, it can reconstruct the original, larger object\. For more information, see [Use range requests to cache large objects](RangeGETs.md#cache-large-objects-with-range-requests)\.

### Redirects<a name="ResponseS3Redirects"></a>

You can configure an Amazon S3 bucket to redirect all requests to another host name; this can be another Amazon S3 bucket or an HTTP server\. If you configure a bucket to redirect all requests and if the bucket is the origin for a CloudFront distribution, we recommend that you configure the bucket to redirect all requests to a CloudFront distribution using either the domain name for the distribution \(for example, d111111abcdef8\.cloudfront\.net\) or an alternate domain name \(a CNAME\) that is associated with a distribution \(for example, example\.com\)\. Otherwise, viewer requests bypass CloudFront, and the objects are served directly from the new origin\.

**Note**  
If you redirect requests to an alternate domain name, you must also update the DNS service for your domain by adding a CNAME record\. For more information, see [Using custom URLs by adding alternate domain names \(CNAMEs\)](CNAMEs.md)\.

Here's what happens when you configure a bucket to redirect all requests:

1. A viewer \(for example, a browser\) requests an object from CloudFront\.

1. CloudFront forwards the request to the Amazon S3 bucket that is the origin for your distribution\.

1. Amazon S3 returns an HTTP status code 301 \(Moved Permanently\) as well as the new location\.

1. CloudFront caches the redirect status code and the new location, and returns the values to the viewer\. CloudFront does not follow the redirect to get the object from the new location\.

1. The viewer sends another request for the object, but this time the viewer specifies the new location that it got from CloudFront:
   + If the Amazon S3 bucket is redirecting all requests to a CloudFront distribution, using either the domain name for the distribution or an alternate domain name, CloudFront requests the object from the Amazon S3 bucket or the HTTP server in the new location\. When the new location returns the object, CloudFront returns it to the viewer and caches it in an edge location\.
   + If the Amazon S3 bucket is redirecting requests to another location, the second request bypasses CloudFront\. The Amazon S3 bucket or the HTTP server in the new location returns the object directly to the viewer, so the object is never cached in a CloudFront edge cache\.