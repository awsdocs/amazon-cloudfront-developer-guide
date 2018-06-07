# Updating Existing Objects Using the Same Object Names<a name="ReplacingObjectsSameName"></a>

Although you can update existing objects in a CloudFront distribution and use the same object names, we don't recommend it\. CloudFront distributes objects to edge locations only when the objects are requested, not when you put new or updated objects in your origin\. If you update an existing object in your origin with a newer version that has the same name, an edge location won't get that new version from your origin until both of the following occur:
+ The old version of the object in the cache expires\. For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.
+ There's an end user request for the object at that edge location\.

If you use the same names when you replace objects, you can't control when CloudFront starts to serve the new files\. By default, CloudFront caches objects in edge locations for 24 hours\. \(For more information, see [Specifying How Long Objects Stay in a CloudFront Edge Cache \(Expiration\)](Expiration.md)\.\) For example, if you're replacing all of the objects on an entire website:
+ Objects for the less popular pages may not be in any edge locations\. The new versions of these objects will start being served on the next request\.
+ Objects for some pages may be in some edge locations and not in others, so your end users will see different versions depending on which edge location they're served from\.
+ New versions of the objects for the most popular pages might not be served for up to 24 hours because CloudFront might have retrieved the objects for those pages just before you replaced the objects with new versions\.