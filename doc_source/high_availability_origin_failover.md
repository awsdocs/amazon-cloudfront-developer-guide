# Optimizing High Availability with CloudFront Origin Failover<a name="high_availability_origin_failover"></a>

You can set up CloudFront with origin failover for scenarios that require high availability\. To get started, create an *origin group* in which you designate a primary origin for CloudFront plus a second origin that CloudFront automatically switches to when the primary origin returns specific HTTP status code failure responses\.

To set up origin failover, you must have a distribution with at least two origins\. Next, you create an origin group for your distribution that includes the two origins, setting one as the primary\. Finally, you define a cache behavior in which you specify the origin group as your origin\. For more information about specifying origin groups for distributions, see [Origin ID](distribution-web-values-specify.md#DownloadDistValuesId)\.

The two origins in the origin group can be any combination of the following: AWS origins, like Amazon S3 buckets or Amazon EC2 instances, or custom origins, like your own HTTP web server\. When you create the origin group, you configure CloudFront to failover to the second origin for GET, HEAD, and OPTIONS HTTP methods when the primary origin returns specific status codes that you configure\. 

To see the steps for setting up origin groups with specific origin failover options, see [Creating an Origin Group](#concept_origin_groups.creating)\.

After you configure origin failover for a cache behavior, CloudFront does the following for viewer requests:
+ When there’s a cache hit, CloudFront returns the requested file\.
+ When there’s a cache miss, CloudFront routes the request to the primary origin that you identified in the origin group\.
+ When a status code that has not been configured for failover is returned, such as an HTTP 2xx or HTTP 3xx status code, CloudFront serves the requested content\.
+ When the primary origin returns an HTTP status code that you’ve configured for failover, or after a timeout, CloudFront routes the request to the backup origin in the origin group\.
**Note**  
CloudFront issues a connect timeout when it cannot connect to the origin after three consecutive attempts for a single request\. CloudFront waits up to 10 seconds between connection attempts\.

CloudFront routes all incoming requests to the primary origin, even when an earlier request has failed over to a second origin\. CloudFront only sends requests to the second origin after an attempt to route to the primary origin returns a status code configured for failover or times out\.

The following diagram illustrates how origin failover works\.

![\[How origin failover works\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

For more information, see the following:
+ **Cache hits and misses:** [How Caching Works with CloudFront Edge Caches](cache-hit-ratio-explained.md)
+ **Request and response behavior with origin failover:** [Request and Response Behavior for Origin Groups](RequestAndResponseBehaviorOriginGroups.md)

## Creating an Origin Group<a name="concept_origin_groups.creating"></a>

You create an origin group with two origins\. You specify one as the primary origin, plus a second origin that CloudFront automatically switches to when the primary origin returns specific HTTP status code failure responses\.\. <a name="create-origin-groups-procedure"></a>

**To create an origin group**

1. On the **Origin and Origin Groups** tab, choose **Create origin group**\.

1. Choose the origins for the origin group, and then choose the **Priority** arrows to set one of them as the Primary origin\.

1. Select one or more HTTP status codes as the failover criteria\. You can choose any combination of the following status codes: 500, 502, 503, 504, 404, or 403\.

1. Enter a unique descriptive id for the origin group\. You can't use an id that is already being used for an origin or for another origin group\.

For information about specifying an origin group for a distribution, see [Origin ID](distribution-web-values-specify.md#DownloadDistValuesId)\.

## Use Origin Failover with Lambda@Edge Functions<a name="concept_origin_groups.lambda"></a>

You can use Lambda@Edge functions with CloudFront distributions that you’ve set up with origin groups\. To use a Lambda function, specify it in an origin request or origin response trigger for an origin group when you create the cache behavior\.

The Lambda function is triggered for the primary origin when CloudFront routes a request to it on a cache miss\. If the primary origin returns an HTTP status code that you’ve configured for failover, the Lambda function is triggered again, when CloudFront re\-routes the request to the second origin\.

The following diagram illustrates how origin failover works when you include a Lambda@Edge function in an origin request or response trigger\.

![\[How origin failover works with Lambda@Edge functions\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

For more information about using Lambda@Edge triggers, see [Adding Triggers for a Lambda@Edge Function](lambda-edge-add-triggers.md)\.

## Use Origin Failover with Custom Error Pages<a name="concept_origin_groups.custom-error"></a>

You can use custom error pages with origin groups similarly to how you use them with origins that are not set up for origin failover\. 

When you use origin failover, CloudFront can be configured to return a custom error page for the primary or secondary origin \(or both\):
+ **Return a custom error page for the primary origin\.** If the primary origin returns an HTTP status code error that you have not configured for failover, CloudFront returns the custom error page to viewers\.
+ **Return a custom error page for the secondary origin\.** If CloudFront fails over to the second origin, CloudFront returns a custom error page\.

For more information about using custom error pages with CloudFront, see [Generating Custom Error Responses](GeneratingCustomErrorResponses.md)\.