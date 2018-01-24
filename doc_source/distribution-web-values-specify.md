# Values That You Specify When You Create or Update a Web Distribution<a name="distribution-web-values-specify"></a>

When you create a new web distribution or update an existing distribution, you specify the following values\. For information about creating or updating a web distribution using the CloudFront console, see the applicable topic:

+ [Working with Web Distributions](distribution-web.md)

+ [Listing, Viewing, and Updating CloudFront Distributions](HowToUpdateDistribution.md)

**[Delivery Method](#DownloadDistValuesMethod)**

**[Origin Settings](#DownloadDistValuesOrigin)**

+ [Origin Domain Name](#DownloadDistValuesDomainName)

+ [Origin Path](#DownloadDistValuesOriginPath)

+ [Origin ID](#DownloadDistValuesId)

+ [Restrict Bucket Access \(Amazon S3 Only\)](#DownloadDistValuesOAIRestrictBucketAccess)

+ [Origin Access Identity \(Amazon S3 Only\)](#DownloadDistValuesOAI)

+ [Comment for New Identity \(Amazon S3 Only\)](#DownloadDistValuesOAIComment)

+ [Your Identities \(Amazon S3 Only\)](#DownloadDistValuesOAIYourIdentities)

+ [Grant Read Permissions on Bucket \(Amazon S3 Only\)](#DownloadDistValuesOAIGrantReadPermissions)

+ [Origin SSL Protocols \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](#DownloadDistValuesOriginSSLProtocols)

+ [Origin Protocol Policy \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](#DownloadDistValuesOriginProtocolPolicy)

+ [Origin Response Timeout \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](#DownloadDistValuesOriginResponseTimeout)

+ [Origin Keep\-alive Timeout \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](#DownloadDistValuesOriginKeepaliveTimeout)

+ [HTTP Port \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](#DownloadDistValuesHTTPPort)

+ [HTTPS Port \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](#DownloadDistValuesHTTPSPort)

+ [Origin Custom Headers](#DownloadDistValuesOriginCustomHeaders)

**[Cache Behavior Settings](#DownloadDistValuesCacheBehavior)**

+ [Path Pattern](#DownloadDistValuesPathPattern)

+ [Origin \(Existing Distributions Only\)](#DownloadDistValuesTargetOriginId)

+ [Viewer Protocol Policy](#DownloadDistValuesViewerProtocolPolicy)

+ [Allowed HTTP Methods](#DownloadDistValuesAllowedHTTPMethods)

+ [Field Level Encryption](#DownloadDistValuesFieldLevelEncryption)

+ [Cached HTTP Methods](#DownloadDistValuesCachedHTTPMethods)

+ [Cache Based on Selected Request Headers](#DownloadDistValuesForwardHeaders)

+ [Whitelist Headers](#DownloadDistValuesWhitelistHeaders)

+ [Object Caching](#DownloadDistValuesObjectCaching)

+ [Minimum TTL](#DownloadDistValuesMinTTL)

+ [Maximum TTL](#DownloadDistValuesMaxTTL)

+ [Default TTL](#DownloadDistValuesDefaultTTL)

+ [Forward Cookies \(Amazon EC2 and Other Custom Origins Only\)](#DownloadDistValuesForwardCookies)

+ [Whitelist Cookies \(Amazon EC2 and Other Custom Origins Only\)](#DownloadDistValuesWhitelistCookies)

+ [Query String Forwarding and Caching](#DownloadDistValuesQueryString)

+ [Query String Whitelist](#DownloadDistValuesQueryStringWhiteList)

+ [Smooth Streaming](#DownloadDistValuesSmoothStreaming)

+ [Restrict Viewer Access \(Use Signed URLs\)](#DownloadDistValuesRestrictViewerAccess)

+ [Trusted Signers](#DownloadDistValuesTrustedSigners)

+ [AWS Account Numbers](#DownloadDistValuesAWSAccountNumbers)

+ [Compress Objects Automatically](#DownloadDistValuesCompressObjectsAutomatically)

+ [Event Type](#DownloadDistValuesEventType)

+ [Lambda Function ARN](#DownloadDistValuesLambdaFunctionARN)

**[Distribution Details](#DownloadDistValuesGeneral)**

+ [Price Class](#DownloadDistValuesPriceClass)

+ [AWS WAF Web ACL](#DownloadDistValuesWAFWebACL)

+ [Alternate Domain Names \(CNAMEs\)](#DownloadDistValuesCNAME)

+ [SSL Certificate](#DownloadDistValuesSSLCertificate)

+ [Clients Supported](#DownloadDistValuesClientsSupported)

+ [Security Policy](#DownloadDistValues-security-policy)

+ Minimum SSL Security Protocol – See [Security Policy](#DownloadDistValues-security-policy)

+ [Supported HTTP Versions](#DownloadDistValuesSupportedHTTPVersions)

+ [Default Root Object](#DownloadDistValuesDefaultRootObject)

+ [Logging](#DownloadDistValuesLoggingOnOff)

+ [Bucket for Logs](#DownloadDistValuesLogBucket)

+ [Log Prefix](#DownloadDistValuesLogPrefix)

+ [Cookie Logging](#DownloadDistValuesCookieLogging)

+ [Enable IPv6](#DownloadDistValuesEnableIPv6)

+ [Comment](#DownloadDistValuesComment)

+ [Distribution State](#DownloadDistValuesEnabled)

**[Custom Error Pages and Error Caching](#DownloadDistValuesErrorPages)**

+ [Error Code](#DownloadDistValuesErrorCode)

+ [Response Page Path](#DownloadDistValuesResponsePagePath)

+ [Response Code](#DownloadDistValuesResponseCode)

+ [Error Caching Minimum TTL](#DownloadDistValuesErrorCachingMinTTL)

**[Restrictions](#DownloadDistValuesRestrictions)**

+ [Enable Geo Restriction](#DownloadDistValuesEnableGeoRestriction)

+ [Restriction Type](#DownloadDistValuesRestrictionType)

+ [Countries](#DownloadDistValuesCountries)

## Delivery Method<a name="DownloadDistValuesMethod"></a>

You specify the delivery method when you create a distribution\. For a web distribution, this value is always **Web**\. You can't change the delivery method for an existing distribution\. 

## Origin Settings<a name="DownloadDistValuesOrigin"></a>

When you create or update a distribution, you provide information about one or more locations—known as origins—where you store the original versions of your web content\. CloudFront gets your web content from your origins and serves it to viewers via a world\-wide network of edge servers\. Each origin is either an Amazon S3 bucket or an HTTP server, for example, a web server\. 

For the current limit on the number of origins that you can create for a distribution or to request a higher limit, see [General Limits on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.

If you want to delete an origin, you must first edit or delete the cache behaviors that are associated with that origin\. 

**Important**  
If you delete an origin, confirm that files that were previously served by that origin are available in another origin and that your cache behaviors are now routing requests for those files to the new origin\.

When you create or update a distribution, you specify the following values for each origin\.

### Origin Domain Name<a name="DownloadDistValuesDomainName"></a>

The DNS domain name of the Amazon S3 bucket or HTTP server from which you want CloudFront to get objects for this origin, for example:

+ **Amazon S3 bucket** – `myawsbucket.s3.amazonaws.com`

+ **Amazon EC2 instance** – `ec2-203-0-113-25.compute-1.amazonaws.com`

+ **Elastic Load Balancing load balancer** – `my-load-balancer-1234567890.us-west-2.elb.amazonaws.com`

If your origin is an HTTP server, type the domain name of the resource\. The files must be publicly readable\.

If your origin is an Amazon S3 bucket, in the CloudFront console, choose in the **Origin Domain Name** field, and a list enumerates the Amazon S3 buckets that are associated with the current AWS account\. Note the following:

+ If the bucket is configured as a website, enter the Amazon S3 static website hosting endpoint for your bucket; do not select the bucket name from the list in the **Origin Domain Name** field\. The static website hosting endpoint appears in the Amazon S3 console, on the **Properties** page under **Static Website Hosting**\.

+ If you configured Amazon S3 Transfer Acceleration for your bucket, do not specify the `s3-accelerate` endpoint for **Origin Domain Name**\.

+ If you're using a bucket from a different AWS account and if the bucket is not configured as a website, type the name in the following format:

  `bucket-name.s3.amazonaws.com` 

  If your bucket is in the US Standard region and you want Amazon S3 to route requests to a facility in Northern Virginia, use the following format:

  `bucket-name.s3-external-1.amazonaws.com` 

  If your bucket is in the EU \(Frankfurt\) region, you can also use the following format:

  `bucket-name.s3.eu-central-1.amazonaws.com` 

+ The files must be publicly readable unless you secure your content in Amazon S3 by using a CloudFront origin access identity\. For more information, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\.

**Important**  
If the origin is an Amazon S3 bucket, the bucket name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

When you change the value of **Origin Domain Name** for an origin, CloudFront immediately begins replicating the change to CloudFront edge locations\. Until the distribution configuration is updated in a given edge location, CloudFront will continue to forward requests to the previous HTTP server or Amazon S3 bucket\. As soon as the distribution configuration is updated in that edge location, CloudFront begins to forward requests to the new HTTP server or Amazon S3 bucket\.

Changing the origin does not require CloudFront to repopulate edge caches with objects from the new origin\. As long as the viewer requests in your application have not changed, CloudFront will continue to serve objects that are already in an edge cache until the TTL on each object expires or until seldom\-requested objects are evicted\. 

### Origin Path<a name="DownloadDistValuesOriginPath"></a>

If you want CloudFront to request your content from a directory in your Amazon S3 bucket or your custom origin, enter the directory path, beginning with a /\. CloudFront appends the directory path to the value of **Origin Domain Name**, for example, **cf\-origin\.example\.com/production/images**\. Do not add a / at the end of the path\.

For example, suppose you've specified the following values for your distribution:

+ **Origin Domain Name** – An Amazon S3 bucket named **myawsbucket**

+ **Origin Path** – **/production**

+ **Alternate Domain Names \(CNAMEs\)** – **example\.com**

When a user enters **example\.com/index\.html** in a browser, CloudFront sends a request to Amazon S3 for **myawsbucket/production/index\.html**\.

When a user enters **example\.com/acme/index\.html** in a browser, CloudFront sends a request to Amazon S3 for **myawsbucket/production/acme/index\.html**\.

### Origin ID<a name="DownloadDistValuesId"></a>

A string that uniquely distinguishes this origin from other origins in this distribution\. If you create cache behaviors in addition to the default cache behavior, you use the origin ID that you specify here to identify the origin to which you want CloudFront to route a request when the request matches the path pattern for that cache behavior\. For more information, see [Cache Behavior Settings](#DownloadDistValuesCacheBehavior)\.

### Restrict Bucket Access \(Amazon S3 Only\)<a name="DownloadDistValuesOAIRestrictBucketAccess"></a>

Choose **Yes** if you want to require users to access objects in an Amazon S3 bucket by using only CloudFront URLs, not by using Amazon S3 URLs\. Then specify the applicable values\.

Choose **No** if you want users to be able to access objects using either CloudFront URLs or Amazon S3 URLs\.

For more information, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\.

For information about how to require users to access objects on a custom origin by using only CloudFront URLs, see [Using Custom Headers to Restrict Access to Your Content on a Custom Origin](forward-custom-headers.md#forward-custom-headers-restrict-access)\.

### Origin Access Identity \(Amazon S3 Only\)<a name="DownloadDistValuesOAI"></a>

If you chose **Yes** for **Restrict Bucket Access**, choose whether to create a new origin access identity or use an existing one that is associated with your AWS account\. If you already have an origin access identity, we recommend that you reuse it to simplify maintenance\. For more information about origin access identities, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\.

### Comment for New Identity \(Amazon S3 Only\)<a name="DownloadDistValuesOAIComment"></a>

If you chose **Create a New Identity** for **Origin Access Identity**, enter a comment that identifies the new origin access identity\. CloudFront will create the origin access identity when you create this distribution\.

### Your Identities \(Amazon S3 Only\)<a name="DownloadDistValuesOAIYourIdentities"></a>

If you chose **Use an Existing Identity** for **Origin Access Identity**, choose the origin access identity that you want to use\. You cannot use an origin access identity that is associated with another AWS account\.

### Grant Read Permissions on Bucket \(Amazon S3 Only\)<a name="DownloadDistValuesOAIGrantReadPermissions"></a>

If you want CloudFront to automatically grant the origin access identity the permission to read objects in your Amazon S3 bucket, choose **Yes, Update Bucket Policy**\. 

**Important**  
If you choose **Yes, Update Bucket Policy**, CloudFront updates the bucket policy to grant the specified origin access identity the permission to read objects in your bucket\. However, CloudFront does not remove existing permissions in the bucket policy or permissions on individual objects\. If users currently have permission to access the objects in your bucket using Amazon S3 URLs, they will still have that permission after CloudFront updates your bucket policy\. To view or change the existing bucket policy and the existing permissions on the objects in your bucket, use a method provided by Amazon S3\. For more information, see [Granting the Origin Access Identity Permission to Read Objects in Your Amazon S3 Bucket](private-content-restricting-access-to-s3.md#private-content-granting-permissions-to-oai)\.

If you want to update permissions manually, for example, if you want to update ACLs on your objects instead of updating bucket permissions, choose **No, I will Update Permissions**\.

### Origin SSL Protocols \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)<a name="DownloadDistValuesOriginSSLProtocols"></a>

Choose the SSL protocols that CloudFront can use when establishing an HTTPS connection with your origin\. The SSLv3 protocol is less secure, so we recommend that you choose SSLv3 only if your origin doesn't support TLSv1 or later\. 

**Note**  
If you select SSLv3, CloudFront does not attempt to make a connection to the Origin using TLS\.

If the origin is an Amazon S3 bucket, CloudFront always uses TLSv1\.2\.

### Origin Protocol Policy \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)<a name="DownloadDistValuesOriginProtocolPolicy"></a>

The protocol policy that you want CloudFront to use when fetching objects from your origin server\. 

**Important**  
If your Amazon S3 bucket is configured as a website endpoint, you must specify HTTP Only\. Amazon S3 doesn't support HTTPS connections in that configuration\.

Choose the applicable value:

+ **HTTP Only:** CloudFront uses only HTTP to access the origin\.

+ **HTTPS Only:** CloudFront uses only HTTPS to access the origin\.

+ **Match Viewer:** CloudFront communicates with your origin using HTTP or HTTPS, depending on the protocol of the viewer request\. CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols\.
**Important**  
For HTTPS viewer requests that CloudFront forwards to this origin, one of the domain names in the SSL certificate on your origin server must match the domain name that you specify for **Origin Domain Name**\. Otherwise, CloudFront responds to the viewer requests with an HTTP status code 502 \(Bad Gateway\) instead of the requested object\. For more information, see [Requirements for Using SSL/TLS Certificates with CloudFront](cnames-and-https-requirements.md)\.

### Origin Response Timeout \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)<a name="DownloadDistValuesOriginResponseTimeout"></a>

 The origin response timeout , also known as the origin read timeout or origin request timeout, applies to both of the following values:

+ The amount of time, in seconds, that CloudFront waits for a response after forwarding a request to a custom origin

+ The amount of time, in seconds, that CloudFront waits after receiving a packet of a response from the origin and before receiving the next packet

The default timeout is 30 seconds\. You can change the value to between 4 and 60 seconds\. If you need a timeout value outside that range, [request a change to the limit](https://console.aws.amazon.com/support/home?region=us-east-1#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.

CloudFront behavior depends on the HTTP method in the viewer request:

+ `GET` and `HEAD` requests – If the origin doesn't respond before the read timeout elapses or if the origin stops responding for the configured timeout, CloudFront drops the connection and tries two more times to contact the origin\. After the third try, if the origin doesn't respond before the read timeout elapses, CloudFront doesn't try again until it receives another request for content on the same origin\.

+ `DELETE`, `OPTIONS`, `PATCH`, `PUT`, and `POST` requests – If the origin doesn't respond before the read timeout elapses, CloudFront drops the connection and doesn't try again to contact the origin\. The client can resubmit the request if necessary\.

### Origin Keep\-alive Timeout \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)<a name="DownloadDistValuesOriginKeepaliveTimeout"></a>

The amount of time, in seconds, that CloudFront tries to maintain a connection to your custom origin after it gets the last packet of a response\. Maintaining a persistent connection saves the time that is required to re\-establish the TCP connection and perform another TLS handshake for subsequent requests\. Increasing the keep\-alive timeout helps improve the request\-per\-connection metric for distributions\.

**Note**  
For the **Origin Keep\-alive Timeout** value to have an effect, your origin must be configured to allow persistent connections\.

The default timeout is 5 seconds\. You can change the value to between 1 and 60 seconds\. If you need a keep\-alive timeout longer than 60 seconds, [request a change to the limit](https://console.aws.amazon.com/support/home?region=us-east-1#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)\.

### HTTP Port \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)<a name="DownloadDistValuesHTTPPort"></a>

Optional\. The HTTP port that the custom origin listens on\. Valid values include ports 80, 443, and 1024 to 65535\. The default value is port 80\.

### HTTPS Port \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)<a name="DownloadDistValuesHTTPSPort"></a>

Optional\. The HTTPS port that the custom origin listens on\. Valid values include ports 80, 443, and 1024 to 65535\. The default value is port 443\.

### Origin Custom Headers<a name="DownloadDistValuesOriginCustomHeaders"></a>

If you want CloudFront to include custom headers whenever it forwards a request to your origin, specify the following values:

**Header Name**  
The name of a header that you want CloudFront to forward to your origin\.

**Value**  
The value for the header that you specified in the **Custom Header** field\.

For more information, see [Forwarding Custom Headers to Your Origin \(Web Distributions Only\)](forward-custom-headers.md)\. 

For the current limit on the maximum number of custom headers that you can forward to the origin, the maximum length of a custom header name and value, and the total length of all header names and values, see [Limits](cloudfront-limits.md)\.

## Cache Behavior Settings<a name="DownloadDistValuesCacheBehavior"></a>

A cache behavior lets you configure a variety of CloudFront functionality for a given URL path pattern for files on your website\. For example, one cache behavior might apply to all `.jpg` files in the `images` directory on a web server that you're using as an origin server for CloudFront\. The functionality you can configure for each cache behavior includes:

+ The path pattern\.

+ If you have configured multiple origins for your CloudFront distribution, which origin you want CloudFront to forward your requests to\.

+ Whether to forward query strings to your origin\.

+ Whether accessing the specified files requires signed URLs\.

+ Whether to require users to use HTTPS to access those files\.

+ The minimum amount of time that those files stay in the CloudFront cache regardless of the value of any `Cache-Control` headers that your origin adds to the files\.

When you create a new distribution, you specify settings for the default cache behavior, which automatically forwards all requests to the origin that you specify when you create the distribution\. After you create a distribution, you can create additional cache behaviors that define how CloudFront responds when it receives a request for objects that match a path pattern, for example, `*.jpg`\. If you create additional cache behaviors, the default cache behavior is always the last to be processed\. Other cache behaviors are processed in the order in which they're listed in the CloudFront console or, if you're using the CloudFront API, the order in which they're listed in the `DistributionConfig` element for the distribution\. For more information, see [Path Pattern](#DownloadDistValuesPathPattern)\.

When you create a cache behavior, you specify the one origin from which you want CloudFront to get objects\. As a result, if you want CloudFront to distribute objects from all of your origins, you must have at least as many cache behaviors \(including the default cache behavior\) as you have origins\. For example, if you have two origins and only the default cache behavior, the default cache behavior will cause CloudFront to get objects from one of the origins, but the other origin will never be used\.

For the current limit on the number of cache behaviors that you can add to a distribution or to request a higher limit, see [General Limits on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.

### Path Pattern<a name="DownloadDistValuesPathPattern"></a>

A path pattern \(for example, `images/*.jpg`\) specifies which requests you want this cache behavior to apply to\. When CloudFront receives an end\-user request, the requested path is compared with path patterns in the order in which cache behaviors are listed in the distribution\. The first match determines which cache behavior is applied to that request\. For example, suppose you have three cache behaviors with the following three path patterns, in this order:

+ `images/*.jpg`

+ `images/*`

+ `*.gif`

**Note**  
You can optionally include a slash \(/\) at the beginning of the path pattern, for example, `/images/*.jpg`\. CloudFront behavior is the same with or without the leading /\.

A request for the file `images/sample.gif` doesn't satisfy the first path pattern, so the associated cache behaviors are not be applied to the request\. The file does satisfy the second path pattern, so the cache behaviors associated with the second path pattern are applied even though the request also matches the third path pattern\.

**Note**  
When you create a new distribution, the value of **Path Pattern** for the default cache behavior is set to **\*** \(all files\) and cannot be changed\. This value causes CloudFront to forward all requests for your objects to the origin that you specified in the [Origin Domain Name](#DownloadDistValuesDomainName) field\. If the request for an object does not match the path pattern for any of the other cache behaviors, CloudFront applies the behavior that you specify in the default cache behavior\.

**Important**  
Define path patterns and their sequence carefully or you may give users undesired access to your content\. For example, suppose a request matches the path pattern for two cache behaviors\. The first cache behavior does not require signed URLs and the second cache behavior does require signed URLs\. Users will be able to access the objects without using a signed URL because CloudFront processes the cache behavior associated with the first match\. 

The path you specify applies to requests for all files in the specified directory and in subdirectories below the specified directory\. CloudFront does not consider query strings or cookies when evaluating the path pattern\. For example, if an `images` directory contains `product1` and `product2` subdirectories, the path pattern `images/*.jpg` applies to requests for any \.jpg file in the `images`, `images/product1`, and `images/product2` directories\. If you want to apply a different cache behavior to the files in the `images/product1` directory than the files in the `images` and `images/product2` directories, create a separate cache behavior for `images/product1` and move that cache behavior to a position above \(before\) the cache behavior for the `images` directory\.

You can use the following wildcard characters in your path pattern:

+ `*` matches 0 or more characters\.

+ `?` matches exactly 1 character\.

The following examples show how the wildcard characters work:


****  

| Path pattern | Files that match the path pattern | 
| --- | --- | 
| `*.jpg` | All \.jpg files | 
| `images/*.jpg` | All \.jpg files in the `images` directory and in subdirectories under the `images` directory | 
| `a*.jpg` | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html)  | 
| `a??.jpg` | All \.jpg files for which the filename begins with `a` and is followed by exactly two other characters, for example, `ant.jpg` and `abe.jpg` | 
| `*.doc*` | All files for which the filename extension begins with `.doc`, for example, `.doc`, `.docx`, and `.docm` files\. You can't use the path pattern `*.doc?` in this case, because that path pattern wouldn't apply to requests for `.doc` files; the `?` wildcard character replaces exactly one character\. | 

The maximum length of a path pattern is 255 characters\. The value can contain any of the following characters:

+ A\-Z, a\-z

  Path patterns are case sensitive, so the path pattern `*.jpg` doesn't apply to the file `LOGO.JPG`\.

+ 0\-9

+ \_ \- \. \* $ / \~ " ' @ : \+

+ &, passed and returned as `&amp;`

### Origin \(Existing Distributions Only\)<a name="DownloadDistValuesTargetOriginId"></a>

Enter the value of **Origin ID** for an existing origin\. This identifies the origin that you want CloudFront to route requests to when a request \(such as http://example\.com/logo\.jpg\) matches the path pattern for a cache behavior \(such as \*\.jpg\) or for the default cache behavior \(\*\)\. 

### Viewer Protocol Policy<a name="DownloadDistValuesViewerProtocolPolicy"></a>

Choose the protocol policy that you want viewers to use to access your content in CloudFront edge locations:

+ **HTTP and HTTPS**: Viewers can use both protocols\. 

+ **Redirect HTTP to HTTPS**: Viewers can use both protocols, but HTTP requests are automatically redirected to HTTPS requests\.

+ **HTTPS Only**: Viewers can only access your content if they're using HTTPS\.

For more information, see [Requiring HTTPS for Communication Between Viewers and CloudFront](using-https-viewers-to-cloudfront.md)\.

### Field Level Encryption<a name="DownloadDistValuesFieldLevelEncryption"></a>

If you want to enforce field\-level encryption on specific data fields, in the dropdown lits, choose a field\-level encryption configuration\.

For more information, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\.

### Allowed HTTP Methods<a name="DownloadDistValuesAllowedHTTPMethods"></a>

Specify the HTTP methods that you want CloudFront to process and forward to your origin:

+ **GET, HEAD:** You can use CloudFront only to get objects from your origin or to get object headers\.

+ **GET, HEAD, OPTIONS:** You can use CloudFront only to get objects from your origin, get object headers, or retrieve a list of the options that your origin server supports\.

+ **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE:** You can use CloudFront to get, add, update, and delete objects, and to get object headers\. In addition, you can perform other POST operations such as submitting data from a web form\. 
**Note**  
CloudFront caches responses to `GET` and `HEAD` requests and, optionally, `OPTIONS` requests\. CloudFront does not cache responses to requests that use the other methods\.

If you use an Amazon S3 bucket as the origin for your distribution and if you use CloudFront origin access identities, `POST` requests aren't supported in some Amazon S3 regions and `PUT` requests in those regions require an additional header\. For more information, see [Using an Origin Access Identity in Amazon S3 Regions that Support Only Signature Version 4 Authentication](private-content-restricting-access-to-s3.md#private-content-origin-access-identity-signature-version-4)\.

**Important**  
If you choose **GET, HEAD, OPTIONS** or **GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE**, you might need to restrict access to your Amazon S3 bucket or to your custom origin to prevent users from performing operations that you don't want them to perform\. The following examples explain how to restrict access:  
**If you're using Amazon S3 as an origin for your distribution:** Create a CloudFront origin access identity to restrict access to your Amazon S3 content, and grant the origin access identity the applicable permissions\. For example, if you configure CloudFront to accept and forward these methods *only* because you want to use `PUT`, you must still configure Amazon S3 bucket policies or ACLs to handle `DELETE` requests appropriately\. For more information, see [Using an Origin Access Identity to Restrict Access to Your Amazon S3 Content](private-content-restricting-access-to-s3.md)\.
**If you're using a custom origin:** Configure your origin server to handle all methods\. For example, if you configure CloudFront to accept and forward these methods *only* because you want to use `POST`, you must still configure your origin server to handle `DELETE` requests appropriately\. 

### Cached HTTP Methods<a name="DownloadDistValuesCachedHTTPMethods"></a>

Specify whether you want CloudFront to cache the response from your origin when a viewer submits an `OPTIONS` request\. CloudFront always caches the response to `GET` and `HEAD` requests\.

### Cache Based on Selected Request Headers<a name="DownloadDistValuesForwardHeaders"></a>

Specify whether you want CloudFront to cache objects based on the values of specified headers:

+ **None \(Improves Caching\)** – CloudFront doesn't cache your objects based on header values\.

+ **Whitelist** – CloudFront caches your objects based only on the values of the specified headers\. Use **Whitelist Headers** to choose the headers that you want CloudFront to base caching on\.

  If you're using an Amazon S3 bucket as your origin, you can configure CloudFront to cache based only on the following headers: `Access-Control-Request-Headers`, `Access-Control-Request-Method`, and `Origin`\. 

+ **All** – CloudFront doesn't cache the objects that are associated with this cache behavior\. Instead, CloudFront sends every request to the origin\.

Regardless of which option you choose, CloudFront forwards headers to your origin based on whether the origin is an S3 bucket or a custom origin\. See the following documentation:

+ **S3 bucket** – See [HTTP Request Headers That CloudFront Removes or Updates](RequestAndResponseBehaviorS3Origin.md#request-s3-removed-headers)

+ **Custom origin** – See [HTTP Request Headers and CloudFront Behavior](RequestAndResponseBehaviorCustomOrigin.md#request-custom-headers-behavior)

For more information, see [Configuring CloudFront to Cache Objects Based on Request Headers](header-caching.md)\.

### Whitelist Headers<a name="DownloadDistValuesWhitelistHeaders"></a>

Specify the headers that you want CloudFront to consider when caching your objects\. Select headers from the list of available headers and choose **Add**\. To forward a custom header, enter the name of the header in the field, and choose **Add Custom**\.

For the current limit on the number of headers that you can whitelist for each cache behavior or to request a higher limit, see [Limits on Custom Headers \(Web Distributions Only\)](cloudfront-limits.md#limits-custom-headers)\.

### Object Caching<a name="DownloadDistValuesObjectCaching"></a>

If your origin server is adding a `Cache-Control` header to your objects to control how long the objects stay in the CloudFront cache and if you don't want to change the `Cache-Control` value, choose **Use Origin Cache Headers**\.

To specify a minimum and maximum time that your objects stay in the CloudFront cache regardless of `Cache-Control` headers, and a default time that your objects stay in the CloudFront cache when the `Cache-Control` header is missing from an object, choose **Customize**\. Then, in the **Minimum TTL**, **Default TTL**, and **Maximum TTL** fields, specify the applicable value\.

For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

### Minimum TTL<a name="DownloadDistValuesMinTTL"></a>

Specify the minimum amount of time, in seconds, that you want objects to stay in CloudFront caches before CloudFront forwards another request to your origin to determine whether the object has been updated\. The default value for **Minimum TTL** is 0 seconds\.

**Important**  
If you configure CloudFront to forward all headers to your origin for a cache behavior, CloudFront never caches the associated objects\. Instead, CloudFront forwards all requests for those objects to the origin\. In that configuration, the value of **Minimum TTL** must be 0\.

To specify a value for **Minimum TTL**, you must choose the **Customize** option for the **Object Caching** setting\.

For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

### Maximum TTL<a name="DownloadDistValuesMaxTTL"></a>

Specify the maximum amount of time, in seconds, that you want objects to stay in CloudFront caches before CloudFront queries your origin to see whether the object has been updated\. The value that you specify for **Maximum TTL** applies only when your origin adds HTTP headers such as `Cache-Control max-age`, `Cache-Control s-maxage`, or `Expires` to objects\. For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

To specify a value for **Maximum TTL**, you must choose the **Customize** option for the **Object Caching** setting\.

The default value for **Maximum TTL** is 31536000 seconds \(one year\)\. If you change the value of **Minimum TTL** or **Default TTL** to more than 31536000 seconds, then the default value of **Maximum TTL** changes to the value of **Default TTL**\. 

### Default TTL<a name="DownloadDistValuesDefaultTTL"></a>

Specify the default amount of time, in seconds, that you want objects to stay in CloudFront caches before CloudFront forwards another request to your origin to determine whether the object has been updated\. The value that you specify for **Default TTL** applies only when your origin does *not* add HTTP headers such as `Cache-Control max-age`, `Cache-Control s-maxage`, or `Expires` to objects\. For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.

To specify a value for **Default TTL**, you must choose the **Customize** option for the **Object Caching** setting\.

The default value for **Default TTL** is 86400 seconds \(one day\)\. If you change the value of **Minimum TTL** to more than 86400 seconds, then the default value of **Default TTL** changes to the value of **Minimum TTL**\. 

### Forward Cookies \(Amazon EC2 and Other Custom Origins Only\)<a name="DownloadDistValuesForwardCookies"></a>

Specify whether you want CloudFront to forward cookies to your origin server and, if so, which ones\. If you choose to forward only selected cookies \(a whitelist of cookies\), enter the cookie names in the **Whitelist Cookies** field\. If you choose **All**, CloudFront forwards all cookies regardless of how many your application uses\.

Amazon S3 doesn't process cookies, and forwarding cookies to the origin reduces cacheability\. For cache behaviors that are forwarding requests to an Amazon S3 origin, choose **None** for **Forward Cookies**\.

For more information about forwarding cookies to the origin, go to [Configuring CloudFront to Cache Objects Based on Cookies](Cookies.md)\.

### Whitelist Cookies \(Amazon EC2 and Other Custom Origins Only\)<a name="DownloadDistValuesWhitelistCookies"></a>

If you chose **Whitelist** in the **Forward Cookies** list, then in the **Whitelist Cookies** field, enter the names of cookies that you want CloudFront to forward to your origin server for this cache behavior\. Enter each cookie name on a new line\.

You can specify the following wildcards to specify cookie names:

+ **\*** matches 0 or more characters in the cookie name

+ **?** matches exactly one character in the cookie name

For example, suppose viewer requests for an object include a cookie named:

`userid_member-number`

where each of your users has a unique value for *member\-number*\. You want CloudFront to cache a separate version of the object for each member\. You could accomplish this by forwarding all cookies to your origin, but viewer requests include some cookies that you don't want CloudFront to cache\. Alternatively, you could specify the following value as a cookie name, which causes CloudFront to forward to the applicable origin all of the cookies that begin with `userid_`:

`userid_*`

For the current limit on the number of cookie names that you can whitelist for each cache behavior or to request a higher limit, see [Limits on Whitelisted Cookies \(Web Distributions Only\)](cloudfront-limits.md#limits-whitelisted-cookies)\.

### Query String Forwarding and Caching<a name="DownloadDistValuesQueryString"></a>

CloudFront can cache different versions of your content based on the values of query string parameters\. Choose the applicable option:

**None \(Improves Caching\)**  
Choose this option if your origin returns the same version of an object regardless of the values of query string parameters\. This increases the likelihood that CloudFront can serve a request from the cache, which improves performance and reduces the load on your origin\.

**Forward all, cache based on whitelist**  
Choose this option if your origin server returns different versions of your objects based on one or more query string parameters\. Then specify the parameters that you want CloudFront to use as a basis for caching in the [Query String Whitelist](#DownloadDistValuesQueryStringWhiteList) field\.

**Forward all, cache based on all**  
Choose this option if your origin server returns different versions of your objects for all query string parameters\. 

For more information about caching based on query string parameters, including how to improve performance, see [Configuring CloudFront to Cache Based on Query String Parameters](QueryStringParameters.md)\.

### Query String Whitelist<a name="DownloadDistValuesQueryStringWhiteList"></a>

If you chose **Forward all, cache based on whitelist** for [Query String Forwarding and Caching](#DownloadDistValuesQueryString), specify the query string parameters that you want CloudFront to use as a basis for caching\.

### Smooth Streaming<a name="DownloadDistValuesSmoothStreaming"></a>

Choose **Yes** if you want to distribute media files in the Microsoft Smooth Streaming format using the origin that is associated with this cache behavior\. Otherwise, choose **No**\.

**Note**  
If you specify **Yes**, you can still distribute other content using this cache behavior if the content matches the value of **Path Pattern**\.

For more information, see [Configuring On\-Demand Smooth Streaming](on-demand-streaming-smooth.md)\.

### Restrict Viewer Access \(Use Signed URLs\)<a name="DownloadDistValuesRestrictViewerAccess"></a>

If you want requests for objects that match the `PathPattern` for this cache behavior to use public URLs, choose **No**\.

If you want requests for objects that match the `PathPattern` for this cache behavior to use signed URLs, choose **Yes**\. Then specify the AWS accounts that you want to use to create signed URLs; these accounts are known as trusted signers\.

For more information about trusted signers, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\. 

### Trusted Signers<a name="DownloadDistValuesTrustedSigners"></a>

Choose which AWS accounts you want to use as trusted signers for this cache behavior:

+ **Self:** Use the account with which you're currently signed into the AWS Management Console as a trusted signer\. If you're currently signed in as an IAM user, the associated AWS account is added as a trusted signer\. 

+ **Specify Accounts:** Enter account numbers for trusted signers in the **AWS Account Numbers** field\.

To create signed URLs, an AWS account must have at least one active CloudFront key pair\.

**Important**  
If you're updating a distribution that you're already using to distribute content, add trusted signers only when you're ready to start generating signed URLs for your objects\. After you add trusted signers to a distribution, users must use signed URLs to access the objects that match the `PathPattern` for this cache behavior\.

### AWS Account Numbers<a name="DownloadDistValuesAWSAccountNumbers"></a>

If you want to create signed URLs using AWS accounts in addition to or instead of the current account, enter one AWS account number per line in this field\. Note the following:

+ The accounts that you specify must have at least one active CloudFront key pair\. For more information, see [Creating CloudFront Key Pairs for Your Trusted Signers](private-content-trusted-signers.md#private-content-creating-cloudfront-key-pairs)\.

+ You can't create CloudFront key pairs for IAM users, so you can't use IAM users as trusted signers\.

+ For information about how to get the AWS account number for an account, see [How Do I Get Security Credentials?](http://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *Amazon Web Services General Reference*\.

+ If you enter the account number for the current account, CloudFront automatically checks the **Self** checkbox and removes the account number from the **AWS Account Numbers** list\.

### Compress Objects Automatically<a name="DownloadDistValuesCompressObjectsAutomatically"></a>

If you want CloudFront to automatically compress files of certain types when viewer requests include `Accept-Encoding: gzip` in the request header, choose **Yes**\. When CloudFront compresses your content, downloads are faster because the files are smaller, and your web pages render faster for your users\. For more information, see [Serving Compressed Files](ServingCompressedFiles.md)\.

### Event Type<a name="DownloadDistValuesEventType"></a>

You can choose to run a Lambda function when one or more of the following CloudFront events occur:

+ When CloudFront receives a request from a viewer \(viewer request\)

+ Before CloudFront forwards a request to the origin \(origin request\)

+ When CloudFront receives a response from the origin \(origin response\)

+ Before CloudFront returns the response to the viewer \(viewer response\)

For more information, see [How You Decide Which CloudFront Event to Use to Trigger a Lambda Function](lambda-how-to-choose-event.md)\.

### Lambda Function ARN<a name="DownloadDistValuesLambdaFunctionARN"></a>

Specify the Amazon Resource Name \(ARN\) of the Lambda function that you want to add a trigger for\. To learn how to get the ARN for a function, see step 1 of the procedure [Adding Triggers for CloudFront Events \(CloudFront Console\)](lambda-create-functions.md#lambda-create-functions-add-triggers-cloudfront-console)\.

## Distribution Details<a name="DownloadDistValuesGeneral"></a>

The following values apply to the entire distribution\. 

### Price Class<a name="DownloadDistValuesPriceClass"></a>

Choose the price class that corresponds with the maximum price that you want to pay for CloudFront service\. By default, CloudFront serves your objects from edge locations in all CloudFront regions\. 

For more information about price classes and about how your choice of price class affects CloudFront performance for your distribution, see [Choosing the Price Class for a CloudFront Distribution](PriceClass.md)\. For information about CloudFront pricing, including how price classes map to CloudFront regions, go to [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

### AWS WAF Web ACL<a name="DownloadDistValuesWAFWebACL"></a>

If you want to use AWS WAF to allow or block requests based on criteria that you specify, choose the web ACL to associate with this distribution\. 

AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to CloudFront, and lets you control access to your content\. Based on conditions that you specify, such as the IP addresses that requests originate from or the values of query strings, CloudFront responds to requests either with the requested content or with an HTTP 403 status code \(Forbidden\)\. You can also configure CloudFront to return a custom error page when a request is blocked\. For more information about AWS WAF, see the [AWS WAF Developer Guide](http://docs.aws.amazon.com/waf/latest/developerguide/)\. 

### Alternate Domain Names \(CNAMEs\)<a name="DownloadDistValuesCNAME"></a>

Optional\. Specify one or more domain names that you want to use for URLs for your objects instead of the domain name that CloudFront assigns when you create your distribution\. For example, if you want the URL for the object:

`/images/image.jpg`

to look like this: 

`http://www.example.com/images/image.jpg`

instead of like this:

`http://d111111abcdef8.cloudfront.net/images/image.jpg`

add a CNAME for `www.example.com`\.

**Important**  
If you add a CNAME for `www.example.com` to your distribution, you also need to create \(or update\) a CNAME record with your DNS service to route queries for `www.example.com` to `d111111abcdef8.cloudfront.net`\. You must have permission to create a CNAME record with the DNS service provider for the domain\. Typically, this means that you own the domain, but you may also be developing an application for the domain owner\.

For the current limit on the number of alternate domain names that you can add to a distribution or to request a higher limit, see [General Limits on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.

For more information about alternate domain names, see [Using Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\. For more information about CloudFront URLs, see [Format of URLs for Objects](LinkFormat.md)\.

### SSL Certificate<a name="DownloadDistValuesSSLCertificate"></a>

If you want viewers to use HTTPS to access your objects, choose the applicable setting\. In addition, if you choose **Custom SSL Certificate**, choose the certificate that you want to use:

+ **Default CloudFront Certificate \(\*\.cloudfront\.net\)** – If you want to use the CloudFront domain name in the URLs for your objects, such as `https://d111111abcdef8.cloudfront.net/image1.jpg`, choose this option\. Also choose this option if you want viewers to use HTTP to access your objects\. 

+ **Custom SSL Certificate** – If you want to use your own domain name in the URLs for your objects, such as `https://example.com/image1.jpg`, choose this option and then choose the applicable certificate\. The list can include certificates provided by AWS Certificate Manager, and certificates that you purchased from a third\-party certificate authority and uploaded to ACM or to the IAM certificate store\. For more information, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

  If you choose this setting, we recommend that you use only an alternate domain name in your object URLs \(https://example\.com/logo\.jpg\)\. If you use your CloudFront distribution domain name \(https://d111111abcdef8\.cloudfront\.net/logo\.jpg\) and the viewer supports SNI, then CloudFront behaves normally\. However, a viewer that does not support SNI exhibits one of the following behaviors, depending on the value of **Clients Supported**:

  + **All Clients**: If the viewer doesn't support SNI, it displays a warning because the CloudFront domain name doesn't match the domain name in your SSL certificate\.

  + **Only Clients that Support Server Name Indication \(SNI\)**: CloudFront drops the connection with the viewer without returning the object\.

### Clients Supported<a name="DownloadDistValuesClientsSupported"></a>

If you specified one or more alternate domain names and you specified an SSL certificate in the IAM certificate store, choose how you want CloudFront to serve HTTPS requests, either a method that works for all clients or one that works for most clients:

+ **All Clients:** Any client can access your content\. However, you must request permission to use this feature, and you incur additional monthly charges\.

+ **Only Clients that Support Server Name Indication \(SNI\):** All modern browsers can access your content because they all support SNI\. However, some browsers still in use don't support SNI\. Users with these browsers must access your content using some other method, for example, by getting your objects directly from the origin\.

For more information, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

### Security Policy<a name="DownloadDistValues-security-policy"></a>

Specify the security policy that you want CloudFront to use for HTTPS connections\. A security policy determines two settings: 

+ The minimum SSL/TLS protocol that CloudFront uses to communicate with viewers

+ The cipher that CloudFront uses to encrypt the content that it returns to viewers

The security policies that are available depend on the values that you specify for **SSL Certificate** and **Custom SSL Client Support**:

+ When **SSL Certificate** is **Default CloudFront Certificate \(\*\.cloudfront\.net\)**, CloudFront automatically sets the value of **Security Policy** to **TLSv1**\.

+ When **SSL Certificate** is **Custom SSL Certificate \(example\.com\)** and **Custom SSL Client Support** is **Only Clients that Support Server Name Indication \(SNI\)**, you must use TLSv1 or later\. We recommend that you choose **TLSv1\.1\_2016** unless your users are using browsers or devices that don't support TLSv1\.1 or later\.

+ When **SSL Certificate** is **Custom SSL Certificate \(example\.com\)** and **Custom SSL Client Support** is **All Clients**, we recommend that you choose **TLSv1**\. In this configuration, the **TLSv1\_2016**, **TLSv1\.1\_2016**, and **TLSv1\.2\_2018** security policies aren't available\.

For information about the relationship between the security policy that you choose and the protocols and ciphers that CloudFront uses to communicate with viewers, see [Supported SSL/TLS Protocols and Ciphers for Communication Between Viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md#secure-connections-supported-ciphers)\.

### Minimum SSL Protocol Version<a name="DownloadDistValuesMinimumSSLProtocolVersion"></a>

See [Security Policy](#DownloadDistValues-security-policy)\.

### Supported HTTP Versions<a name="DownloadDistValuesSupportedHTTPVersions"></a>

Choose the HTTP versions that you want viewers to use to communicate with CloudFront\. Viewers use the latest version that you configure CloudFront to use\. Viewers that don't support HTTP/2 will automatically use an earlier version\.

For viewers and CloudFront to use HTTP/2, viewers must support TLS 1\.2 or later, and must support Server Name Identification \(SNI\)\.

In general, configuring CloudFront to communicate with viewers using HTTP/2 reduces latency\. You can improve performance by optimizing for HTTP/2\. For more information, do an internet search for "http/2 optimization\." 

### Default Root Object<a name="DownloadDistValuesDefaultRootObject"></a>

Optional\. The object that you want CloudFront to request from your origin \(for example, `index.html`\) when a viewer requests the root URL of your distribution \(`http://www.example.com/`\) instead of an object in your distribution \(`http://www.example.com/product-description.html`\)\. Specifying a default root object avoids exposing the contents of your distribution\. 

The maximum length of the name is 255 characters\. The name can contain any of the following characters:

+ A\-Z, a\-z

+ 0\-9

+ \_ \- \. \* $ / \~ " '

+ &, passed and returned as `&amp;`

When you specify the default root object, enter only the object name, for example, `index.html`\. Do not add a `/` before the object name\.

For more information, see [Specifying a Default Root Object \(Web Distributions Only\)](DefaultRootObject.md)\.

### Logging<a name="DownloadDistValuesLoggingOnOff"></a>

Whether you want CloudFront to log information about each request for an object and store the log files in an Amazon S3 bucket\. You can enable or disable logging at any time\. There is no extra charge if you enable logging, but you accrue the usual Amazon S3 charges for storing and accessing the files in an Amazon S3 bucket\. You can delete the logs at any time\. For more information about CloudFront access logs, see [Access Logs](AccessLogs.md)\.

### Bucket for Logs<a name="DownloadDistValuesLogBucket"></a>

If you chose **On** for **Logging**, the Amazon S3 bucket that you want CloudFront to store access logs in, for example, `myawslogbucket.s3.amazonaws.com`\. If you enable logging, CloudFront records information about each end\-user request for an object and stores the files in the specified Amazon S3 bucket\. You can enable or disable logging at any time\. For more information about CloudFront access logs, see [Access Logs](AccessLogs.md)\.

**Note**  
You must have the permissions required to get and update Amazon S3 bucket ACLs, and the S3 ACL for the bucket must grant you `FULL_CONTROL`\. This allows CloudFront to give the awsdatafeeds account permission to save log files in the bucket\. For more information, see [Permissions Required to Configure Logging and to Access Your Log Files](AccessLogs.md#AccessLogsBucketAndFileOwnership)\.

### Log Prefix<a name="DownloadDistValuesLogPrefix"></a>

Optional\. If you chose **On** for **Logging**, specify the string, if any, that you want CloudFront to prefix to the access log filenames for this distribution, for example, `exampleprefix/`\. The trailing slash \( / \) is optional but recommended to simplify browsing your log files\. For more information about CloudFront access logs, see [Access Logs](AccessLogs.md)\.

### Cookie Logging<a name="DownloadDistValuesCookieLogging"></a>

If you want CloudFront to include cookies in access logs, choose **On**\. If you choose to include cookies in logs, CloudFront logs all cookies regardless of how you configure the cache behaviors for this distribution: forward all cookies, forward no cookies, or forward a specified list of cookies to the origin\. 

Amazon S3 doesn't process cookies, so unless your distribution also includes an Amazon EC2 or other custom origin, we recommend that you choose **Off** for the value of **Cookie Logging**\.

For more information about cookies, go to [Configuring CloudFront to Cache Objects Based on Cookies](Cookies.md)\.

### Enable IPv6<a name="DownloadDistValuesEnableIPv6"></a>

IPv6 is a new version of the IP protocol\. It's the eventual replacement for IPv4 and uses a larger address space\. CloudFront always responds to IPv4 requests\. If you want CloudFront to respond to requests from IPv4 IP addresses \(such as 192\.0\.2\.44\) and requests from IPv6 addresses \(such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334\), select **Enable IPv6**\. 

In general, you should enable IPv6 if you have users on IPv6 networks who want to access your content\. However, if you're using signed URLs or signed cookies to restrict access to your content, and if you're using a custom policy that includes the `IpAddress` parameter to restrict the IP addresses that can access your content, do not enable IPv6\. If you want to restrict access to some content by IP address and not restrict access to other content \(or restrict access but not by IP address\), you can create two distributions\. For information about creating signed URLs by using a custom policy, see [Creating a Signed URL Using a Custom Policy](private-content-creating-signed-url-custom-policy.md)\. For information about creating signed cookies by using a custom policy, see [Setting Signed Cookies Using a Custom Policy](private-content-setting-signed-cookie-custom-policy.md)\.

If you're using an Route 53 alias resource record set to route traffic to your CloudFront distribution, you need to create a second alias resource record set when both of the following are true:

+ You enable IPv6 for the distribution

+ You're using alternate domain names in the URLs for your objects

For more information, see [Routing Traffic to an Amazon CloudFront Web Distribution by Using Your Domain Name](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.

If you created a CNAME resource record set, either with Route 53 or with another DNS service, you don't need to make any changes\. A CNAME record will route traffic to your distribution regardless of the IP address format of the viewer request\.

If you enable IPv6 and CloudFront access logs, the `c-ip` column will include values in IPv4 and IPv6 format\. For more information, see [Access Logs](AccessLogs.md)\.

**Note**  
To maintain high customer availability, CloudFront will respond to viewer requests by using IPv4 if our data suggests that IPv4 will provide a better user experience\. To find out what percentage of requests CloudFront is serving over IPv6, enable CloudFront logging for your distribution and parse the `c-ip` column, which contains the IP address of the viewer that made the request\. This percentage should grow over time, but it will remain a minority of traffic as IPv6 is not yet supported by all viewer networks globally\. Some viewer networks have excellent IPv6 support, but others don't support IPv6 at all\. \(A viewer network is analogous to your home internet or wireless carrier\.\)  
For more information about our support for IPv6, see the [CloudFront FAQ](https://aws.amazon.com/cloudfront/faqs/)\. For information about enabling access logs, see the fields [Logging](#DownloadDistValuesLoggingOnOff), [Bucket for Logs](#DownloadDistValuesLogBucket), and [Log Prefix](#DownloadDistValuesLogPrefix)\.

### Comment<a name="DownloadDistValuesComment"></a>

Optional\. When you create a distribution, you can include a comment of up to 128 characters\. You can update the comment at any time\.

### Distribution State<a name="DownloadDistValuesEnabled"></a>

Indicates whether you want the distribution to be enabled or disabled once it's deployed:

+ *Enabled* means that as soon as the distribution is fully deployed you can deploy links that use the distribution's domain name and users can retrieve content\. Whenever a distribution is enabled, CloudFront accepts and handles any end\-user requests for content that use the domain name associated with that distribution\. 

  When you create, modify, or delete a CloudFront distribution, it takes time for your changes to propagate to the CloudFront database\. An immediate request for information about a distribution might not show the change\. Propagation usually completes within minutes, but a high system load or network partition might increase this time\. 

+ *Disabled* means that even though the distribution might be deployed and ready to use, users can't use it\. Whenever a distribution is disabled, CloudFront doesn't accept any end\-user requests that use the domain name associated with that distribution\. Until you switch the distribution from disabled to enabled \(by updating the distribution's configuration\), no one can use it\.

You can toggle a distribution between disabled and enabled as often as you want\. Follow the process for updating a distribution's configuration\. For more information, see [Listing, Viewing, and Updating CloudFront Distributions](HowToUpdateDistribution.md)\.

## Custom Error Pages and Error Caching<a name="DownloadDistValuesErrorPages"></a>

You can have CloudFront return an object to the viewer \(for example, an HTML file\) when your Amazon S3 or custom origin returns an HTTP 4xx or 5xx status code to CloudFront\. You can also specify how long an error response from your origin or a custom error page is cached in CloudFront edge caches\. For more information, see [Customizing Error Responses](custom-error-pages.md)\.

**Note**  
The following values aren't included in the Create Distribution wizard, so you can configure custom error pages only when you update a distribution\.

### Error Code<a name="DownloadDistValuesErrorCode"></a>

The HTTP status code for which you want CloudFront to return a custom error page\. You can configure CloudFront to return custom error pages for none, some, or all of the HTTP status codes that CloudFront caches\.

### Response Page Path<a name="DownloadDistValuesResponsePagePath"></a>

The path to the custom error page \(for example, `/4xx-errors/403-forbidden.html`\) that you want CloudFront to return to a viewer when your origin returns the HTTP status code that you specified for **Error Code** \(for example, 403\)\. If you want to store your objects and your custom error pages in different locations, your distribution must include a cache behavior for which the following is true:

+ The value of **Path Pattern** matches the path to your custom error messages\. For example, suppose you saved custom error pages for 4xx errors in an Amazon S3 bucket in a directory named `/4xx-errors`\. Your distribution must include a cache behavior for which the path pattern routes requests for your custom error pages to that location, for example, **/4xx\-errors/\***\. 

+ The value of **Origin** specifies the value of **Origin ID** for the origin that contains your custom error pages\.

### Response Code<a name="DownloadDistValuesResponseCode"></a>

The HTTP status code that you want CloudFront to return to the viewer along with the custom error page\. 

### Error Caching Minimum TTL<a name="DownloadDistValuesErrorCachingMinTTL"></a>

The minimum amount of time that you want CloudFront to cache error responses from your origin server\.

## Restrictions<a name="DownloadDistValuesRestrictions"></a>

If you need to prevent users in selected countries from accessing your content, you can configure your CloudFront distribution either to allow users in a whitelist of specified countries to access your content or to not allow users in a blacklist of specified countries to access your content\. For more information, see [Restricting the Geographic Distribution of Your Content](georestrictions.md)\.

**Note**  
The following values aren't included in the Create Distribution wizard, so you can configure geo restrictions only when you update a distribution\.

### Enable Geo Restriction<a name="DownloadDistValuesEnableGeoRestriction"></a>

Whether you want to prevent users in selected countries from accessing your content\. There is no additional charge for configuring geo restriction\.

### Restriction Type<a name="DownloadDistValuesRestrictionType"></a>

How you want to specify the countries from which your users can access your content:

+ **Whitelist:** The **Countries** list includes all of the countries from which you *do* want your users to access your content\.

+ **Blacklist:** The **Countries** list includes all of the countries from which you *do not* want your users to access your content\.

### Countries<a name="DownloadDistValuesCountries"></a>

The countries that you want to add to your whitelist or blacklist\. To add a country, select it in the list on the left and choose **Add**\. Note the following:

+ To add multiple consecutive countries, select the first country, press and hold the Shift key, select the last country, and choose **Add**\. 

+ To add multiple non\-consecutive countries, select the first country, press and hold the Ctrl key, select the remaining countries, and choose **Add**\.

+ To find a country in the left list, enter the first few characters of the country's full name\. 

+ The two\-letter code before the name of each country is the value that you enter if you want to create or update a distribution by using the CloudFront API\. We use the International Organization for Standardization country codes\. For an easy\-to\-use list, sortable by code and by country name, see the Wikipedia entry [ISO 3166\-1 alpha\-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)\.