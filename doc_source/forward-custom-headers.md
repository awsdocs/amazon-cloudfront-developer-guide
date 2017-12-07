# Forwarding Custom Headers to Your Origin \(Web Distributions Only\)<a name="forward-custom-headers"></a>

You can configure CloudFront to include custom headers whenever it forwards a request to your origin\. You can specify the names and values of custom headers for each origin, both for custom origins and for Amazon S3 buckets\. Custom headers have a variety of uses, such as the following: 

+ You can identify the requests that are forwarded to your custom origin by CloudFront\. This is useful if you want to know whether users are bypassing CloudFront or if you're using more than one CDN and you want information about which requests are coming from each CDN\. \(If you're using an Amazon S3 origin and you enable [Amazon S3 server access logging](http://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html), the logs don't include header information\.\)

+ If you've configured more than one CloudFront distribution to use the same origin, you can specify different custom headers for the origins in each distribution and use the logs for your web server to distinguish between the requests that CloudFront forwards for each distribution\.

+ If some of your users use viewers that don't support cross\-origin resource sharing \(CORS\), you can configure CloudFront to forward the `Origin` header to your origin\. That will cause your origin to return the `Access-Control-Allow-Origin` header for every request\.

+ You can use custom headers together and, optionally, signed URLs or signed cookies, to control access to content on a custom origin\. If you configure your custom origin to respond to requests only if they include a custom header, you can prevent users from bypassing CloudFront and submitting requests directly to your origin\. 


+ [Configuring CloudFront to Forward Custom Headers to Your Origin](#forward-custom-headers-configure)
+ [Custom Headers that CloudFront Can't Forward to Your Origin](#forward-custom-headers-blacklist)
+ [Using Custom Headers for Cross\-Origin Resource Sharing \(CORS\)](#forward-custom-headers-cors)
+ [Using Custom Headers to Restrict Access to Your Content on a Custom Origin](#forward-custom-headers-restrict-access)

## Configuring CloudFront to Forward Custom Headers to Your Origin<a name="forward-custom-headers-configure"></a>

To configure a web distribution to forward custom headers to your origin, you update the configuration of the applicable origins by using one of the following methods: 

**CloudFront console**  
When you create or update a distribution, specify header names and values in the **Origin Custom Headers** settings\. For more information, see [Creating or Updating a Web Distribution Using the CloudFront Console](distribution-web-creating-console.md)\.

**CloudFront API**  
For each origin that you want to forward custom headers to, add header names and values to the `CustomHeaders` section of the `DistributionConfig` complex type\. For more information, see [POST Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateDistribution.html) \(to create a new distribution\) or [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) \(to update an existing distribution\)\. 

If the header names and values that you specify are not already present in the viewer request, CloudFront adds them\. If a header is present, CloudFront overwrites the header value before forwarding the request to the origin\.

For the current limits related to forwarding custom headers to the origin, see [Limits](cloudfront-limits.md)\.

## Custom Headers that CloudFront Can't Forward to Your Origin<a name="forward-custom-headers-blacklist"></a>

You can't configure CloudFront to forward the following custom headers to your origin\. 


****  

|  |  | 
| --- |--- |
| Accept\-Encoding | Proxy\-Authenticate | 
| Cache\-Control | Proxy\-Authorization | 
| Connection | Proxy\-Connection | 
| Content\-Length | Range | 
| Cookie | Request\-Range | 
| Host | TE | 
| If\-Match | Trailer | 
| If\-Modified\-Since | Transfer\-Encoding | 
| If\-None\-Match | Upgrade | 
| If\-Range | Via | 
| If\-Unmodified\-Since | Headers that begin with X\-Amz\-\* | 
| Max\-Forwards | Headers that begin with X\-Edge\-\* | 
| Pragma | X\-Real\-Ip | 

## Using Custom Headers for Cross\-Origin Resource Sharing \(CORS\)<a name="forward-custom-headers-cors"></a>

You can configure CloudFront to always forward the applicable headers to your origin to accommodate viewers that don't automatically include those headers in requests\. You also need to configure CloudFront to respect CORS settings\. For more information, see [Configuring CloudFront to Respect Cross\-Origin Resource Sharing \(CORS\) Settings](header-caching.md#header-caching-web-cors)\.

## Using Custom Headers to Restrict Access to Your Content on a Custom Origin<a name="forward-custom-headers-restrict-access"></a>

If you're using a custom origin, you can use custom headers to prevent users from bypassing CloudFront and requesting content directly from your origin\. You can also optionally restrict access to your content by requiring that your users access your objects by using either signed URLs or signed cookies\. For more information about private content, see [Serving Private Content through CloudFront](PrivateContent.md)\.

To require that users access your content through CloudFront, change the following settings in your CloudFront distributions:

**Origin Custom Headers**  
Configure CloudFront to forward custom headers to your origin\. See [Configuring CloudFront to Forward Custom Headers to Your Origin](#forward-custom-headers-configure)\.

**Viewer Protocol Policy**  
Configure your distribution to require viewers to use HTTPS to access CloudFront\. See [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\.

**Origin Protocol Policy**  
Configure your distribution to require CloudFront to use the same protocol as viewers to forward requests to the origin\. See [Origin Protocol Policy \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](distribution-web-values-specify.md#DownloadDistValuesOriginProtocolPolicy)\.

The combination of **Viewer Protocol Policy** and **Origin Protocol Policy** ensure that your custom headers are encrypted between the viewer and your origin\. However, we recommend that you periodically perform the following tasks to rotate the custom headers that CloudFront forwards to your origin:

1. Update your CloudFront distribution to begin forwarding a new header to your custom origin\.

1. Update your application to accept the new header as confirmation that the request is coming from CloudFront\.

1. When viewer requests no longer include the header that you're replacing, update your application to no longer accept the old header as confirmation that the request is coming from CloudFront\.