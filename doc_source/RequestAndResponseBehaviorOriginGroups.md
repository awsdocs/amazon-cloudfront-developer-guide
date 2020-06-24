# Request and Response Behavior for Origin Groups<a name="RequestAndResponseBehaviorOriginGroups"></a>

Requests to an origin group work the same as requests to an origin that is not set up as an origin group, except when there is an origin failover\. As with any other origin, when CloudFront receives a request and the content is already cached in an edge location, the content is served to viewers from the cache\. When there’s a cache miss and the origin is an origin group, viewer requests are forwarded to the primary origin in the origin group\.

The request and response behavior for the primary origin is the same as it is for an origin that isn’t in an origin group\. For more information, see [Request and Response Behavior for Amazon S3 Origins](RequestAndResponseBehaviorS3Origin.md) and [Request and Response Behavior for Custom Origins](RequestAndResponseBehaviorCustomOrigin.md)\.

The following describes the behavior for origin failover when the primary origin returns specific HTTP status codes:
+ HTTP 2xx status code \(success\): CloudFront caches the file and returns it to the viewer\.
+ HTTP 3xx status code \(redirection\): CloudFront returns the status code to the viewer\.
+ HTTP 4xx or 5xx status code \(client/server error\): If the returned status code has been configured for failover, CloudFront sends the same request to the secondary origin in the origin group\.
+ HTTP 4xx or 5xx status code \(client/server error\): If the returned status code has not been configured for failover, CloudFront returns the error to the viewer\.

CloudFront fails over to the secondary origin only when the HTTP method of the viewer request is `GET`, `HEAD`, or `OPTIONS`\. CloudFront does not fail over when the viewer sends a different HTTP method \(for example `POST`, `PUT`, and so on\)\.

When CloudFront sends a request to a secondary origin, the response behavior is the same as for a CloudFront origin that’s not in an origin group\.

For more information about origin groups, see [Optimizing High Availability with CloudFront Origin Failover](high_availability_origin_failover.md)\.