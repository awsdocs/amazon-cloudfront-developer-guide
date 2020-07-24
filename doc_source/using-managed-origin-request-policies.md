# Using the managed origin request policies<a name="using-managed-origin-request-policies"></a>

CloudFront provides a set of managed origin request policies that you can attach to any of your distribution’s cache behaviors\. With a managed origin request policy, you don’t need to write or maintain your own origin request policy\. The managed policies use settings that are optimized for specific use cases\.

**Contents**
+ [Attaching a managed origin request policy](#attaching-managed-origin-request-policies)
+ [Understanding the managed origin request policies](#managed-origin-request-policies-list)

## Attaching a managed origin request policy<a name="attaching-managed-origin-request-policies"></a>

To use a managed origin request policy, you attach it to a cache behavior in your distribution\. The process is the same as when you create an origin request policy, but instead of creating a new one, you just attach one of the managed origin request policies\. You attach the policy either by name \(with the console\) or by ID \(with the AWS CLI or SDKs\)\. The names and IDs are listed in the following section\.

For more information, see [Creating origin request policies](controlling-origin-requests.md#origin-request-create-origin-request-policy)\.

## Understanding the managed origin request policies<a name="managed-origin-request-policies-list"></a>

The following list describes the managed origin request policies\.

**Name: Managed\-UserAgentRefererHeadersID: acba4595\-bd28\-49b8\-b9fe\-13317c0390fa**  
This policy includes only the `User-Agent` and `Referer` headers\. It doesn’t include any query strings or cookies\.  
**Policy settings**  
+ **Query strings included in origin requests:** None
+ **Headers included in origin requests:**
  + `User-Agent`
  + `Referer`
+ **Cookies included in origin requests:** None

**Name: Managed\-CORS\-CustomOriginID: 59781a5b\-3903\-41f3\-afcb\-af62929ccde1**  
This policy includes the header that enables cross\-origin resource sharing \(CORS\) requests when the origin is a custom origin\.  
**Policy settings**  
+ **Query strings included in origin requests:** None
+ **Headers included in origin requests:**
  + `Origin`
+ **Cookies included in origin requests:** None

**Name: Managed\-CORS\-S3OriginID: 88a5eaf4\-2fd4\-4709\-b370\-b4c650ea3fcf**  
This policy includes the headers that enable cross\-origin resource sharing \(CORS\) requests when the origin is an Amazon S3 bucket\.  
**Policy settings**  
+ **Query strings included in origin requests:** None
+ **Headers included in origin requests:**
  + `Origin`
  + `Access-Control-Request-Headers`
  + `Access-Control-Request-Method`
+ **Cookies included in origin requests:** None

**Name: Managed\-AllViewerID: 216adef6\-5c7f\-47e4\-b989\-5492eafa07d3**  
This policy includes all values \(query strings, headers, and cookies\) in the viewer request\.  
**Policy settings**  
+ **Query strings included in origin requests:** All
+ **Headers included in origin requests:** All headers in the viewer request
+ **Cookies included in origin requests:** All

**Name: Managed\-Elemental\-MediaTailor\-PersonalizedManifestsID: 775133bc\-15f2\-49f9\-abea\-afb2e0bf67d2**  
This policy is designed for use with an origin that is an AWS Elemental MediaTailor endpoint\.  
**Policy settings**  
+ **Query strings included in origin requests:** All
+ **Headers included in origin requests:**
  + `Origin`
  + `Access-Control-Request-Headers`
  + `Access-Control-Request-Method`
  + `User-Agent`
  + `X-Forwarded-For`
+ **Cookies included in origin requests:** None