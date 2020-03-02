# Serving Compressed Files<a name="ServingCompressedFiles"></a>

You can configure CloudFront to automatically compress files of certain types and serve the compressed files when viewer requests include `Accept-Encoding: gzip` in the request header\. When content is compressed, downloads are faster because the files are smaller—in some cases, less than a quarter the size of the original\. Especially for JavaScript and CSS files, faster downloads translates into faster rendering of web pages for your users\. In addition, because the cost of CloudFront data transfer is based on the total amount of data served, serving compressed files is less expensive than serving uncompressed files\. 

**Important**  
A viewer request must include `Accept-Encoding: gzip` in the request header, or CloudFront won't compress the requested file\. 

If you're using a custom or Amazon S3 origin, you can configure your origin to compress files with or without CloudFront compression\. Your origin can compress file types that CloudFront doesn't compress\. \(See [File Types that CloudFront Compresses](#compressed-content-cloudfront-file-types)\.\) If your origin returns a compressed file to CloudFront, CloudFront detects that the file is compressed based on the value of the `Content-Encoding` header and doesn't compress the file again\.

**Topics**
+ [Configuring a CloudFront Distribution to Compress Content](#compressed-content-cloudfront-configuring)
+ [Using CloudFront to Compress Your Content](#compressed-content-cloudfront)
+ [Using a Custom Origin to Compress Your Content](#compressed-content-custom-origin)
+ [Using an Amazon S3 Origin to Compress Your Content](#compressed-content-S3-origin)
+ [Serving Compressed Files When Your Origin Server Is Running IIS](#serving-compressed-files-iis)
+ [Serving Compressed Files When Your Origin Server Is Running NGINX](#serving-compressed-files-nginx)

## Configuring a CloudFront Distribution to Compress Content<a name="compressed-content-cloudfront-configuring"></a>

To configure a distribution to compress your content, update the cache behaviors that you want to serve the compressed content by using one of the following methods: 
+ **CloudFront console** – Update the **Compress objects automatically** setting\. For more information, see [Creating a Distribution](distribution-web-creating-console.md)\.
+ **CloudFront API** – Change the value of the `Compress` element to `true`\. For more information, see [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) or [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ **One of the AWS SDKs** – See the SDK documentation on the [AWS Documentation](http://aws.amazon.com/documentation/) page\.
+ **The AWS CLI** – For more information, see [create\-distribution](https://docs.aws.amazon.com/cli/latest/reference/cloudfront/create-distribution.html) or [update\-distribution](https://docs.aws.amazon.com/cli/latest/reference/cloudfront/update-distribution.html) in the *AWS CLI Command Reference*\.

## Using CloudFront to Compress Your Content<a name="compressed-content-cloudfront"></a>

CloudFront can compress files both for Amazon S3 origins and for custom origins\. When you configure CloudFront to compress your content, you specify the setting in your cache behaviors\. 

When you configure CloudFront to compress your content, here's how CloudFront serves your content: 

1. You create or update a CloudFront distribution and configure CloudFront to compress content\. 

1. A viewer requests a file\. The viewer adds the `Accept-Encoding: gzip` header to the request\. This indicates that the viewer supports compressed content\.

1. At the edge location, CloudFront checks the cache for a compressed version of the file that is referenced in the request\.

1. If the compressed file is already in the cache, CloudFront returns the file to the viewer and skips the remaining steps\. 

1. If the compressed file is not in the cache, CloudFront forwards the request to the origin server, which can be either an Amazon S3 bucket or a custom origin\. 
**Note**  
If CloudFront has an uncompressed version of the file in the cache, it still forwards a request to the origin\. 

1. The origin server returns an uncompressed version of the requested file to CloudFront\.

1. CloudFront determines whether the file is compressible:
   + The file must be of a type that CloudFront compresses\.
   + The file size must be between 1,000 and 10,000,000 bytes\.
   + The response must include a `Content-Length` header so CloudFront can determine whether the size of the file is in the range that CloudFront compresses\. If the `Content-Length` header is missing, CloudFront won't compress the file\.
   + The response must not include a `Content-Encoding` header\.

1. If the file is compressible, CloudFront compresses it, returns the compressed file to the viewer, and adds it to the cache\. 

1. The viewer uncompresses the file\.

Note the following:

**File types that CloudFront compresses**  
CloudFront compresses files for a large number of file types\. For a complete list, see [File Types that CloudFront Compresses](#compressed-content-cloudfront-file-types)\.

**Size of files that CloudFront compresses**  
CloudFront compresses files that are between 1,000 bytes and 10,000,000 bytes in size\.

**Content\-Length header**  
The origin must include a `Content-Length` header in the response so CloudFront can determine whether the size of the file is in the range that CloudFront compresses\. If the `Content-Length` header is missing, CloudFront won't compress the file\.

**Etag header**  
If you configure CloudFront to compress content, CloudFront removes the `ETag` response header from the files that it compresses\. When the `ETag` header is present, CloudFront and your origin can use it to determine whether the version of a file in a CloudFront edge cache is identical to the version on the origin server\. However, after compression the two versions are no longer identical\. As a result, when a compressed file expires and CloudFront forwards another request to your origin, your origin always returns the file to CloudFront instead of an HTTP status code 304 \(Not Modified\)\.

**Content already in edge locations when you configure CloudFront to compress files**  
CloudFront compresses files in each edge location when it gets the files from your origin\. When you configure CloudFront to compress your content, it doesn't compress files that are already in edge locations\. In addition, when a file expires in an edge location and CloudFront forwards another request for the file to your origin, CloudFront doesn't compress the file if your origin returns an HTTP status code 304, which means that the edge location already has the latest version of the file\. If you want CloudFront to compress the files that are already in edge locations, you'll need to invalidate those files\. For more information, see [Invalidating Files](Invalidation.md)\.

**Custom origin is already configured to compress files**  
If you configure CloudFront to compress files and CloudFront is forwarding requests to a custom origin that is also configured to compress files, the custom origin will include a `Content-Encoding` header, which indicates that the file that the origin returned to CloudFront has already been compressed\. CloudFront returns the cached file to the viewer and caches it in the edge location\.  
CloudFront does not compress a file if the response includes a `Content-Encoding` header, regardless of the value\.

**Request doesn't include Accept\-Encoding: gzip**  
If the `Accept-Encoding` header is missing from the request, CloudFront serves uncompressed content\. If the `Accept-Encoding` header includes additional values such as `deflate` or `sdch`, CloudFront removes them before forwarding the request to the origin server\.

**Request that uses HTTP 1\.0**  
If a request to CloudFront uses HTTP 1\.0, CloudFront removes the `Accept-Encoding` header and serves uncompressed content\.

**CloudFront is busy**  
In rare cases, when a CloudFront edge location is unusually busy, some files might not be compressed\. 

### File Types that CloudFront Compresses<a name="compressed-content-cloudfront-file-types"></a>

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
+ `application/rss+xml`
+ `application/truetype`
+ `application/ttf`
+ `application/vnd.apple.mpegurl`
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

## Using a Custom Origin to Compress Your Content<a name="compressed-content-custom-origin"></a>

CloudFront can compress some types of files for you \(see [File Types that CloudFront Compresses](#compressed-content-cloudfront-file-types)\), by using gzip\. But if you want to compress other file types, or if you want to use a compression algorithm that isn’t gzip, such as brotli, you can compress the files on your own server, and then serve the files by using CloudFront\.

To use CloudFront to serve a file with a compression algorithm that isn’t gzip, set up CloudFront to cache based on the `Accept-Encoding` header\. When you do this, CloudFront does not make any changes to the `Accept-Encoding` header and your origin can return an appropriate compressed file for the viewer\.

When your origin returns a compressed file to CloudFront, include a `Content-Encoding` header to indicate to CloudFront that the file is already compressed\. Then CloudFront simply returns the compressed file to the viewer\.

## Using an Amazon S3 Origin to Compress Your Content<a name="compressed-content-S3-origin"></a>

When you use Amazon S3 to store your content, you can use CloudFront to compress content if you want to use the gzip compression algorithm\. But you might want to use other compression algorithms, such as brotli, instead of gzip, or in addition to gzip\.

After you compress the files, you can serve them with CloudFront by doing the following, for example:
+ Configure CloudFront to cache based on the `Accept-Encoding` header\.
+ Using a Lambda@Edge function that triggers on origin requests, change the URI to point to the compressed file that you want to return, based on the `Accept-Encoding` header\.
+ Add content\-encoding metadata to files that you store on S3 so that the viewer can determine the format of the compressed file\. For more information, see [How Do I Add Metadata to an S3 Object?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/add-object-metadata.html) in the Amazon Simple Storage Service Console User Guide\.

## Serving Compressed Files When Your Origin Server Is Running IIS<a name="serving-compressed-files-iis"></a>

By default, IIS does not serve compressed content for requests that come through proxy servers such as CloudFront\. If you're using IIS and if you configured IIS to compress content by using the `httpCompression` element, change the value of the `noCompressionForProxies` attribute to `false` so IIS will return compressed content to CloudFront\.

In addition, if you have compressed objects that are requested less frequently than every few seconds, you might have to change the values of `frequentHitThreshold` and `frequentHitTimePeriod`\.

For more information, refer to the IIS documentation on the Microsoft website\.

## Serving Compressed Files When Your Origin Server Is Running NGINX<a name="serving-compressed-files-nginx"></a>

When CloudFront forwards a request to the origin server, it includes a `Via` header\. This causes NGINX to interpret the request as proxied and, by default, NGINX disables compression for proxied requests\. If your version of NGINX includes the `gzip_proxied` setting, change the value to `any` so that NGINX will return compressed content to CloudFront\. For more information, see the NGINX documentation for the module `ngx_http_gzip_module`\. 