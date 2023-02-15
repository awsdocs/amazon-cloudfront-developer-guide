# Serving compressed files<a name="ServingCompressedFiles"></a>

You can use CloudFront to automatically compress certain types of objects \(files\) and serve the compressed objects when viewers \(web browsers or other clients\) support them\. Viewers indicate their support for compressed objects with the `Accept-Encoding` HTTP header\. CloudFront can compress objects using the Gzip and Brotli compression formats\. When the viewer supports both formats, CloudFront prefers Brotli\.

**Note**  
The Chrome and Firefox web browsers support Brotli compression only when the request is sent using HTTPS\. These browsers do not support Brotli with HTTP requests\.

When requested objects are compressed, downloads can be faster because the objects are smaller—in some cases, less than a quarter the size of the original\. Especially for JavaScript and CSS files, faster downloads can result in faster rendering of webpages for your users\. In addition, because the cost of CloudFront data transfer is based on the total amount of data served, serving compressed objects can be less expensive than serving them uncompressed\.

Some custom origins can also compress objects\. Your origin might be able to compress objects that CloudFront doesn’t compress \(see [File types that CloudFront compresses](#compressed-content-cloudfront-file-types)\)\. If your origin returns a compressed object to CloudFront, CloudFront detects that the object is compressed based on the presence of a `Content-Encoding` header and doesn’t compress the object again\.

## Configuring CloudFront to compress objects<a name="compressed-content-cloudfront-configuring"></a>

To configure CloudFront to compress objects, update the cache behavior that you want to serve the compressed objects by doing ***all*** of the following:

1. Make sure the **Compress objects automatically** setting is **Yes**\. \(In AWS CloudFormation or the CloudFront API, set `Compress` to `true`\.\)

1. Use a [cache policy](controlling-the-cache-key.md) to specify caching settings, and make sure the **Gzip** and **Brotli** settings are both enabled\. \(In AWS CloudFormation or the CloudFront API, set `EnableAcceptEncodingGzip` and `EnableAcceptEncodingBrotli` to `true`\.\)

1. Make sure the TTL values in the cache policy are set to a value greater than zero\. When you set the TTL values to zero, caching is disabled and CloudFront doesn’t compress objects\.

To update a cache behavior, you can use any of the following tools:
+ The [CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home)
+ [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_CloudFront.html)
+ The [AWS SDKs and command line tools](http://aws.amazon.com/getting-started/tools-sdks/)

## How CloudFront compression works<a name="compressed-content-cloudfront-how-it-works"></a>

When you configure CloudFront to compress objects \(see the previous section\), here’s how it works:

1. A viewer requests an object\. The viewer includes the `Accept-Encoding` HTTP header in the request, and the header value includes `gzip`, `br`, or both\. This indicates that the viewer supports compressed objects\. When the viewer supports both Gzip and Brotli, CloudFront prefers Brotli\.
**Note**  
The Chrome and Firefox web browsers support Brotli compression only when the request is sent using HTTPS\. These browsers do not support Brotli with HTTP requests\.

1. At the edge location, CloudFront checks the cache for a compressed copy of the requested object\.

1. If the compressed object is already in the cache, CloudFront sends it to the viewer and skips the remaining steps\.

   If the compressed object is not in the cache, CloudFront forwards the request to the origin\.
**Note**  
If an uncompressed copy of the object is already in the cache, CloudFront might send it to the viewer without forwarding the request to the origin\. For example, this can happen when CloudFront previously skipped compression for this object due to an unusually busy host\. When this happens, CloudFront caches the uncompressed object and continues to serve it until the object expires, is evicted, or is invalidated\.

1. If the origin returns a compressed object, as indicated by the presence of a `Content-Encoding` header in the HTTP response, CloudFront sends the compressed object to the viewer, adds it to the cache, and skips the remaining steps\. CloudFront doesn’t compress the object again\.

   If the origin returns an uncompressed object to CloudFront \(there’s no `Content-Encoding` header in the HTTP response\), CloudFront determines whether the object is compressible\. For more information about how CloudFront determines whether an object is compressible, see the following section\.

1. If the object is compressible, CloudFront compresses it, sends it to the viewer, and adds it to the cache\.

## Notes about CloudFront compression<a name="compressed-content-cloudfront-notes"></a>

The following list provides more information about when CloudFront compresses objects\.

**Request uses HTTP 1\.0**  
If a request to CloudFront uses HTTP 1\.0, CloudFront removes the `Accept-Encoding` header and does not compress the object in the response\.

**`Accept-Encoding` request header**  
If the `Accept-Encoding` header is missing from the viewer request, or if it doesn’t contain `gzip` or `br` as a value, CloudFront does not compress the object in the response\. If the `Accept-Encoding` header includes additional values such as `deflate`, CloudFront removes them before forwarding the request to the origin\.  
When CloudFront is [configured to compress objects](#compressed-content-cloudfront-configuring), it includes the `Accept-Encoding` header in the cache key and in origin requests automatically\. However, if the `Accept-Encoding` header is explicitly listed in the cache policy \(or in the legacy cache settings\), CloudFront does not compress the object in the response\.

**Dynamic content**  
CloudFront does not always compress dynamic content\. Sometimes responses for dynamic content are compressed, and sometimes they are not\.

**Content is already cached when you configure CloudFront to compress objects**  
CloudFront compresses objects when it gets them from the origin\. When you configure CloudFront to compress objects, CloudFront doesn’t compress objects that are already cached in edge locations\. In addition, when a cached object expires in an edge location and CloudFront forwards another request for the object to your origin, CloudFront doesn’t compress the object when your origin returns an HTTP status code 304, which means that the edge location already has the latest version of the object\. If you want CloudFront to compress objects that are already cached in edge locations, you need to invalidate those objects\. For more information, see [Invalidating files](Invalidation.md)\.

**Origin is already configured to compress objects**  
If you configure CloudFront to compress objects and the origin also compresses objects, the origin should include a `Content-Encoding` header, which indicates to CloudFront that the object is already compressed\. When a response from an origin includes the `Content-Encoding` header, CloudFront does not compress the object, regardless of the header’s value\. CloudFront sends the response to the viewer and caches the object in the edge location\.

**File types that CloudFront compresses**  
For a complete list of the file types that CloudFront compresses, see [File types that CloudFront compresses](#compressed-content-cloudfront-file-types)\.

**Size of objects that CloudFront compresses**  
CloudFront compresses objects that are between 1,000 bytes and 10,000,000 bytes in size\.

**`Content-Length` header**  
The origin must include a `Content-Length` header in the response, which CloudFront uses to determine whether the size of the object is in the range that CloudFront compresses\. If the `Content-Length` header is missing, contains an invalid value, or contains a value outside the range of sizes that CloudFront compresses, CloudFront does not compress the object\.

**HTTP status code of the response**  
CloudFront compresses objects only when the HTTP status code of the response is `200`, `403`, or `404`\.

**Response has no body**  
When the HTTP response from the origin has no body, there is nothing for CloudFront to compress\.

**`ETag` header**  
CloudFront sometimes modifies the `ETag` header in the HTTP response when it compresses objects\. For more information, see [`ETag` header conversion](#compressed-content-cloudfront-etag-header)\.

**CloudFront is busy**  
In rare cases when a CloudFront host is unusually busy, CloudFront might not compress some objects\.  
If CloudFront skips compression for an object due to an unusually busy host, it caches the uncompressed object and continues to serve it until the object expires, is evicted, or is invalidated\.

## File types that CloudFront compresses<a name="compressed-content-cloudfront-file-types"></a>

If you configure CloudFront to compress objects, CloudFront only compresses objects that have one of the following values in the `Content-Type` response header:
+ `application/dash+xml`
+ `application/eot`
+ `application/font`
+ `application/font-sfnt`
+ `application/javascript`
+ `application/json`
+ `application/opentype`
+ `application/otf`
+ `application/pkcs7-mime`
+ `application/protobuf`
+ `application/rss+xml`
+ `application/truetype`
+ `application/ttf`
+ `application/vnd.apple.mpegurl`
+ `application/vnd.mapbox-vector-tile`
+ `application/vnd.ms-fontobject`
+ `application/xhtml+xml`
+ `application/xml`
+ `application/x-font-opentype`
+ `application/x-font-truetype`
+ `application/x-font-ttf`
+ `application/x-httpd-cgi`
+ `application/x-javascript`
+ `application/x-mpegurl`
+ `application/x-opentype`
+ `application/x-otf`
+ `application/x-perl`
+ `application/x-ttf`
+ `font/eot`
+ `font/opentype`
+ `font/otf`
+ `font/ttf`
+ `image/svg+xml`
+ `text/css`
+ `text/csv`
+ `text/html`
+ `text/javascript`
+ `text/js`
+ `text/plain`
+ `text/richtext`
+ `text/tab-separated-values`
+ `text/xml`
+ `text/x-component`
+ `text/x-java-source`
+ `text/x-script`
+ `vnd.apple.mpegurl`

Note that, image and video formats (such as `image/jpeg`, `image/png`, and `video/mpeg4`) are almost always already compressed, so CloudFront doesn't compress them. Re-compressing an already compressed response rarely reduces file size, and clients might exhibit unexpected behavior when receiving a response of this kind.

## `ETag` header conversion<a name="compressed-content-cloudfront-etag-header"></a>

When the uncompressed object from the origin includes a valid, strong `ETag` HTTP header, and CloudFront compresses the object, CloudFront also converts the strong `ETag` header value to a weak `ETag`, and returns the weak `ETag` value to the viewer\. Viewers can store the weak `ETag` value and use it to send conditional requests with the `If-None-Match` HTTP header\. This allows viewers, CloudFront, and the origin to treat the compressed and uncompressed versions of an object as semantically equivalent, which reduces unnecessary data transfer\.

A valid, strong `ETag` header value begins with a double quote character \(`"`\)\. To convert the strong `ETag` value to a weak one, CloudFront adds the characters `W/` to the beginning of the strong `ETag` value\.

When the object from the origin includes a weak `ETag` header value \(a value that begins with the characters `W/`\), CloudFront does not modify this value, and returns it to the viewer as received from the origin\.

When the object from the origin includes an invalid `ETag` header value \(the value does not begin with `"` or with `W/`\), CloudFront removes the `ETag` header and returns the object to the viewer without the `ETag` response header\.

For more information, see the following pages in the MDN web docs:
+ [Directives](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#Directives) \(`ETag` HTTP header\)
+ [Weak validation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Conditional_requests#Weak_validation) \(HTTP conditional requests\)
+ [`If-None-Match` HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-None-Match)
