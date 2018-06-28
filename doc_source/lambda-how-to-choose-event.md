# How to Decide Which CloudFront Event to Use to Trigger a Lambda Function<a name="lambda-how-to-choose-event"></a>

When you're deciding which CloudFront event you want to use to trigger a Lambda function, consider the following:

**Do you want CloudFront to cache objects that are changed by a Lambda function?**  
If you want CloudFront to cache an object that was modified by a Lambda function so that CloudFront can serve the object from the edge location the next time it's requested, use the origin request or origin response event\. This reduces the load on the origin, reduces latency for subsequent requests, and reduces the cost of invoking Lambda@Edge on subsequent requests\.  
For example, if you want to add, remove, or change headers for objects that are returned by the origin and you want CloudFront to cache the result, use the origin response event\.

**Do you want the function to execute for every request?**  
If you want the function to execute for every request that CloudFront receives for the distribution, use the viewer request or viewer response events\. Origin request and origin response events occur only when a requested object isn't cached in an edge location and CloudFront forwards a request to the origin\.

**Does the function change the cache key?**  
If you want the function to change a value that you're using as a basis for caching, use the viewer request event\. For example, if a function changes the URL to include a language abbreviation in the path \(for example, because the user chose their language from a dropdown list\), use the viewer request event:  
+ **URL in the viewer request** – http://example\.com/en/index\.html
+ **URL when the request comes from an IP address in Germany** – http://example\.com/de/index\.html
You also use the viewer request event if you're caching based on cookies or request headers\.  
If the function changes cookies or headers, configure CloudFront to forward the applicable part of the request to the origin\. For more information, see the following topics:  
+ [Caching Content Based on Cookies](Cookies.md)
+ [Caching Content Based on Request Headers](header-caching.md)

**Does the function affect the response from the origin?**  
If you want the function to change the request in a way that affects the response from the origin, use the origin request event\. Typically, most viewer request events aren't forwarded to the origin; CloudFront responds to a request with an object that's already in the edge cache\. If the function changes the request based on an origin request event, CloudFront caches the response to the changed origin request\.