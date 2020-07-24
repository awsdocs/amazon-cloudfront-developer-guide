# Working with policies<a name="working-with-policies"></a>

With CloudFront *policies*, you can control the values that are included in the *cache key* for objects that are cached at CloudFront edge locations\. These values can include HTTP request query strings, headers, and cookies\. The cache key determines whether a viewer request results in a *cache hit* \(the object is served to the viewer from a CloudFront edge location\)\.

When there’s a *cache miss* \(the requested object is not cached at the edge location\), CloudFront sends a request to the origin to retrieve the object\. This is called an *origin request*\. You can separately control which of these values \(query strings, headers, and cookies\) are included in the origin request\.

You control the cache key with a *cache policy* and the origin request with an *origin request policy*\. By controlling the cache key and the origin request separately, you can forward request values to your origin without duplicating cached content when the content doesn’t differ based on those values\.

For more information, see the following\.

**Contents**
+ [Controlling the cache key](controlling-the-cache-key.md)
  + [Understanding cache policies](controlling-the-cache-key.md#cache-key-understand-cache-policy)
    + [Policy information](controlling-the-cache-key.md#cache-key-understand-cache-policy-info)
    + [Time to live \(TTL\) settings](controlling-the-cache-key.md#cache-key-understand-cache-policy-ttl)
    + [Cache key settings](controlling-the-cache-key.md#cache-key-understand-cache-policy-settings)
  + [Creating cache policies](controlling-the-cache-key.md#cache-key-create-cache-policy)
  + [Using the managed cache policies](using-managed-cache-policies.md)
    + [Attaching a managed cache policy](using-managed-cache-policies.md#attaching-managed-cache-policies)
    + [Understanding the managed cache policies](using-managed-cache-policies.md#managed-cache-policies-list)
  + [Understanding the cache key](understanding-the-cache-key.md)
    + [The default cache key](understanding-the-cache-key.md#cache-key-default)
    + [Customizing the cache key](understanding-the-cache-key.md#cache-key-custom)
+ [Controlling origin requests](controlling-origin-requests.md)
  + [Understanding origin request policies](controlling-origin-requests.md#origin-request-understand-origin-request-policy)
    + [Policy information](controlling-origin-requests.md#origin-request-understand-origin-request-policy-info)
    + [Origin request settings](controlling-origin-requests.md#origin-request-understand-origin-request-policy-settings)
  + [Creating origin request policies](controlling-origin-requests.md#origin-request-create-origin-request-policy)
  + [Using the managed origin request policies](using-managed-origin-request-policies.md)
    + [Attaching a managed origin request policy](using-managed-origin-request-policies.md#attaching-managed-origin-request-policies)
    + [Understanding the managed origin request policies](using-managed-origin-request-policies.md#managed-origin-request-policies-list)
+ [Using the CloudFront HTTP headers](using-cloudfront-headers.md)
  + [Headers for determining the viewer’s device type](using-cloudfront-headers.md#cloudfront-headers-device-type)
  + [Headers for determining the viewer’s location](using-cloudfront-headers.md#cloudfront-headers-viewer-location)
  + [Other CloudFront headers](using-cloudfront-headers.md#cloudfront-headers-other)