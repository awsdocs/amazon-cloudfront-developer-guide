# How Caching Works with CloudFront Edge Caches<a name="cache-hit-ratio-explained"></a>

One of the purposes of using CloudFront is to reduce the number of requests that your origin server must respond to directly\. This reduces the load on your origin server and also reduces latency because more objects are served from CloudFront edge locations, which are closer to your users\. 

The more requests that CloudFront is able to serve from edge caches as a proportion of all requests \(that is, the greater the *cache hit ratio*\), the fewer viewer requests that CloudFront needs to forward to your origin to get the latest version or a unique version of an object\. 

You can view the percentage of viewer requests that are hits, misses, and errors in the CloudFront console\. For more information, see [CloudFront Cache Statistics Reports](cache-statistics.md)\. 

A number of factors affect the cache hit ratio\. You can adjust your CloudFront distribution configuration to improve the cache hit ratio by following the guidance in [Increasing the Proportion of Requests that Are Served from CloudFront Edge Caches \(Cache Hit Ratio\)](cache-hit-ratio.md)\.