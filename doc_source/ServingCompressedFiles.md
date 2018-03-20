# Serving Compressed Files<a name="ServingCompressedFiles"></a>

You can configure CloudFront to automatically compress files of certain types and serve the compressed files when viewer requests include `Accept-Encoding: gzip` in the request header\. When content is compressed, downloads are faster because the files are smaller—in some cases, less than a quarter the size of the original\. Especially for JavaScript and CSS files, faster downloads translates into faster rendering of web pages for your users\. In addition, because the cost of CloudFront data transfer is based on the total amount of data served, serving compressed files is less expensive than serving uncompressed files\. 

**Important**  
A viewer request must include `Accept-Encoding: gzip` in the request header, or CloudFront won't compress the requested file\. 

If you're using a custom origin, you can configure your origin to compress files with or without CloudFront compression\. Your origin can compress file types that CloudFront doesn't compress\. \(See [File Types that CloudFront Compresses](#compressed-content-cloudfront-file-types)\.\) If your origin returns a compressed file to CloudFront, CloudFront detects that the file is compressed based on the value of the `Content-Encoding` header and doesn't compress the file again\.

**Topics**
+ [Using CloudFront to Compress Your Content](#compressed-content-cloudfront)
+ [Using a Custom Origin to Compress Your Content](#compressed-content-custom-origin)

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
CloudFront compresses files in a large number of file types\. For a complete list, see [File Types that CloudFront Compresses](#compressed-content-cloudfront-file-types)\.

**Size of files that CloudFront compresses**  
CloudFront compresses files that are between 1,000 bytes and 10,000,000 bytes in size\.

**Content\-Length header**  
The origin must include a `Content-Length` header in the response so CloudFront can determine whether the size of the file is in the range that CloudFront compresses\. If the `Content-Length` header is missing, CloudFront won't compress the file\.

**Etag header**  
If you configure CloudFront to compress content, CloudFront removes the `ETag` response header from the files that it compresses\. When the `ETag` header is present, CloudFront and your origin can use it to determine whether the version of a file in a CloudFront edge cache is identical to the version on the origin server\. However, after compression the two versions are no longer identical\. As a result, when a compressed file expires and CloudFront forwards another request to your origin, your origin always returns the file to CloudFront instead of an HTTP status code 304 \(Not Modified\)\.

**Content already in edge locations when you configure CloudFront to compress files**  
CloudFront compresses files in each edge location when it gets the files from your origin\. When you configure CloudFront to compress your content, it doesn't compress files that are already in edge locations\. In addition, when a file expires in an edge location and CloudFront forwards another request for the file to your origin, CloudFront doesn't compress the file if your origin returns an HTTP status code 304, which means that the edge location already has the latest version of the file\. If you want CloudFront to compress the files that are already in edge locations, you'll need to invalidate those files\. For more information, see [Invalidating Objects \(Web Distributions Only\)](Invalidation.md)\.

**Custom origin is already configured to compress files**  
If you configure CloudFront to compress files and CloudFront is forwarding requests to a custom origin that is also configured to compress files, the custom origin will include a `Content-Encoding: gzip` header, which indicates that the file that the origin returned to CloudFront has already been compressed\. CloudFront returns the cached file to the viewer and caches it in the edge location\.  
CloudFront does not compress a file if the response includes a `Content-Encoding` header, regardless of the value\.

**Request doesn't include Accept\-Encoding: gzip**  
If the `Accept-Encoding` header is missing from the request, CloudFront serves uncompressed content\. If the `Accept-Encoding` header includes additional values such as `deflate` or `sdch`, CloudFront removes them before forwarding the request to the origin server\.

**CloudFront is busy**  
In rare cases, when a CloudFront edge location is unusually busy, some files might not be compressed\. 

### Configuring a CloudFront Distribution to Compress Content<a name="compressed-content-cloudfront-configuring"></a>

To configure a web distribution to compress your content, you update the applicable cache behaviors by using one of the following methods: 
+ **CloudFront console** – Update the **Compress objects automatically** setting\. For more information, see [Creating or Updating a Web Distribution Using the CloudFront Console](distribution-web-creating-console.md)\.
+ **CloudFront API** – Change the value of the `Compress` element to `true`\. For more information, see [POST Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateDistribution.html) \(to create a new distribution\) or [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) \(to update an existing distribution\)\.
+ **One of the AWS SDKs** – See the applicable SDK documentation on the [AWS Documentation](http://aws.amazon.com/documentation/) page\.
+ **The AWS CLI** – For more information, see [create\-distribution](http://docs.aws.amazon.com/cli/latest/reference/cloudfront/create-distribution.html) or [update\-distribution](http://docs.aws.amazon.com/cli/latest/reference/cloudfront/update-distribution.html) in the *AWS CLI Command Reference*\.

### File Types that CloudFront Compresses<a name="compressed-content-cloudfront-file-types"></a>

If you configure CloudFront to compress your content, CloudFront compresses files that have the following values in the `Content-Type` header:


****  

|  |  | 
| --- |--- |
| application/eot | application/x\-otf | 
| application/font | application/x\-perl | 
| application/font\-sfnt | application/x\-ttf | 
| application/javascript | font/eot | 
| application/json | font/ttf | 
| application/opentype | font/otf | 
| application/otf | font/opentype | 
| application/pkcs7\-mime | image/svg\+xml | 
| application/truetype | text/css | 
| application/ttf | text/csv | 
| application/vnd\.ms\-fontobject | text/html | 
| application/xhtml\+xml | text/javascript | 
| application/xml | text/js | 
| application/xml\+rss | text/plain | 
| application/x\-font\-opentype | text/richtext | 
| application/x\-font\-truetype | text/tab\-separated\-values | 
| application/x\-font\-ttf | text/xml | 
| application/x\-httpd\-cgi | text/x\-script | 
| application/x\-javascript | text/x\-component | 
| application/x\-mpegurl | text/x\-java\-source | 
| application/x\-opentype |   | 

## Using a Custom Origin to Compress Your Content<a name="compressed-content-custom-origin"></a>

If you want to compress file types that CloudFront doesn't compress, you can configure your custom origin to compress files of those types using gzip\. CloudFront doesn't support other compression algorithms\. When your origin returns the compressed file to CloudFront, it will include a `Content-Encoding: gzip` header, which indicates to CloudFront that the file is already compressed\. 

**Note**  
CloudFront does not compress a file if the response includes a `Content-Encoding` header, regardless of the value\.

### Serving Compressed Files When Your Origin Server Is Running IIS<a name="serving-compressed-files-iis"></a>

By default, IIS does not serve compressed content for requests that come through proxy servers such as CloudFront\. If you're using IIS and if you configured IIS to compress content by using the `httpCompressionelement`, change the value of the `noCompressionForProxies` attribute to `false` so IIS will return compressed content to CloudFront\.

In addition, if you have compressed objects that are requested less frequently than every few seconds, you might have to change the values of `frequentHitThreshold` and `frequentHitTimePeriod`\.

For more information, refer to the IIS documentation on the Microsoft website\.

### Serving Compressed Files When Your Origin Server Is Running NGINX<a name="serving-compressed-files-nginx"></a>

When CloudFront forwards a request to the origin server, it includes a `Via` header\. This causes NGINX to interpret the request as proxied and, by default, NGINX disables compression for proxied requests\. If your version of NGINX includes the `gzip_proxied` setting, change the value to `any` so that NGINX will return compressed content to CloudFront\. For more information, see the NGINX documentation for the module `ngx_http_gzip_module`\. 