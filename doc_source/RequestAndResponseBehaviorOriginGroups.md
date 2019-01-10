# Request and Response Behavior for Origin Groups<a name="RequestAndResponseBehaviorOriginGroups"></a>

Requests to an origin group work similarly to requests for an origin that is not set up as an origin group, except when there is a cache miss which results in origin failover\. As with any other origin, when CloudFront receives a request and the content is already cached in an edge location, the content is served to viewers from the cache\. When you've set up origin failover and there’s a cache miss, viewer requests are forwarded to the primary origin in the origin group\.

The request and response behavior for the primary origin is the same as it is for an origin that isn't included in an origin group\. For more information, see [Request and Response Behavior for Amazon S3 Origins](RequestAndResponseBehaviorS3Origin.md) and [Request and Response Behavior for Custom Origins](RequestAndResponseBehaviorCustomOrigin.md)\.

The following describes the behavior for origin failover when the primary origin returns specific HTTP status codes:
+ HTTP 2xx status code \(success\): CloudFront caches the file and returns it to the viewer\.
+ HTTP 3xx status code \(redirection\): CloudFront returns the status code to the viewer\.
+ HTTP 4xx or 5xx status code \(client/server error\): If the returned status code has been configured for failover, CloudFront switches to the second origin in the origin group and requests the file\.
+ HTTP 4xx or 5xx status code \(client/server error\): If the returned status code has not been configured for failover, CloudFront returns the error to the viewer\.

When CloudFront requests the file from the second origin, the response behavior is the same as for a CloudFront origin that has not been set up in an origin group\.

For more information about origin groups, see [Optimizing High Availability with CloudFront Origin Failover](high_availability_origin_failover.md)\.