# Request and Response Behavior for Amazon S3 Origins<a name="RequestAndResponseBehaviorS3Origin"></a>

**Topics**
+ [How CloudFront Processes HTTP and HTTPS Requests](HTTPandHTTPSRequests.md)
+ [How CloudFront Processes and Forwards Requests to Your Amazon S3 Origin Server](#RequestBehaviorS3Origin)
+ [How CloudFront Processes Responses from Your Amazon S3 Origin Server](#ResponseBehaviorS3Origin)

## How CloudFront Processes and Forwards Requests to Your Amazon S3 Origin Server<a name="RequestBehaviorS3Origin"></a>

This topic contains information about how CloudFront processes viewer requests and forwards the requests to your Amazon S3 origin\.

**Topics**
+ [Caching Duration and Minimum TTL](#RequestS3Caching)
+ [Client IP Addresses](#RequestS3IPAddresses)
+ [Conditional GETs](#RequestS3ConditionalGETs)
+ [Cookies](#RequestS3Cookies)
+ [Cross\-Origin Resource Sharing \(CORS\)](#RequestS3-cors)
+ [GET Requests That Include a Body](#RequestS3-get-body)
+ [HTTP Methods](#RequestS3HTTPMethods)
+ [HTTP Request Headers That CloudFront Removes or Updates](#request-s3-removed-headers)
+ [Maximum Length of a Request and Maximum Length of a URL](#RequestS3MaxRequestStringLength)
+ [OCSP Stapling](#request-s3-ocsp-stapling)
+ [Protocols](#RequestS3Protocol)
+ [Query Strings](#RequestS3QueryStrings)
+ [Origin Connection Timeout and Attempts](#s3-origin-timeout-attempts)
+ [Origin Response Timeout](#RequestS3RequestTimeout)
+ [Simultaneous Requests for the Same Object \(Traffic Spikes\)](#request-s3-traffic-spikes)

### Caching Duration and Minimum TTL<a name="RequestS3Caching"></a>

For web distributions, to control how long your objects stay in a CloudFront cache before CloudFront forwards another request to your origin, you can:
+ Configure your origin to add a `Cache-Control` or an `Expires` header field to each object\.
+ Specify a value for Minimum TTL in CloudFront cache behaviors\.
+ Use the default value of 24 hours\.

For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

### Client IP Addresses<a name="RequestS3IPAddresses"></a>

If a viewer sends a request to CloudFront and does not include an `X-Forwarded-For` request header, CloudFront gets the IP address of the viewer from the TCP connection, adds an `X-Forwarded-For` header that includes the IP address, and forwards the request to the origin\. For example, if CloudFront gets the IP address `192.0.2.2` from the TCP connection, it forwards the following header to the origin:

`X-Forwarded-For: 192.0.2.2`

If a viewer sends a request to CloudFront and includes an `X-Forwarded-For` request header, CloudFront gets the IP address of the viewer from the TCP connection, appends it to the end of the `X-Forwarded-For` header, and forwards the request to the origin\. For example, if the viewer request includes `X-Forwarded-For: 192.0.2.4,192.0.2.3` and CloudFront gets the IP address `192.0.2.2` from the TCP connection, it forwards the following header to the origin:

`X-Forwarded-For: 192.0.2.4,192.0.2.3,192.0.2.2`

**Note**  
The `X-Forwarded-For` header contains IPv4 addresses \(such as 192\.0\.2\.44\) and IPv6 addresses \(such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334\)\.

### Conditional GETs<a name="RequestS3ConditionalGETs"></a>

When CloudFront receives a request for an object that has expired from an edge cache, it forwards the request to the Amazon S3 origin either to get the latest version of the object or to get confirmation from Amazon S3 that the CloudFront edge cache already has the latest version\. When Amazon S3 originally sent the object to CloudFront, it included an `ETag` value and a `LastModified` value in the response\. In the new request that CloudFront forwards to Amazon S3, CloudFront adds one or both of the following:
+ An `If-Match` or `If-None-Match` header that contains the `ETag` value for the expired version of the object\.
+ An `If-Modified-Since` header that contains the `LastModified` value for the expired version of the object\.

Amazon S3 uses this information to determine whether the object has been updated and, therefore, whether to return the entire object to CloudFront or to return only an HTTP 304 status code \(not modified\)\.

### Cookies<a name="RequestS3Cookies"></a>

Amazon S3 doesn't process cookies\. If you configure a cache behavior to forward cookies to an Amazon S3 origin, CloudFront forwards the cookies, but Amazon S3 ignores them\. All future requests for the same object, regardless if you vary the cookie, are served from the existing object in the cache\.

### Cross\-Origin Resource Sharing \(CORS\)<a name="RequestS3-cors"></a>

If you want CloudFront to respect Amazon S3 cross\-origin resource sharing settings, configure CloudFront to forward selected headers to Amazon S3\. For more information, see [Caching Content Based on Request Headers](header-caching.md)\.

### GET Requests That Include a Body<a name="RequestS3-get-body"></a>

If a viewer `GET` request includes a body, CloudFront returns an HTTP status code 403 \(Forbidden\) to the viewer\.

### HTTP Methods<a name="RequestS3HTTPMethods"></a>

If you configure CloudFront to process all of the HTTP methods that it supports, CloudFront accepts the following requests from viewers and forwards them to your Amazon S3 origin:
+ `DELETE`
+ `GET`
+ `HEAD`
+ `OPTIONS`
+ `PATCH`
+ `POST`
+ `PUT`

CloudFront always caches responses to `GET` and `HEAD` requests\. You can also configure CloudFront to cache responses to `OPTIONS` requests\. CloudFront does not cache responses to requests that use the other methods\.

If you use an Amazon S3 bucket as the origin for your distribution and if you use CloudFront origin access identities, `POST` requests aren't supported in some Amazon S3 Regions and `PUT` requests in those Regions require an additional header\. For more information, see [Using an OAI in Amazon S3 Regions that Support Only Signature Version 4 Authentication](private-content-restricting-access-to-s3.md#private-content-origin-access-identity-signature-version-4)\.

If you want to use multi\-part uploads to add objects to an Amazon S3 bucket, you must add a CloudFront origin access identity to your distribution and grant the origin access identity the needed permissions\. For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

**Important**  
If you configure CloudFront to accept and forward to Amazon S3 all of the HTTP methods that CloudFront supports, you must create a CloudFront origin access identity to restrict access to your Amazon S3 content and grant the origin access identity the required permissions\. For example, if you configure CloudFront to accept and forward these methods because you want to use `PUT`, you must configure Amazon S3 bucket policies or ACLs to handle `DELETE` requests appropriately so viewers can't delete resources that you don't want them to\. For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

For information about the operations supported by Amazon S3, see the [ Amazon S3 documentation](http://aws.amazon.com/documentation/s3/)\.

### HTTP Request Headers That CloudFront Removes or Updates<a name="request-s3-removed-headers"></a>

CloudFront removes or updates some headers before forwarding requests to your Amazon S3 origin\. For most headers this behavior is the same as for custom origins\. For a full list of HTTP request headers and how CloudFront processes them, see [HTTP Request Headers and CloudFront Behavior \(Custom and S3 Origins\)](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)\.

### Maximum Length of a Request and Maximum Length of a URL<a name="RequestS3MaxRequestStringLength"></a>

The maximum length of a request, including the path, the query string \(if any\), and headers, is 20,480 bytes\.

CloudFront constructs a URL from the request\. The maximum length of this URL is 8192 bytes\.

If a request or a URL exceeds these maximums, CloudFront returns HTTP status code 413, Request Entity Too Large, to the viewer, and then terminates the TCP connection to the viewer\.

### OCSP Stapling<a name="request-s3-ocsp-stapling"></a>

When a viewer submits an HTTPS request for an object, either CloudFront or the viewer must confirm with the certificate authority \(CA\) that the SSL certificate for the domain has not been revoked\. OCSP stapling speeds up certificate validation by allowing CloudFront to validate the certificate and to cache the response from the CA, so the client doesn't need to validate the certificate directly with the CA\.

The performance improvement of OCSP stapling is more pronounced when CloudFront receives a lot of HTTPS requests for objects in the same domain\. Each server in a CloudFront edge location must submit a separate validation request\. When CloudFront receives a lot of HTTPS requests for the same domain, every server in the edge location soon has a response from the CA that it can "staple" to a packet in the SSL handshake; when the viewer is satisfied that the certificate is valid, CloudFront can serve the requested object\. If your distribution doesn't get much traffic in a CloudFront edge location, new requests are more likely to be directed to a server that hasn't validated the certificate with the CA yet\. In that case, the viewer separately performs the validation step and the CloudFront server serves the object\. That CloudFront server also submits a validation request to the CA, so the next time it receives a request that includes the same domain name, it has a validation response from the CA\.

### Protocols<a name="RequestS3Protocol"></a>

CloudFront forwards HTTP or HTTPS requests to the origin server based on the protocol of the viewer request, either HTTP or HTTPS\.

**Important**  
If your Amazon S3 bucket is configured as a website endpoint, you cannot configure CloudFront to use HTTPS to communicate with your origin because Amazon S3 doesn't support HTTPS connections in that configuration\.

### Query Strings<a name="RequestS3QueryStrings"></a>

For web distributions, you can configure whether CloudFront forwards query string parameters to your Amazon S3 origin\. For RTMP distributions, CloudFront does not forward query string parameters\. For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.

### Origin Connection Timeout and Attempts<a name="s3-origin-timeout-attempts"></a>

*Origin connection timeout* is the number of seconds that CloudFront waits when trying to establish a connection to the origin\.

*Origin connection attempts* is the number of times that CloudFront attempts to connect to the origin\.

Together, these settings determine how long CloudFront tries to connect to the origin before failing over to the secondary origin \(in the case of an origin group\) or returning an error response to the viewer\. By default, CloudFront waits as long as 30 seconds \(3 attempts of 10 seconds each\) before attempting to connect to the secondary origin or returning an error response\. You can reduce this time by specifying a shorter connection timeout, fewer attempts, or both\.

For more information, see [Controlling Origin Timeouts and Attempts](high_availability_origin_failover.md#controlling-attempts-and-timeouts)\.

### Origin Response Timeout<a name="RequestS3RequestTimeout"></a>

The *origin response timeout*, also known as the *origin read timeout* or *origin request timeout*, applies to both of the following:
+ The amount of time, in seconds, that CloudFront waits for a response after forwarding a request to the origin\.
+ The amount of time, in seconds, that CloudFront waits after receiving a packet of a response from the origin and before receiving the next packet\.

CloudFront behavior depends on the HTTP method of the viewer request:
+ `GET` and `HEAD` requests – If the origin doesn’t respond within 30 seconds or stops responding for 30 seconds, CloudFront drops the connection\. If the specified number of [origin connection attempts](distribution-web-values-specify.md#origin-connection-attempts) is more than 1, CloudFront tries again to get a complete response\. CloudFront tries up to 3 times, as determined by the value of the *origin connection attempts* setting\. If the origin doesn’t respond during the final attempt, CloudFront doesn’t try again until it receives another request for content on the same origin\.
+ `DELETE`, `OPTIONS`, `PATCH`, `PUT`, and `POST` requests – If the origin doesn’t respond within 30 seconds, CloudFront drops the connection and doesn’t try again to contact the origin\. The client can resubmit the request if necessary\.

You can’t change the response timeout for an Amazon S3 origin \(an S3 bucket that is *not* configured with static website hosting\)\.

### Simultaneous Requests for the Same Object \(Traffic Spikes\)<a name="request-s3-traffic-spikes"></a>

When a CloudFront edge location receives a request for an object and either the object isn't currently in the cache or the object has expired, CloudFront immediately sends the request to your Amazon S3 origin\. If there's a traffic spike—if additional requests for the same object arrive at the edge location before Amazon S3 responds to the first request—CloudFront pauses briefly before forwarding additional requests for the object to your origin\. Typically, the response to the first request will arrive at the CloudFront edge location before the response to subsequent requests\. This brief pause helps to reduce unnecessary load on Amazon S3\. If additional requests are not identical because, for example, you configured CloudFront to cache based on request headers or query strings, CloudFront forwards all of the unique requests to your origin\.

When the response from the origin includes a `Cache-Control: no-cache` header, CloudFront typically forwards the next request for the same object to the origin to determine whether the object has been updated\. However, when there's a traffic spike and CloudFront pauses after forwarding the first request to your origin, multiple viewer requests might arrive before CloudFront receives a response from the origin\. When CloudFront receives a response that contains a `Cache-Control: no-cache` header, it sends the object in the response to the viewer that made the original request and to all of the viewers that requested the object during the pause\. After the response arrives from the origin, CloudFront forwards the next viewer request for the same object to the origin\. In CloudFront access logs, the first request is identified as a `Miss` in the `x-edge-result-type` column, and all subsequent requests that CloudFront received during the pause are identified as a `Hit`\. For more information about access log file format, see [Web Distribution Standard Log File Format](AccessLogs.md#BasicDistributionFileFormat)\.

## How CloudFront Processes Responses from Your Amazon S3 Origin Server<a name="ResponseBehaviorS3Origin"></a>

This topic contains information about how CloudFront processes responses from your Amazon S3 origin\.

**Topics**
+ [Canceled Requests](#response-s3-canceled-requests)
+ [HTTP Response Headers That CloudFront Removes or Updates](#response-s3-removed-headers)
+ [Maximum File Size](#ResponseS3MaxFileSize)
+ [Redirects](#ResponseS3Redirects)

### Canceled Requests<a name="response-s3-canceled-requests"></a>

If an object is not in the edge cache, and if a viewer terminates a session \(for example, closes a browser\) after CloudFront gets the object from your origin but before it can deliver the requested object, CloudFront does not cache the object in the edge location\.

### HTTP Response Headers That CloudFront Removes or Updates<a name="response-s3-removed-headers"></a>

CloudFront removes or updates the following header fields before forwarding the response from your Amazon S3 origin to the viewer: 
+ `Set-Cookie` – If you configure CloudFront to forward cookies, it will forward the `Set-Cookie` header field to clients\. For more information, see [Caching Content Based on Cookies](Cookies.md)\.
+ `Trailer`
+ `Transfer-Encoding` – If your Amazon S3 origin returns this header field, CloudFront sets the value to `chunked` before returning the response to the viewer\.
+ `Upgrade`
+ `Via` – CloudFront sets the value to the following in the response to the viewer:

  `Via: `*http\-version* *alphanumeric\-string*`.cloudfront.net (CloudFront)`

  For example, if the client makes a request over HTTP/1\.1, the value is something like the following:

  `Via: 1.1 1026589cc7887e7a0dc7827b4example.cloudfront.net (CloudFront)`

### Maximum File Size<a name="ResponseS3MaxFileSize"></a>

The maximum size of a response body that CloudFront will return to the viewer is 20 GB\. This includes chunked transfer responses that don't specify the `Content-Length` header value\.

### Redirects<a name="ResponseS3Redirects"></a>

You can configure an Amazon S3 bucket to redirect all requests to another host name; this can be another Amazon S3 bucket or an HTTP server\. If you configure a bucket to redirect all requests and if the bucket is the origin for a CloudFront distribution, we recommend that you configure the bucket to redirect all requests to a CloudFront distribution using either the domain name for the distribution \(for example, d111111abcdef8\.cloudfront\.net\) or an alternate domain name \(a CNAME\) that is associated with a distribution \(for example, example\.com\)\. Otherwise, viewer requests bypass CloudFront, and the objects are served directly from the new origin\.

**Note**  
If you redirect requests to an alternate domain name, you must also update the DNS service for your domain by adding a CNAME record\. For more information, see [Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.

Here's what happens when you configure a bucket to redirect all requests:

1. A viewer \(for example, a browser\) requests an object from CloudFront\.

1. CloudFront forwards the request to the Amazon S3 bucket that is the origin for your distribution\.

1. Amazon S3 returns an HTTP status code 301 \(Moved Permanently\) as well as the new location\.

1. CloudFront caches the redirect status code and the new location, and returns the values to the viewer\. CloudFront does not follow the redirect to get the object from the new location\.

1. The viewer sends another request for the object, but this time the viewer specifies the new location that it got from CloudFront:
   + If the Amazon S3 bucket is redirecting all requests to a CloudFront distribution, using either the domain name for the distribution or an alternate domain name, CloudFront requests the object from the Amazon S3 bucket or the HTTP server in the new location\. When the new location returns the object, CloudFront returns it to the viewer and caches it in an edge location\.
   + If the Amazon S3 bucket is redirecting requests to another location, the second request bypasses CloudFront\. The Amazon S3 bucket or the HTTP server in the new location returns the object directly to the viewer, so the object is never cached in a CloudFront edge cache\.