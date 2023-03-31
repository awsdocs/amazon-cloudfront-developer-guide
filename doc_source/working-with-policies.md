# Working with policies<a name="working-with-policies"></a>

Amazon CloudFront offers three different kinds of *policies* that you can use to customize CloudFront in the following ways:

**Specify cache and compression settings**  
With a CloudFront *cache policy*, you can specify the HTTP headers, cookies, and query strings that CloudFront includes in the *cache key*\. The cache key determines whether a viewer's HTTP request results in a *cache hit* \(the object is served to the viewer from the CloudFront cache\)\. Including fewer values in the cache key increases the likelihood of a cache hit\.  
You can also use the cache policy to specify time to live \(TTL\) settings for objects in the CloudFront cache, and enable CloudFront to request and cache compressed objects\.

**Specify values to include in origin requests \(but not in the cache key\)**  
With a CloudFront *origin request policy*, you can specify the HTTP headers, cookies, and query strings that CloudFront includes in *origin requests*\. These are the requests that CloudFront sends to the origin when there's a cache miss\.  
All of the values in the cache policy are automatically included in origin requests, but with an origin request policy you can include additional values in origin requests without including them in the cache key\.

**Specify HTTP headers to remove or add in viewer responses**  
With a CloudFront *response headers policy*, you can control the HTTP headers that CloudFront includes in HTTP responses that it sends to viewers \(web browsers or other clients\)\. You can remove headers from the origin's HTTP response, or add HTTP headers to the responses that CloudFront sends to viewers, without making any changes to your origin or writing any code\.

For more information, see the following topics\.

**Topics**
+ [Controlling the cache key](controlling-the-cache-key.md)
+ [Controlling origin requests](controlling-origin-requests.md)
+ [Adding or removing response headers](modifying-response-headers.md)