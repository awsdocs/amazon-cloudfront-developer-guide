# How caching works with CloudFront edge locations<a name="cache-hit-ratio-explained"></a>

One of the purposes of using CloudFront is to reduce the number of requests that your origin server must respond to directly\. With CloudFront caching, more objects are served from CloudFront edge locations, which are closer to your users\. This reduces the load on your origin server and reduces latency\.

The more requests that CloudFront can serve from edge caches, the fewer viewer requests that CloudFront must forward to your origin to get the latest version or a unique version of an object\. To optimize CloudFront to make as few requests to your origin as possible, consider using a CloudFront Origin Shield\. For more information, see [Using Amazon CloudFront Origin Shield](origin-shield.md)\.

The proportion of requests that are served directly from the CloudFront cache compared to all requests is called the *cache hit ratio*\. You can view the percentage of viewer requests that are hits, misses, and errors in the CloudFront console\. For more information, see [CloudFront Cache Statistics Reports](cache-statistics.md)\.

A number of factors affect the cache hit ratio\. You can adjust your CloudFront distribution configuration to improve the cache hit ratio by following the guidance in [Increasing the proportion of requests that are served directly from the CloudFront caches \(cache hit ratio\)](cache-hit-ratio.md)\.