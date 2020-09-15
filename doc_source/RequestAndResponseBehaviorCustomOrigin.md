# Request and Response Behavior for Custom Origins<a name="RequestAndResponseBehaviorCustomOrigin"></a>

**Topics**
+ [How CloudFront Processes and Forwards Requests to Your Custom Origin Server](#RequestBehaviorCustomOrigin)
+ [How CloudFront Processes Responses from Your Custom Origin Server](#ResponseBehaviorCustomOrigin)

## How CloudFront Processes and Forwards Requests to Your Custom Origin Server<a name="RequestBehaviorCustomOrigin"></a>

This topic contains information about how CloudFront processes viewer requests and forwards the requests to your custom origin\.

**Topics**
+ [Authentication](#RequestCustomClientAuth)
+ [Caching Duration and Minimum TTL](#RequestCustomCaching)
+ [Client IP Addresses](#RequestCustomIPAddresses)
+ [Client\-Side SSL Authentication](#RequestCustomClientSideSslAuth)
+ [Compression](#RequestCustomCompression)
+ [Conditional Requests](#RequestCustomConditionalGETs)
+ [Cookies](#RequestCustomCookies)
+ [Cross\-Origin Resource Sharing \(CORS\)](#request-custom-cors)
+ [Encryption](#RequestCustomEncryption)
+ [GET Requests That Include a Body](#RequestCustom-get-body)
+ [HTTP Methods](#RequestCustomHTTPMethods)
+ [HTTP Request Headers and CloudFront Behavior \(Custom and S3 Origins\)](#request-custom-headers-behavior)
+ [HTTP Version](#RequestCustomHTTPVersion)
+ [Maximum Length of a Request and Maximum Length of a URL](#RequestCustomMaxRequestStringLength)
+ [OCSP Stapling](#request-custom-ocsp-stapling)
+ [Persistent Connections](#request-custom-persistent-connections)
+ [Protocols](#RequestCustomProtocols)
+ [Query Strings](#RequestCustomQueryStrings)
+ [Origin Connection Timeout and Attempts](#custom-origin-timeout-attempts)
+ [Origin Response Timeout](#request-custom-request-timeout)
+ [Simultaneous Requests for the Same Object \(Traffic Spikes\)](#request-custom-traffic-spikes)
+ [User\-Agent Header](#request-custom-user-agent-header)

### Authentication<a name="RequestCustomClientAuth"></a>

For `DELETE`, `GET`, `HEAD`, `PATCH`, `POST`, and `PUT` requests, if you configure CloudFront to forward the `Authorization` header to your origin, you can configure your origin server to request client authentication\. 

For `OPTIONS` requests, you can configure your origin server to request client authentication only if you use the following CloudFront settings:
+ Configure CloudFront to forward the `Authorization` header to your origin\.
+ Configure CloudFront to *not* cache the response to `OPTIONS` requests\.

You can configure CloudFront to forward requests to your origin using either HTTP or HTTPS; for more information, see [Using HTTPS with CloudFront](using-https.md)\.

### Caching Duration and Minimum TTL<a name="RequestCustomCaching"></a>

For web distributions, to control how long your objects stay in a CloudFront cache before CloudFront forwards another request to your origin, you can:
+ Configure your origin to add a `Cache-Control` or an `Expires` header field to each object\.
+ Specify a value for Minimum TTL in CloudFront cache behaviors\.
+ Use the default value of 24 hours\.

For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

### Client IP Addresses<a name="RequestCustomIPAddresses"></a>

If a viewer sends a request to CloudFront and does not include an `X-Forwarded-For` request header, CloudFront gets the IP address of the viewer from the TCP connection, adds an `X-Forwarded-For` header that includes the IP address, and forwards the request to the origin\. For example, if CloudFront gets the IP address `192.0.2.2` from the TCP connection, it forwards the following header to the origin:

`X-Forwarded-For: 192.0.2.2`

If a viewer sends a request to CloudFront and includes an `X-Forwarded-For` request header, CloudFront gets the IP address of the viewer from the TCP connection, appends it to the end of the `X-Forwarded-For` header, and forwards the request to the origin\. For example, if the viewer request includes `X-Forwarded-For: 192.0.2.4,192.0.2.3` and CloudFront gets the IP address `192.0.2.2` from the TCP connection, it forwards the following header to the origin:

`X-Forwarded-For: 192.0.2.4,192.0.2.3,192.0.2.2`

Some applications, such as load balancers \(including Elastic Load Balancing\), web application firewalls, reverse proxies, intrusion prevention systems, and API Gateway, append the IP address of the CloudFront edge server that forwarded the request onto the end of the `X-Forwarded-For` header\. For example, if CloudFront includes `X-Forwarded-For: 192.0.2.2` in a request that it forwards to ELB and if the IP address of the CloudFront edge server is 192\.0\.2\.199, the request that your EC2 instance receives contains the following header:

`X-Forwarded-For: 192.0.2.2,192.0.2.199`

**Note**  
The `X-Forwarded-For` header contains IPv4 addresses \(such as 192\.0\.2\.44\) and IPv6 addresses \(such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334\)\.

### Client\-Side SSL Authentication<a name="RequestCustomClientSideSslAuth"></a>

CloudFront does not support client authentication with client\-side SSL certificates\. If an origin requests a client\-side certificate, CloudFront drops the request\. 

### Compression<a name="RequestCustomCompression"></a>

For more information, see [Serving compressed files](ServingCompressedFiles.md)\. 

### Conditional Requests<a name="RequestCustomConditionalGETs"></a>

When CloudFront receives a request for an object that has expired from an edge cache, it forwards the request to the origin either to get the latest version of the object or to get confirmation from the origin that the CloudFront edge cache already has the latest version\. Typically, when the origin last sent the object to CloudFront, it included an `ETag` value, a `LastModified` value, or both values in the response\. In the new request that CloudFront forwards to the origin, CloudFront adds one or both of the following:
+ An `If-Match` or `If-None-Match` header that contains the `ETag` value for the expired version of the object\.
+ An `If-Modified-Since` header that contains the `LastModified` value for the expired version of the object\.

The origin uses this information to determine whether the object has been updated and, therefore, whether to return the entire object to CloudFront or to return only an HTTP 304 status code \(not modified\)\.

### Cookies<a name="RequestCustomCookies"></a>

You can configure CloudFront to forward cookies to your origin\. For more information, see [Caching Content Based on Cookies](Cookies.md)\.

### Cross\-Origin Resource Sharing \(CORS\)<a name="request-custom-cors"></a>

If you want CloudFront to respect cross\-origin resource sharing settings, configure CloudFront to forward the `Origin` header to your origin\. For more information, see [Caching Content Based on Request Headers](header-caching.md)\.

### Encryption<a name="RequestCustomEncryption"></a>

You can require viewers to use HTTPS to send requests to CloudFront and require CloudFront to forward requests to your custom origin by using the protocol that is used by the viewer\. For more information, see the following distribution settings:
+ [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)
+ [Origin Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesOriginProtocolPolicy)

CloudFront forwards HTTPS requests to the origin server using the SSLv3, TLSv1\.0, TLSv1\.1, and TLSv1\.2 protocols\. For custom origins, you can choose the SSL protocols that you want CloudFront to use when communicating with your origin:
+ If you're using the CloudFront console, choose protocols by using the **Origin SSL Protocols** check boxes\. For more information, see [Creating a Distribution](distribution-web-creating-console.md)\. 
+ If you're using the CloudFront API, specify protocols by using the `OriginSslProtocols` element\. For more information, see [ OriginSslProtocols](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_OriginSslProtocols.html) and [ DistributionConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DistributionConfig.html) in the *Amazon CloudFront API Reference*\.

If the origin is an Amazon S3 bucket, CloudFront always uses TLSv1\.2\.

**Important**  
Other versions of SSL and TLS are not supported\.

For more information about using HTTPS with CloudFront, see [Using HTTPS with CloudFront](using-https.md)\. For lists of the ciphers that CloudFront supports for HTTPS communication between viewers and CloudFront, and between CloudFront and your origin, see [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)\.

### GET Requests That Include a Body<a name="RequestCustom-get-body"></a>

If a viewer `GET` request includes a body, CloudFront returns an HTTP status code 403 \(Forbidden\) to the viewer\.

### HTTP Methods<a name="RequestCustomHTTPMethods"></a>

If you configure CloudFront to process all of the HTTP methods that it supports, CloudFront accepts the following requests from viewers and forwards them to your custom origin:
+ `DELETE`
+ `GET`
+ `HEAD`
+ `OPTIONS`
+ `PATCH`
+ `POST`
+ `PUT`

CloudFront always caches responses to `GET` and `HEAD` requests\. You can also configure CloudFront to cache responses to `OPTIONS` requests\. CloudFront does not cache responses to requests that use the other methods\.

For information about configuring whether your custom origin processes these methods, see the documentation for your origin\.

**Important**  
If you configure CloudFront to accept and forward to your origin all of the HTTP methods that CloudFront supports, configure your origin server to handle all methods\. For example, if you configure CloudFront to accept and forward these methods because you want to use `POST`, you must configure your origin server to handle `DELETE` requests appropriately so viewers can't delete resources that you don't want them to\. For more information, see the documentation for your HTTP server\.

### HTTP Request Headers and CloudFront Behavior \(Custom and S3 Origins\)<a name="request-custom-headers-behavior"></a>

The following table lists HTTP request headers that you can forward to both custom and Amazon S3 origins \(with the exceptions that are noted\)\. For each header, the table includes information about the following:
+ CloudFront behavior if you don't configure CloudFront to forward the header to your origin, which causes CloudFront to cache your objects based on header values\.
+ Whether you can configure CloudFront to cache objects based on header values for that header\. 

  You can configure CloudFront to cache objects based on values in the `Date` and `User-Agent` headers, but we don't recommend it\. These headers have many possible values, and caching based on their values would cause CloudFront to forward significantly more requests to your origin\.

For more information about caching based on header values, see [Caching Content Based on Request Headers](header-caching.md)\.


****  

| Header | Behavior If You Don't Configure CloudFront to Cache Based on Header Values | Caching Based on Header Values Is Supported | 
| --- | --- | --- | 
| Other\-defined headers | CloudFront forwards the headers to your origin\. | Yes | 
| `Accept` | CloudFront removes the header\. | Yes | 
| `Accept-Charset` | CloudFront removes the header\. | Yes | 
| `Accept-Encoding` | If the value contains `gzip` or `br`, CloudFront forwards a normalized `Accept-Encoding` header to your origin\. For more information, see [Cache compressed objects \(uses the `Accept-Encoding` header\)](controlling-the-cache-key.md#cache-policy-compressed-objects) and [Serving compressed files](ServingCompressedFiles.md)\. | Yes | 
| `Accept-Language` | CloudFront removes the header\. | Yes | 
| `Authorization` |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/RequestAndResponseBehaviorCustomOrigin.html)  | Yes | 
| `Cache-Control` | CloudFront forwards the header to your origin\. | No | 
| `CloudFront-Forwarded-Proto` | CloudFront does not add the header before forwarding the request to your origin\. For more information, see [Configuring Caching Based on the Protocol of the Request](header-caching.md#header-caching-web-protocol)\. | Yes | 
| `CloudFront-Is-Desktop-Viewer` | CloudFront does not add the header before forwarding the request to your origin\. For more information, see [Configuring Caching Based on the Device Type](header-caching.md#header-caching-web-device)\. | Yes | 
| `CloudFront-Is-Mobile-Viewer` | CloudFront does not add the header before forwarding the request to your origin\. For more information, see [Configuring Caching Based on the Device Type](header-caching.md#header-caching-web-device)\. | Yes | 
| `CloudFront-Is-Tablet-Viewer` | CloudFront does not add the header before forwarding the request to your origin\. For more information, see [Configuring Caching Based on the Device Type](header-caching.md#header-caching-web-device)\. | Yes | 
| `CloudFront-Viewer-Country` | CloudFront does not add the header before forwarding the request to your origin\. | Yes | 
| `Connection` | CloudFront replaces this header with `Connection: Keep-Alive` before forwarding the request to your origin\. | No | 
| `Content-Length` | CloudFront forwards the header to your origin\. | No | 
| `Content-MD5` | CloudFront forwards the header to your origin\. | Yes | 
| `Content-Type` | CloudFront forwards the header to your origin\. | Yes | 
| `Cookie` | If you configure CloudFront to forward cookies, it will forward the `Cookie` header field to your origin\. If you don't, CloudFront removes the `Cookie` header field\. For more information, see [Caching Content Based on Cookies](Cookies.md)\. | No | 
| `Date` | CloudFront forwards the header to your origin\. | Yes, but not recommended | 
| `Expect` | CloudFront removes the header\. | Yes | 
| `From` | CloudFront forwards the header to your origin\. | Yes | 
| `Host` | CloudFront sets the value to the domain name of the origin that is associated with the requested object\. You can't cache based on the Host header for Amazon S3 or MediaStore origins\. | Yes \(custom\) No \(S3 and MediaStore\) | 
| `If-Match` | CloudFront forwards the header to your origin\. | Yes | 
| `If-Modified-Since` | CloudFront forwards the header to your origin\. | Yes | 
| `If-None-Match` | CloudFront forwards the header to your origin\. | Yes | 
| `If-Range` | CloudFront forwards the header to your origin\. | Yes | 
| `If-Unmodified-Since` | CloudFront forwards the header to your origin\. | Yes | 
| `Max-Forwards` | CloudFront forwards the header to your origin\. | No | 
| `Origin` | CloudFront forwards the header to your origin\. | Yes | 
| `Pragma` | CloudFront forwards the header to your origin\. | No | 
| `Proxy-Authenticate` | CloudFront removes the header\. | No | 
| `Proxy-Authorization` | CloudFront removes the header\. | No | 
| `Proxy-Connection` | CloudFront removes the header\. | No | 
| `Range` | CloudFront forwards the header to your origin\. For more information, see [How CloudFront Processes Partial Requests for an Object \(Range GETs\)](RangeGETs.md)\. | Yes, by default | 
| `Referer` | CloudFront removes the header\. | Yes | 
| `Request-Range` | CloudFront forwards the header to your origin\. | No | 
| `TE` | CloudFront removes the header\. | No | 
| `Trailer` | CloudFront removes the header\. | No | 
| `Transfer-Encoding` | CloudFront forwards the header to your origin\. | No | 
| `Upgrade` | CloudFront removes the header, unless you've established a WebSocket connection\. | No \(except for WebSocket connections\) | 
| `User-Agent` | CloudFront replaces the value of this header field with `Amazon CloudFront`\. If you want CloudFront to cache your content based on the device the user is using, see [Configuring Caching Based on the Device Type](header-caching.md#header-caching-web-device)\. | Yes, but not recommended | 
| `Via` | CloudFront forwards the header to your origin\. | Yes | 
| `Warning` | CloudFront forwards the header to your origin\. | Yes | 
| `X-Amz-Cf-Id` | CloudFront adds the header to the viewer request before forwarding the request to your origin\. The header value contains an encrypted string that uniquely identifies the request\. | No | 
| `X-Edge-*` | CloudFront removes all `X-Edge-*` headers\. | No | 
| `X-Forwarded-For` | CloudFront forwards the header to your origin\. For more information, see [Client IP Addresses](#RequestCustomIPAddresses)\. | Yes | 
| `X-Forwarded-Proto` | CloudFront removes the header\. | No | 
| `X-Real-IP` | CloudFront removes the header\. | No | 

### HTTP Version<a name="RequestCustomHTTPVersion"></a>

CloudFront forwards requests to your custom origin using HTTP/1\.1\.

### Maximum Length of a Request and Maximum Length of a URL<a name="RequestCustomMaxRequestStringLength"></a>

The maximum length of a request, including the path, the query string \(if any\), and headers, is 20,480 bytes\.

CloudFront constructs a URL from the request\. The maximum length of this URL is 8192 bytes\.

If a request or a URL exceeds these maximums, CloudFront returns HTTP status code 413, Request Entity Too Large, to the viewer, and then terminates the TCP connection to the viewer\.

### OCSP Stapling<a name="request-custom-ocsp-stapling"></a>

When a viewer submits an HTTPS request for an object, either CloudFront or the viewer must confirm with the certificate authority \(CA\) that the SSL certificate for the domain has not been revoked\. OCSP stapling speeds up certificate validation by allowing CloudFront to validate the certificate and to cache the response from the CA, so the client doesn't need to validate the certificate directly with the CA\.

The performance improvement of OCSP stapling is more pronounced when CloudFront receives numerous HTTPS requests for objects in the same domain\. Each server in a CloudFront edge location must submit a separate validation request\. When CloudFront receives a lot of HTTPS requests for the same domain, every server in the edge location soon has a response from the CA that it can "staple" to a packet in the SSL handshake; when the viewer is satisfied that the certificate is valid, CloudFront can serve the requested object\. If your distribution doesn't get much traffic in a CloudFront edge location, new requests are more likely to be directed to a server that hasn't validated the certificate with the CA yet\. In that case, the viewer separately performs the validation step and the CloudFront server serves the object\. That CloudFront server also submits a validation request to the CA, so the next time it receives a request that includes the same domain name, it has a validation response from the CA\.

### Persistent Connections<a name="request-custom-persistent-connections"></a>

When CloudFront gets a response from your origin, it tries to maintain the connection for several seconds in case another request arrives during that period\. Maintaining a persistent connection saves the time required to re\-establish the TCP connection and perform another TLS handshake for subsequent requests\. 

For more information, including how to configure the duration of persistent connections, see [Origin Keep\-alive Timeout](distribution-web-values-specify.md#DownloadDistValuesOriginKeepaliveTimeout) in the section [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

### Protocols<a name="RequestCustomProtocols"></a>

CloudFront forwards HTTP or HTTPS requests to the origin server based on the following:
+ The protocol of the request that the viewer sends to CloudFront, either HTTP or HTTPS\.
+ The value of the **Origin Protocol Policy** field in the CloudFront console or, if you're using the CloudFront API, the `OriginProtocolPolicy` element in the `DistributionConfig` complex type\. In the CloudFront console, the options are **HTTP Only**, **HTTPS Only**, and **Match Viewer**\.

If you specify **HTTP Only** or **HTTPS Only**, CloudFront forwards requests to the origin server using the specified protocol, regardless of the protocol in the viewer request\.

If you specify **Match Viewer**, CloudFront forwards requests to the origin server using the protocol in the viewer request\. Note that CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols\.

**Important**  
If CloudFront forwards a request to the origin using the HTTPS protocol, and if the origin server returns an invalid certificate or a self\-signed certificate, CloudFront drops the TCP connection\.

For information about how to update a distribution using the CloudFront console, see [Updating a Distribution](HowToUpdateDistribution.md)\. For information about how to update a distribution using the CloudFront API, go to [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\. 

### Query Strings<a name="RequestCustomQueryStrings"></a>

You can configure whether CloudFront forwards query string parameters to your origin\. For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.

### Origin Connection Timeout and Attempts<a name="custom-origin-timeout-attempts"></a>

*Origin connection timeout* is the number of seconds that CloudFront waits when trying to establish a connection to the origin\.

*Origin connection attempts* is the number of times that CloudFront attempts to connect to the origin\.

Together, these settings determine how long CloudFront tries to connect to the origin before failing over to the secondary origin \(in the case of an origin group\) or returning an error response to the viewer\. By default, CloudFront waits as long as 30 seconds \(3 attempts of 10 seconds each\) before attempting to connect to the secondary origin or returning an error response\. You can reduce this time by specifying a shorter connection timeout, fewer attempts, or both\.

For more information, see [Controlling Origin Timeouts and Attempts](high_availability_origin_failover.md#controlling-attempts-and-timeouts)\.

### Origin Response Timeout<a name="request-custom-request-timeout"></a>

The *origin response timeout*, also known as the *origin read timeout* or *origin request timeout*, applies to both of the following:
+ The amount of time, in seconds, that CloudFront waits for a response after forwarding a request to the origin\.
+ The amount of time, in seconds, that CloudFront waits after receiving a packet of a response from the origin and before receiving the next packet\.

CloudFront behavior depends on the HTTP method of the viewer request:
+ `GET` and `HEAD` requests – If the origin doesn’t respond or stops responding within the duration of the response timeout, CloudFront drops the connection\. If the specified number of [origin connection attempts](distribution-web-values-specify.md#origin-connection-attempts) is more than 1, CloudFront tries again to get a complete response\. CloudFront tries up to 3 times, as determined by the value of the *origin connection attempts* setting\. If the origin doesn’t respond during the final attempt, CloudFront doesn’t try again until it receives another request for content on the same origin\.
+ `DELETE`, `OPTIONS`, `PATCH`, `PUT`, and `POST` requests – If the origin doesn’t respond within 30 seconds, CloudFront drops the connection and doesn’t try again to contact the origin\. The client can resubmit the request if necessary\.

For more information, including how to configure the origin response timeout, see [Origin Response Timeout](distribution-web-values-specify.md#DownloadDistValuesOriginResponseTimeout)\.

### Simultaneous Requests for the Same Object \(Traffic Spikes\)<a name="request-custom-traffic-spikes"></a>

When a CloudFront edge location receives a request for an object and either the object isn't currently in the cache or the object has expired, CloudFront immediately sends the request to your origin\. If there's a traffic spike—if additional requests for the same object arrive at the edge location before your origin responds to the first request—CloudFront pauses briefly before forwarding additional requests for the object to your origin\. Typically, the response to the first request will arrive at the CloudFront edge location before the response to subsequent requests\. This brief pause helps to reduce unnecessary load on your origin server\. If additional requests are not identical because, for example, you configured CloudFront to cache based on request headers or cookies, CloudFront forwards all of the unique requests to your origin\.

### User\-Agent Header<a name="request-custom-user-agent-header"></a>

If you want CloudFront to cache different versions of your objects based on the device that a user is using to view your content, we recommend that you configure CloudFront to forward one or more of the following headers to your custom origin:
+ `CloudFront-Is-Desktop-Viewer`
+ `CloudFront-Is-Mobile-Viewer`
+ `CloudFront-Is-SmartTV-Viewer`
+ `CloudFront-Is-Tablet-Viewer`

Based on the value of the `User-Agent` header, CloudFront sets the value of these headers to `true` or `false` before forwarding the request to your origin\. If a device falls into more than one category, more than one value might be `true`\. For example, for some tablet devices, CloudFront might set both `CloudFront-Is-Mobile-Viewer` and `CloudFront-Is-Tablet-Viewer` to `true`\. For more information about configuring CloudFront to cache based on request headers, see [Caching Content Based on Request Headers](header-caching.md)\.

You can configure CloudFront to cache objects based on values in the `User-Agent` header, but we don't recommend it\. The `User-Agent` header has many possible values, and caching based on those values would cause CloudFront to forward significantly more requests to your origin\. 

If you do not configure CloudFront to cache objects based on values in the `User-Agent` header, CloudFront adds a `User-Agent` header with the following value before it forwards a request to your origin:

`User-Agent = Amazon CloudFront`

CloudFront adds this header regardless of whether the request from the viewer includes a `User-Agent` header\. If the request from the viewer includes a `User-Agent` header, CloudFront removes it\.

## How CloudFront Processes Responses from Your Custom Origin Server<a name="ResponseBehaviorCustomOrigin"></a>

This topic contains information about how CloudFront processes responses from your custom origin\.

**Topics**
+ [100\-Continue Responses](#Response100Continue)
+ [Caching](#ResponseCustomCaching)
+ [Canceled Requests](#response-custom-canceled-requests)
+ [Content Negotiation](#ResponseCustomContentNegotiation)
+ [Cookies](#ResponseCustomCookies)
+ [Dropped TCP Connections](#ResponseCustomDroppedTCPConnections)
+ [HTTP Response Headers that CloudFront Removes or Replaces](#ResponseCustomRemovedHeaders)
+ [Maximum File Size](#ResponseCustomMaxFileSize)
+ [Origin Unavailable](#ResponseCustomOriginUnavailable)
+ [Redirects](#ResponseCustomRedirects)
+ [Transfer Encoding](#ResponseCustomTransferEncoding)

### 100\-Continue Responses<a name="Response100Continue"></a>

Your origin cannot send more than one 100\-Continue response to CloudFront\. After the first 100\-Continue response, CloudFront expects an HTTP 200 OK response\. If your origin sends another 100\-Continue response after the first one, CloudFront will return an error\.

### Caching<a name="ResponseCustomCaching"></a>
+ Ensure that the origin server sets valid and accurate values for the `Date` and `Last-Modified` header fields\.
+ If requests from viewers include the `If-Match` or `If-None-Match` request header fields, set the `ETag` response header field\. If you do not specify an `ETag` value, CloudFront ignores subsequent `If-Match` or `If-None-Match` headers\.
+ CloudFront normally respects a `Cache-Control: no-cache` header in the response from the origin\. For an exception, see [Simultaneous Requests for the Same Object \(Traffic Spikes\)](#request-custom-traffic-spikes)\.

### Canceled Requests<a name="response-custom-canceled-requests"></a>

If an object is not in the edge cache, and if a viewer terminates a session \(for example, closes a browser\) after CloudFront gets the object from your origin but before it can deliver the requested object, CloudFront does not cache the object in the edge location\.

### Content Negotiation<a name="ResponseCustomContentNegotiation"></a>

If your origin returns `Vary:*` in the response, and if the value of **Minimum TTL** for the corresponding cache behavior is **0**, CloudFront caches the object but still forwards every subsequent request for the object to the origin to confirm that the cache contains the latest version of the object\. CloudFront doesn't include any conditional headers, such as `If-None-Match` or `If-Modified-Since`\. As a result, your origin returns the object to CloudFront in response to every request\.

If your origin returns `Vary:*` in the response, and if the value of **Minimum TTL** for the corresponding cache behavior is any other value, CloudFront processes the `Vary` header as described in [HTTP Response Headers that CloudFront Removes or Replaces](#ResponseCustomRemovedHeaders)\.

### Cookies<a name="ResponseCustomCookies"></a>

If you enable cookies for a cache behavior, and if the origin returns cookies with an object, CloudFront caches both the object and the cookies\. Note that this reduces cacheability for an object\. For more information, see [Caching Content Based on Cookies](Cookies.md)\.

### Dropped TCP Connections<a name="ResponseCustomDroppedTCPConnections"></a>

If the TCP connection between CloudFront and your origin drops while your origin is returning an object to CloudFront, CloudFront behavior depends on whether your origin included a `Content-Length` header in the response:
+ **Content\-Length header** – CloudFront returns the object to the viewer as it gets the object from your origin\. However, if the value of the `Content-Length` header doesn't match the size of the object, CloudFront doesn't cache the object\.
+ **Transfer\-Encoding: Chunked** – CloudFront returns the object to the viewer as it gets the object from your origin\. However, if the chunked response is not complete, CloudFront does not cache the object\.
+ **No Content\-Length header** – CloudFront returns the object to the viewer and caches it, but the object may not be complete\. Without a `Content-Length` header, CloudFront cannot determine whether the TCP connection was dropped accidentally or on purpose\.

We recommend that you configure your HTTP server to add a `Content-Length` header to prevent CloudFront from caching partial objects\.

### HTTP Response Headers that CloudFront Removes or Replaces<a name="ResponseCustomRemovedHeaders"></a>

CloudFront removes or updates the following header fields before forwarding the response from your origin to the viewer: 
+ `Set-Cookie` – If you configure CloudFront to forward cookies, it will forward the `Set-Cookie` header field to clients\. For more information, see [Caching Content Based on Cookies](Cookies.md)\.
+ `Trailer`
+ `Transfer-Encoding` – If your origin returns this header field, CloudFront sets the value to `chunked` before returning the response to the viewer\.
+ `Upgrade`
+ `Vary` – Note the following:
  + If you configure CloudFront to forward any of the device\-specific headers to your origin \(`CloudFront-Is-Desktop-Viewer`, `CloudFront-Is-Mobile-Viewer`, `CloudFront-Is-SmartTV-Viewer`, `CloudFront-Is-Tablet-Viewer`\) and you configure your origin to return `Vary:User-Agent` to CloudFront, CloudFront returns `Vary:User-Agent` to the viewer\. For more information, see [Configuring Caching Based on the Device Type](header-caching.md#header-caching-web-device)\.
  + If you configure your origin to include either `Accept-Encoding` or `Cookie` in the `Vary` header, CloudFront includes the values in the response to the viewer\.
  + If you configure CloudFront to forward a whitelist of headers to your origin, and if you configure your origin to return the header names to CloudFront in the `Vary` header \(for example, `Vary:Accept-Charset,Accept-Language`\), CloudFront returns the `Vary` header with those values to the viewer\.
  + For information about how CloudFront processes a value of `*` in the `Vary` header, see [Content Negotiation](#ResponseCustomContentNegotiation)\.
  + If you configure your origin to include any other values in the `Vary` header, CloudFront removes the values before returning the response to the viewer\.
+ `Via` – CloudFront sets the value to the following in the response to the viewer:

  `Via: `*http\-version* *alphanumeric\-string*`.cloudfront.net (CloudFront)`

  For example, if the client makes a request over HTTP/1\.1, the value is something like the following:

  `Via: 1.1 1026589cc7887e7a0dc7827b4example.cloudfront.net (CloudFront)`

### Maximum File Size<a name="ResponseCustomMaxFileSize"></a>

The maximum size of a response body that CloudFront will return to the viewer is 20 GB\. This includes chunked transfer responses that don't specify the `Content-Length` header value\.

### Origin Unavailable<a name="ResponseCustomOriginUnavailable"></a>

If your origin server is unavailable and CloudFront gets a request for an object that is in the edge cache but that has expired \(for example, because the period of time specified in the `Cache-Control max-age` directive has passed\), CloudFront either serves the expired version of the object or serves a custom error page\. For more information about CloudFront behavior when you've configured custom error pages, see [How CloudFront Processes Errors When You Have Configured Custom Error Pages](HTTPStatusCodes.md#HTTPStatusCodes-custom-error-pages)\. 

In some cases, an object that is seldom requested is evicted and is no longer available in the edge cache\. CloudFront can't serve an object that has been evicted\.

### Redirects<a name="ResponseCustomRedirects"></a>

If you change the location of an object on the origin server, you can configure your web server to redirect requests to the new location\. After you configure the redirect, the first time a viewer submits a request for the object, CloudFront Front sends the request to the origin, and the origin responds with a redirect \(for example, `302 Moved Temporarily`\)\. CloudFront caches the redirect and returns it to the viewer\. CloudFront does not follow the redirect\. 

You can configure your web server to redirect requests to one of the following locations:
+ The new URL of the object on the origin server\. When the viewer follows the redirect to the new URL, the viewer bypasses CloudFront and goes straight to the origin\. As a result, we recommend that you not redirect requests to the new URL of the object on the origin\.
+ The new CloudFront URL for the object\. When the viewer submits the request that contains the new CloudFront URL, CloudFront gets the object from the new location on your origin, caches it at the edge location, and returns the object to the viewer\. Subsequent requests for the object will be served by the edge location\. This avoids the latency and load associated with viewers requesting the object from the origin\. However, every new request for the object will incur charges for two requests to CloudFront\.

### Transfer Encoding<a name="ResponseCustomTransferEncoding"></a>

CloudFront supports only the `chunked` value of the `Transfer-Encoding` header\. If your origin returns `Transfer-Encoding: chunked`, CloudFront returns the object to the client as the object is received at the edge location, and caches the object in chunked format for subsequent requests\.

If the viewer makes a `Range GET` request and the origin returns `Transfer-Encoding: chunked`, CloudFront returns the entire object to the viewer instead of the requested range\.

We recommend that you use chunked encoding if the content length of your response cannot be predetermined\. For more information, see [Dropped TCP Connections](#ResponseCustomDroppedTCPConnections)\. 