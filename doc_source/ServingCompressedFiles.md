# Serving compressed files<a name="ServingCompressedFiles"></a>

You can use CloudFront to automatically compress files of certain types and serve the compressed files when viewers support them \(viewers indicate their support for compressed files with the `Accept-Encoding` HTTP header\)\. CloudFront can compress files using the Gzip and Brotli compression formats\. When the viewer supports both formats, CloudFront uses Brotli\.

**Note**  
The Chrome and Firefox web browsers support Brotli compression only when the request is sent using HTTPS\. These browsers do not support Brotli with HTTP requests\.

When content is compressed, downloads can be faster because the files are smaller—in some cases, less than a quarter the size of the original\. Especially for JavaScript and CSS files, faster downloads can result in faster rendering of webpages for your users\. In addition, because the cost of CloudFront data transfer is based on the total amount of data served, serving compressed files can be less expensive than serving uncompressed files\.

If you’re using a custom origin, you can configure your origin to compress files\. Your origin might be able to compress files that CloudFront doesn’t compress \(see [File types that CloudFront compresses](#compressed-content-cloudfront-file-types)\)\. If your origin returns a compressed file to CloudFront, CloudFront detects that the file is compressed based on the value of the `Content-Encoding` header and doesn’t compress the file again\.

If you configure CloudFront to serve compressed content, you should also [cache compressed objects](controlling-the-cache-key.md#cache-policy-compressed-objects)\.

## Configuring a CloudFront distribution to compress content<a name="compressed-content-cloudfront-configuring"></a>

To configure a distribution to compress content, update the cache behavior that you want to serve the compressed content by doing the following:

1. Make sure the **Compress Objects Automatically** setting is enabled\. \(In AWS CloudFormation or the CloudFront API, set `Compress` to `true`\.\)

1. Use a [cache policy](controlling-the-cache-key.md) to specify caching settings, and make sure the **Gzip** and **Brotli** settings are both enabled\. \(In AWS CloudFormation or the CloudFront API, set `EnableAcceptEncodingGzip` and `EnableAcceptEncodingBrotli` to `true`\.\)

1. Make sure the TTL values in the cache policy are set to a value higher than zero\. When you set the TTL values to zero, caching is disabled and CloudFront doesn’t compress content\.

To update a cache behavior, you can use any of the following tools:
+ The [CloudFront console](https://console.aws.amazon.com/cloudfront/home)
+ [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_CloudFront.html)
+ [The AWS SDKs and command line tools](http://aws.amazon.com/getting-started/tools-sdks/)

## Using CloudFront to compress your content<a name="compressed-content-cloudfront"></a>

CloudFront can compress files both for Amazon S3 origins and for custom origins\. When you configure CloudFront to compress your content, you specify the setting in one or more cache behaviors\. 

When you configure CloudFront to compress your content, here’s how CloudFront serves your content:

1. You create or update a CloudFront distribution and configure CloudFront to compress content\. For more information, see the previous section\.

1. A viewer requests a file\. The viewer includes the `Accept-Encoding` HTTP header in the request, and the header values include `gzip`, `br`, or both\. This indicates that the viewer supports compressed content\. When the viewer supports both formats, CloudFront uses Brotli\.
**Note**  
The Chrome and Firefox web browsers support Brotli compression only when the request is sent using HTTPS\. These browsers do not support Brotli with HTTP requests\.

1. At the edge location, CloudFront checks the cache for a compressed version of the requested file\.

1. If the compressed file is already in the cache, CloudFront returns the file to the viewer and skips the remaining steps\. 

1. If the compressed file is not in the cache, CloudFront sends the request to the origin server, which can be either an Amazon S3 bucket or a custom origin\. 
**Note**  
If CloudFront has an uncompressed version of the file in the cache, it still sends a request to the origin\.

1. The origin server returns an uncompressed version of the requested file to CloudFront\.

1. CloudFront determines whether the file is compressible:
   + The file must be of a type that CloudFront compresses\. For more information, see [File types that CloudFront compresses](#compressed-content-cloudfront-file-types)\.
   + The file size must be between 1,000 and 10,000,000 bytes\.
   + The response must include a `Content-Length` header so CloudFront can determine whether the size of the file is in the range that CloudFront compresses\. If the `Content-Length` header is missing, CloudFront won’t compress the file\.
   + The response must not include a `Content-Encoding` header\.

1. If the file is compressible, CloudFront compresses it, returns the compressed file to the viewer, and adds it to the cache\. 

1. The viewer uncompresses the file\.

Note the following:

**File types that CloudFront compresses**  
CloudFront compresses files for a large number of file types\. For a complete list, see [File types that CloudFront compresses](#compressed-content-cloudfront-file-types)\.

**Size of files that CloudFront compresses**  
CloudFront compresses files that are between 1,000 bytes and 10,000,000 bytes in size\.

**Content\-Length header**  
The origin must include a `Content-Length` header in the response so CloudFront can determine whether the size of the file is in the range that CloudFront compresses\. If the `Content-Length` header is missing, CloudFront won’t compress the file\.

**ETag header**  
When the uncompressed object from the origin includes a valid, strong `ETag` HTTP header, CloudFront converts the strong `ETag` header value to a weak `ETag`, and returns the weak `ETag` value to the viewer\. Viewers can store the weak `ETag` value and use it to send conditional requests with the `If-None-Match` HTTP header\. This allows viewers, CloudFront, and the origin to treat the compressed and uncompressed versions of an object as semantically equivalent, which reduces unnecessary data transfer\.  
A valid, strong `ETag` header value begins with a double quote character \(`"`\)\. To convert the strong `ETag` value to a weak one, CloudFront adds the characters `W/` to the beginning of the strong `ETag` value\.  
When the object from the origin includes a weak `ETag` header value \(a value that begins with the characters `W/`\), CloudFront does not modify this value, and returns it to the viewer as received from the origin\.  
When the object from the origin includes an invalid `ETag` header value \(the value does not begin with `"` or with `W/`\), CloudFront removes the `ETag` header and returns the object to the viewer without the `ETag` response header\.  
For more information, see the following pages in the MDN web docs:  
+ [Directives](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#Directives) \(`ETag` HTTP header\)
+ [Weak validation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Conditional_requests#Weak_validation) \(HTTP conditional requests\)
+ [`If-None-Match` HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-None-Match)

**Content already in edge locations when you configure CloudFront to compress files**  
CloudFront compresses files in each edge location when it gets the files from your origin\. When you configure CloudFront to compress your content, CloudFront doesn’t compress files that are already cached in edge locations\. In addition, when a file expires in an edge location and CloudFront sends another request for the file to your origin, CloudFront doesn’t compress the file if your origin returns an HTTP status code 304, which means that the edge location already has the latest version of the file\. If you want CloudFront to compress the files that are already cached in edge locations, you need to invalidate those files\. For more information, see [Invalidating Files](Invalidation.md)\.

**Origin is already configured to compress files**  
If you configure CloudFront to compress files and the origin also compresses files, the origin includes a `Content-Encoding` header, which indicates that the file is already compressed\. CloudFront returns the cached file to the viewer and caches it in the edge location\.  
CloudFront does not compress a file if the response includes a `Content-Encoding` header, regardless of the header’s value\.

**Request doesn’t include the Accept\-Encoding header**  
If the `Accept-Encoding` header is missing from the viewer request, CloudFront serves uncompressed content\. If the `Accept-Encoding` header includes additional values such as `deflate` or `sdch`, CloudFront removes them before forwarding the request to the origin server\.

**Request uses HTTP 1\.0**  
If a request to CloudFront uses HTTP 1\.0, CloudFront removes the `Accept-Encoding` header and serves uncompressed content\.

**CloudFront is busy**  
In rare cases, when a CloudFront edge location is unusually busy, some files might not be compressed\.

### File types that CloudFront compresses<a name="compressed-content-cloudfront-file-types"></a>

If you configure CloudFront to compress your content, CloudFront compresses files that have the following values in the `Content-Type` header:
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