# Generating custom error responses<a name="GeneratingCustomErrorResponses"></a>

If an object that you’re serving through CloudFront is unavailable for some reason, your web server typically returns a relevant HTTP status code to CloudFront to indicate this\. For example, if a viewer requests an invalid URL, your web server returns an HTTP 404 \(Not Found\) status code to CloudFront, and CloudFront returns that status code to the viewer\.

You can configure CloudFront to return a custom error response to the viewer instead, if you like\. You also have several options for managing how CloudFront responds when there’s an error\. To specify options for custom error messages, you update your CloudFront distribution to specify those values\. For more information, see [Configuring error response behavior](#custom-error-pages-procedure)\.

If you configure CloudFront to return a custom error page for an HTTP status code but the custom error page isn’t available, CloudFront returns to the viewer the status code that CloudFront received from the origin that contains the custom error pages\. For example, suppose your custom origin returns a 500 status code and you have configured CloudFront to get a custom error page for a 500 status code from an Amazon S3 bucket\. However, someone accidentally deleted the custom error page from your bucket\. CloudFront returns an HTTP 404 status code \(Not Found\) to the viewer that requested the object\.

When CloudFront returns a custom error page to a viewer, you pay the standard CloudFront charges for the custom error page, not the charges for the requested object\. For more information about CloudFront charges, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

**Topics**
+ [Configuring error response behavior](#custom-error-pages-procedure)
+ [Creating a custom error page for specific HTTP status codes](#creating-custom-error-pages)
+ [Storing objects and custom error pages in different locations](#custom-error-pages-different-locations)
+ [Changing response codes returned by CloudFront](#custom-error-pages-response-code)
+ [Controlling how long CloudFront caches errors](#custom-error-pages-expiration)

## Configuring error response behavior<a name="custom-error-pages-procedure"></a>

To configure custom error responses, you can use the CloudFront console, the CloudFront API, or AWS CloudFormation\. Regardless of how you choose to update the configuration, consider the following tips and recommendations:
+ Save your custom error pages in a location that is accessible to CloudFront\. We recommend that you store them in an Amazon S3 bucket, and that you [don’t store them in the same place as the rest of your website or application’s content](#custom-error-pages-different-locations)\. If you store the custom error pages on the same origin as your website or application, and the origin starts to return 5xx errors, CloudFront can’t get the custom error pages because the origin server is unavailable\. For more information, see [Storing objects and custom error pages in different locations](#custom-error-pages-different-locations)\.
+ Make sure that CloudFront has permission to get your custom error pages\. If the custom error pages are stored in Amazon S3, the pages must be publicly accessible or you must configure a CloudFront [origin access identity \(OAI\)](private-content-restricting-access-to-s3.md)\. If the custom error pages are stored in a custom origin, the pages must be publicly accessible\.
+ \(Optional\) Configure your origin to add a `Cache-Control` or `Expires` header along with the custom error pages, if you want\. You can also use the **Error Caching Minimum TTL** setting to control how long CloudFront caches the custom error pages\. For more information, see [Controlling how long CloudFront caches errors](#custom-error-pages-expiration)\.

### Configure custom error responses \(CloudFront console\)<a name="custom-error-pages-console"></a>

To configure custom error responses in the CloudFront console, you must have a CloudFront distribution\. In the console, the configuration settings for custom error responses are only available for existing distributions\. To learn how to create a distribution, see [Getting started with a simple CloudFront distribution](GettingStarted.SimpleDistribution.md)\.

**To configure custom error responses \(console\)**

1. Sign in to the AWS Management Console and open the **Distributions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home#distributions](https://console.aws.amazon.com/cloudfront/v3/home#distributions)\.

1. In the list of distributions, choose the distribution to update\.

1. Choose the **Error Pages** tab, then choose **Create Custom Error Response**\.

1. Enter the applicable values\. For more information, see [Custom Error Pages and Error Caching](distribution-web-values-specify.md#DownloadDistValuesErrorPages) in *Values That You Specify When You Create or Update a Distribution*\.

1. After entering the desired values, choose **Create**\.

### Configure custom error responses \(CloudFront API or AWS CloudFormation\)<a name="custom-error-pages-api-cfn"></a>

To configure custom error responses with the CloudFront API or AWS CloudFormation, use the `CustomErrorResponse` type in a distribution\. For more information, see the following:
+ [AWS::CloudFront::Distribution CustomErrorResponse](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-cloudfront-distribution-customerrorresponse.html) in the *AWS CloudFormation User Guide*
+ [CustomErrorResponse](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CustomErrorResponse.html) in the *Amazon CloudFront API Reference*

## Creating a custom error page for specific HTTP status codes<a name="creating-custom-error-pages"></a>

If you’d rather display a custom error message instead of the default message—for example, a page that uses the same formatting as the rest of your website—you can have CloudFront return to the viewer an object \(such as an HTML file\) that contains your custom error message\.

To specify the file that you want to return and the errors for which the file should be returned, you update your CloudFront distribution to specify those values\. For more information, see [Configuring error response behavior](#custom-error-pages-procedure)\.

For example, the following is a custom error page:

![\[AWS 404 page\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/custom-error-page-aws-404-example.png)

You can specify a different object for each supported HTTP status code, or you can use the same object for all of the supported status codes\. You can choose to specify custom error pages for some status codes and not for others\.

The objects that you’re serving through CloudFront can be unavailable for a variety of reasons\. These fall into two broad categories:
+ *Client errors* indicate a problem with the request\. For example, an object with the specified name isn’t available, or the user doesn’t have the permissions required to get an object in your Amazon S3 bucket\. When a client error occurs, the origin returns an HTTP status code in the 4xx range to CloudFront\.
+ *Server errors* indicate a problem with the origin server\. For example, the HTTP server is busy or unavailable\. When a server error occurs, either your origin server returns an HTTP status code in the 5xx range to CloudFront, or CloudFront doesn’t get a response from your origin server for a certain period of time and assumes a 504 status code \(Gateway Timeout\)\.

The HTTP status codes for which CloudFront can return a custom error page include the following:
+ 400, 403, 404, 405, 414, 416
**Note**  
You can create a custom error page for HTTP status code 416 \(Requested Range Not Satisfiable\), and you can change the HTTP status code that CloudFront returns to viewers when your origin returns a status code 416 to CloudFront\. \(For more information, see [Changing response codes returned by CloudFront](#custom-error-pages-response-code)\.\) However, CloudFront doesn’t cache status code 416 responses, so even if you specify a value for **Error Caching Minimum TTL** for status code 416, CloudFront doesn’t use it\.
+ 500, 501, 502, 503, 504
**Note**  
In some cases, CloudFront doesn’t return a custom error page for the HTTP 503 status code even if you configure CloudFront to do so\. If the CloudFront error code is `Capacity Exceeded` or `Limit Exceeded`, CloudFront returns a 503 status code to the viewer without using your custom error page\.

For a detailed explanation of how CloudFront handles error responses from your origin, see [How CloudFront processes and caches HTTP 4xx and 5xx status codes from your origin](HTTPStatusCodes.md)\.

## Storing objects and custom error pages in different locations<a name="custom-error-pages-different-locations"></a>

If you want to store your objects and your custom error pages in different locations, your distribution must include a cache behavior for which the following is true:
+ The value of **Path Pattern** matches the path to your custom error messages\. For example, suppose you saved custom error pages for 4xx errors in an Amazon S3 bucket in a directory named `/4xx-errors`\. Your distribution must include a cache behavior for which the path pattern routes requests for your custom error pages to that location, for example, `/4xx-errors/*`\.
+ The value of **Origin** specifies the value of **Origin ID** for the origin that contains your custom error pages\.

For more information, see [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

## Changing response codes returned by CloudFront<a name="custom-error-pages-response-code"></a>

You can configure CloudFront to return a different HTTP status code to the viewer than what CloudFront received from the origin\. For example, if your origin returns a 500 status code to CloudFront, you might want CloudFront to return a custom error page and a 200 status code \(OK\) to the viewer\. There are a variety of reasons that you might want CloudFront to return a status code to the viewer that is different from the one that your origin returned to CloudFront:
+ Some internet devices \(some firewalls and corporate proxies, for example\) intercept HTTP 4xx and 5xx status codes and prevent the response from being returned to the viewer\. In this scenario, if you substitute `200`, the response is not intercepted\.
+ If you don’t care about distinguishing among different client errors or server errors, you can specify `400` or `500` as the value that CloudFront returns for all 4xx or 5xx status codes\.
+ You might want to return a `200` status code \(OK\) and a static website so your customers don’t know that your website is down\.

If you enable [CloudFront standard logs](AccessLogs.md) and you configure CloudFront to change the HTTP status code in the response, the value of the `sc-status` column in the logs contains the status code that you specify\. However, the value of the `x-edge-result-type` column is not affected\. It contains the result type of the response from the origin\. For example, suppose you configure CloudFront to return a status code of `200` to the viewer when the origin returns `404` \(Not Found\) to CloudFront\. When the origin responds to a request with a `404` status code, the value in the `sc-status` column in the log will be `200`, but the value in the `x-edge-result-type` column will be `Error`\.

You can configure CloudFront to return any of the following HTTP status codes along with a custom error page:
+ 200
+ 400, 403, 404, 405, 414, 416
+ 500, 501, 502, 503, 504

## Controlling how long CloudFront caches errors<a name="custom-error-pages-expiration"></a>

By default, when your origin returns an HTTP 4xx or 5xx status code, CloudFront caches these error responses for 10 seconds\. CloudFront then submits the next request for the object to your origin to see whether the problem that caused the error has been resolved and the requested object is now available\.

**Note**  
You can create a custom error page for HTTP status code 416 \(Requested Range Not Satisfiable\), and you can change the HTTP status code that CloudFront returns to viewers when your origin returns a status code 416 to CloudFront\. \(For more information, see [Changing response codes returned by CloudFront](#custom-error-pages-response-code)\.\) However, CloudFront doesn’t cache status code 416 responses, so even if you specify a value for **Error Caching Minimum TTL** for status code 416, CloudFront doesn’t use it\.

You can specify the error\-caching duration—the **Error Caching Minimum TTL**—for each 4xx and 5xx status code that CloudFront caches\. When you specify a duration, note the following:
+ If you specify a short error\-caching duration, CloudFront forwards more requests to your origin than if you specify a longer duration\. For 5xx errors, this might aggravate the problem that originally caused your origin to return an error\.
+ When your origin returns an error for an object, CloudFront responds to requests for the object either with the error response or with your custom error page until the error\-caching duration elapses\. If you specify a long error\-caching duration, CloudFront might continue to respond to requests with an error response or your custom error page for a long time after the object becomes available again\.

If you want to control how long CloudFront caches errors for individual objects, you can configure your origin server to add the applicable header to the error response for that object: 
+ **If the origin adds a `Cache-Control: max-age` or `Cache-Control: s-maxage` directive, or an `Expires` header:**

  CloudFront caches error responses for the greater of the value in the header or the value of **Error Caching Minimum TTL**\.

  Be aware that the `Cache-Control: max-age` and `Cache-Control: s-maxage` values cannot be greater than the **Maximum TTL** value set for the cache behavior for which the error page is being fetched\.
+ **If the origin adds other `Cache-Control` directives or adds no headers:**

  CloudFront caches error responses for the value of **Error Caching Minimum TTL**\.

If the expiration time for a 4xx or 5xx status code for an object is longer than you want, and the object is available again, you can invalidate cached error code by using the URL of the requested object\. If your origin is returning an error response for multiple objects, you need to invalidate each object separately\. For more information about invalidating objects, see [Invalidating Files](Invalidation.md)\.