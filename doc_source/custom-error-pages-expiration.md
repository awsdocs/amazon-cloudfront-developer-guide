# Controlling How Long CloudFront Caches Errors<a name="custom-error-pages-expiration"></a>

By default, when your origin returns an HTTP 4xx or 5xx status code, CloudFront caches these error responses for 10 seconds\. CloudFront then submits the next request for the object to your origin to see whether the problem that caused the error has been resolved and the requested object is now available\.

**Note**  
You can create a custom error page for HTTP status code 416 \(Requested Range Not Satisfiable\), and you can change the HTTP status code that CloudFront returns to viewers when your origin returns a status code 416 to CloudFront\. \(For more information, see [Changing Response Codes Returned by CloudFront](custom-error-pages-response-code.md)\.\) However, CloudFront doesn't cache status code 416 responses, so although you can specify a value for **Error Caching Minimum TTL** for status code 416, CloudFront doesn't use it\. 

You can specify the error\-caching duration—the **Error Caching Minimum TTL**—for each 4xx and 5xx status code that CloudFront caches\. For a procedure, see [Configuring Error Response Behavior](custom-error-pages-procedure.md)\. When you specify a duration, note the following:
+ If you specify a short error\-caching duration, CloudFront forwards more requests to your origin than if you specify a longer duration\. For 5xx errors, this may aggravate the problem that originally caused your origin to return an error\.
+ When your origin returns an error for an object, CloudFront responds to requests for the object either with the error response or with your custom error page until the error\-caching duration elapses\. If you specify a long error\-caching duration, CloudFront might continue to respond to requests with an error response or your custom error page for a long time after the object becomes available again\.

If you want to control how long CloudFront caches errors for individual objects, you can configure your origin server to add the applicable header to the error response for that object: 
+ **If the origin adds a `Cache-Control max-age` or `Cache-Control s-maxage` directive, or an `Expires` header:** CloudFront caches error responses for the greater of the value in the header or the value of **Error Caching Minimum TTL**\.

  Be aware that `Cache-Control max-age` and `Cache-Control s-maxage` values cannot be greater than the **Maximum TTL** value set for the cache behavior for which the error page is being fetched\.
+ **If the origin adds other `Cache-Control` directives or adds no headers:** CloudFront caches error responses for the value of **Error Caching Minimum TTL**\.

If the expiration time for a 4xx or 5xx status code for an object is longer than you want to wait, you can invalidate the status code by using the URL of the requested object\. If your origin is returning an error response for multiple objects, you need to invalidate each object separately\. For more information about invalidating objects, see [Invalidating Files](Invalidation.md)\.