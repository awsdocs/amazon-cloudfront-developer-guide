# Using the managed cache policies<a name="using-managed-cache-policies"></a>

CloudFront provides a set of managed cache policies that you can attach to any of your distribution’s cache behaviors\. With a managed cache policy, you don’t need to write or maintain your own cache policy\. The managed policies use settings that are optimized for specific use cases\.

**Topics**
+ [Attaching a managed cache policy](#attaching-managed-cache-policies)
+ [Understanding the managed cache policies](#managed-cache-policies-list)

## Attaching a managed cache policy<a name="attaching-managed-cache-policies"></a>

To use a managed cache policy, you attach it to a cache behavior in your distribution\. The process is the same as when you create a cache policy, but instead of creating a new one, you just attach one of the managed cache policies\. You attach the policy either by name \(with the console\) or by ID \(with the AWS CLI or SDKs\)\. The names and IDs are listed in the following section\.

For more information, see [Creating cache policies](controlling-the-cache-key.md#cache-key-create-cache-policy)\.

## Understanding the managed cache policies<a name="managed-cache-policies-list"></a>

The following list describes the managed cache policies\.

**Name: CachingOptimizedID: 658327ea\-f89d\-4fab\-a63d\-7e88639e58f6**  
This policy is designed to optimize cache efficiency by minimizing the values that CloudFront includes in the cache key\. CloudFront doesn’t include any query strings or cookies in the cache key, and only includes the normalized `Accept-Encoding` header\. This enables CloudFront to separately cache objects in the Gzip and Brotli compressions formats when the origin returns them or when [CloudFront edge compression](ServingCompressedFiles.md) is enabled\.  
**Policy settings**  
+ **Minimum TTL:** 1 second\.
+ **Maximum TTL:** 31,536,000 seconds \(365 days\)\.
+ **Default TTL:** 86,400 seconds \(24 hours\)\.
+ **Headers included in the cache key:** None are explicitly included\. The normalized `Accept-Encoding` header is included because the cache compressed objects setting is enabled\. For more information, see [Compression support](controlling-the-cache-key.md#cache-policy-compressed-objects)\.
+ **Cookies included in the cache key:** None\.
+ **Query strings included in the cache key:** None\.
+ **Cache compressed objects setting:** Enabled\. For more information, see [Compression support](controlling-the-cache-key.md#cache-policy-compressed-objects)\.

**Name: CachingOptimizedForUncompressedObjectsID: b2884449\-e4de\-46a7\-ac36\-70bc7f1ddd6d**  
This policy is designed to optimize cache efficiency by minimizing the values included in the cache key\. No query strings, headers, or cookies are included\. This policy is identical to the previous one, but it disables the cache compressed objects setting\.  
**Policy settings**  
+ **Minimum TTL:** 1 second
+ **Maximum TTL:** 31,536,000 seconds \(365 days\)
+ **Default TTL:** 86,400 seconds \(24 hours\)
+ **Headers included in the cache key:** None
+ **Cookies included in the cache key:** None
+ **Query strings included in the cache key:** None
+ **Cache compressed objects setting:** Disabled

**Name: CachingDisabledID: 4135ea2d\-6df8\-44a3\-9df3\-4b5a84be39ad**  
This policy disables caching\. This policy is useful for dynamic content and for requests that are not cacheable\.  
**Policy settings**  
+ **Minimum TTL:** 0 seconds
+ **Maximum TTL:** 0 seconds
+ **Default TTL:** 0 seconds
+ **Headers included in the cache key:** None
+ **Cookies included in the cache key:** None
+ **Query strings included in the cache key:** None
+ **Cache compressed objects setting:** Disabled

**Name: Elemental\-MediaPackageID: 08627262\-05a9\-4f76\-9ded\-b50ca2e3a84f**  
This policy is designed for use with an origin that is an AWS Elemental MediaPackage endpoint\.  
**Policy settings**  
+ **Minimum TTL:** 0 seconds
+ **Maximum TTL:** 31,536,000 seconds \(365 days\)
+ **Default TTL:** 86,400 seconds \(24 hours\)
+ **Headers included in the cache key:**
  + `Origin`

  The normalized `Accept-Encoding` header is also included because the cache compressed objects setting is enabled for Gzip\. For more information, see [Compression support](controlling-the-cache-key.md#cache-policy-compressed-objects)\.
+ **Cookies included in the cache key:** None
+ **Query strings included in the cache key:**
  + `aws.manifestfilter`
  + `start`
  + `end`
  + `m`
+ **Cache compressed objects setting:** Enabled for Gzip\. For more information, see [Compression support](controlling-the-cache-key.md#cache-policy-compressed-objects)\.

**Name: AmplifyID: 2e54312d\-136d\-493c\-8eb9\-b001f22f67d2**  
This policy is designed for use with an origin that is an [AWS Amplify](http://aws.amazon.com/amplify/) web app\.  
**Policy settings**  
+ **Minimum TTL:** 2 seconds
+ **Maximum TTL:** 600 seconds \(10 minutes\)
+ **Default TTL:** 2 seconds
+ **Headers included in cache key:**
  + `Authorization`
  + `CloudFront-Viewer-Country`
  + `Host`

  The normalized `Accept-Encoding` header is also included because the cache compressed objects setting is enabled\. For more information, see [Compression support](controlling-the-cache-key.md#cache-policy-compressed-objects)\.
+ **Cookies included in cache key:** All cookies are included\.
+ **Query strings included in cache key:** All query strings are included\.
+ **Cache compressed objects setting:** Enabled\. For more information, see [Compression support](controlling-the-cache-key.md#cache-policy-compressed-objects)\.