# Optimizing High Availability with CloudFront Origin Failover<a name="high_availability_origin_failover"></a>

You can set up CloudFront with origin failover for scenarios that require high availability\. To get started, you create an *origin group* with two origins: a primary and a secondary\. If the primary origin is unavailable, or returns specific HTTP response status codes that indicate a failure, CloudFront automatically switches to the secondary origin\.

To set up origin failover, you must have a distribution with at least two origins\. Next, you create an origin group for your distribution that includes two origins, setting one as the primary\. Finally, you create or update a cache behavior to use the origin group\.

To see the steps for setting up origin groups and configuring specific origin failover options, see [Creating an Origin Group](#concept_origin_groups.creating)\.

After you configure origin failover for a cache behavior, CloudFront does the following for viewer requests:
+ When there’s a cache hit, CloudFront returns the requested file\.
+ When there’s a cache miss, CloudFront routes the request to the primary origin in the origin group\.
+ When the primary origin returns a status code that’s configured for failover, such as an HTTP 2xx or 3xx status code, CloudFront serves the requested content to the viewer\.
+ When any of the following occur:
  + The primary origin returns an HTTP status code that you’ve configured for failover
  + CloudFront fails to connect to the primary origin
**Note**  
CloudFront times out when it cannot connect to the origin after three consecutive attempts for a single request\. CloudFront waits up to 10 seconds between connection attempts\.
  + The response from the primary origin takes too long \(times out\)

  Then CloudFront routes the request to the secondary origin in the origin group\.

CloudFront routes all incoming requests to the primary origin, even when a previous request failed over to the secondary origin\. CloudFront only sends requests to the secondary origin after a request to the primary origin fails\.

The following diagram illustrates how origin failover works\.

![\[How origin failover works\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origingroups-overview.png)

For more information, see the following:
+ **Cache hits and misses:** [How Caching Works with CloudFront Edge Caches](cache-hit-ratio-explained.md)
+ **Request and response behavior with origin failover:** [Request and Response Behavior for Origin Groups](RequestAndResponseBehaviorOriginGroups.md)

**Topics**
+ [Creating an Origin Group](#concept_origin_groups.creating)
+ [Use Origin Failover with Lambda@Edge Functions](#concept_origin_groups.lambda)
+ [Use Custom Error Pages with Origin Failover](#concept_origin_groups.custom-error)

## Creating an Origin Group<a name="concept_origin_groups.creating"></a><a name="create-origin-groups-procedure"></a>

**To create an origin group**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the distribution that you want to create the origin group for\.

1. Choose the **Origins and Origin Groups** tab\.

1. On the **Origin and Origin Groups** tab, choose **Create Origin Group**\.

1. Choose the origins for the origin group\. After you add origins, use the arrows to set the priority—that is, which origin is primary and which is secondary\.

1. Choose the HTTP status codes to use as failover criteria\. You can choose any combination of the following status codes: 500, 502, 503, 504, 404, or 403\. When CloudFront receives a response with one of the status codes that you specify, it fails over to the secondary origin\.

1. Enter a unique identifier for the origin group\. You can’t use an identifier that’s already in use for a different origin or origin group in your AWS account\.

For information about specifying an origin group for a distribution, see [Origin ID](distribution-web-values-specify.md#DownloadDistValuesId)\.

## Use Origin Failover with Lambda@Edge Functions<a name="concept_origin_groups.lambda"></a>

You can use Lambda@Edge functions with CloudFront distributions that you’ve set up with origin groups\. To use a Lambda function, specify it in an [origin request or origin response trigger](lambda-cloudfront-trigger-events.md) for an origin group when you create the cache behavior\. When you use a Lambda@Edge function with an origin group, the function can be triggered twice for a single viewer request\. For example, consider this scenario:

1. You create a Lambda@Edge function with an origin request trigger\.

1. The Lambda function is triggered once when CloudFront sends a request to the primary origin \(on a cache miss\)\.

1. The primary origin responds with an HTTP status code that’s configured for failover\.

1. The Lambda function is triggered again when CloudFront sends the same request to the secondary origin\.

The following diagram illustrates how origin failover works when you include a Lambda@Edge function in an origin request or response trigger\.

![\[How origin failover works with Lambda@Edge functions\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origingroups-with-lambda-edge.png)

For more information about using Lambda@Edge triggers, see [Adding Triggers for a Lambda@Edge Function](lambda-edge-add-triggers.md)\.

## Use Custom Error Pages with Origin Failover<a name="concept_origin_groups.custom-error"></a>

You can use custom error pages with origin groups similarly to how you use them with origins that are not set up for origin failover\. 

When you use origin failover, you can configure CloudFront to return a custom error page for the primary or secondary origin \(or both\):
+ **Return a custom error page for the primary origin** – If the primary origin returns an HTTP status code that’s not configured for failover, CloudFront returns the custom error page to viewers\.
+ **Return a custom error page for the secondary origin** – If CloudFront receives a failure status code from the secondary origin, CloudFront returns the custom error page\.

For more information about using custom error pages with CloudFront, see [Generating Custom Error Responses](GeneratingCustomErrorResponses.md)\.