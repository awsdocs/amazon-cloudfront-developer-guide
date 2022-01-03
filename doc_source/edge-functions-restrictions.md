# Restrictions on edge functions<a name="edge-functions-restrictions"></a>

The following topics describe the restrictions that apply to CloudFront Functions and Lambda@Edge\. Some restrictions apply to all edge functions, while others apply only to CloudFront Functions or Lambda@Edge\.

For information about quotas \(formerly referred to as limits\), see [Quotas on CloudFront Functions](cloudfront-limits.md#limits-functions) and [Quotas on Lambda@Edge](cloudfront-limits.md#limits-lambda-at-edge)\.

**Topics**
+ [Restrictions on all edge functions](#edge-function-restrictions-all)
+ [Restrictions on CloudFront Functions](#cloudfront-function-restrictions)
+ [Restrictions on Lambda@Edge](#lambda-at-edge-function-restrictions)

## Restrictions on all edge functions<a name="edge-function-restrictions-all"></a>

The following restrictions apply to all edge functions, both CloudFront Functions and Lambda@Edge\.

### AWS account ownership<a name="function-restrictions-account-ownership"></a>

To associate an edge function with a CloudFront distribution, the function and distribution must be owned by the same AWS account\.

### Combining CloudFront Functions with Lambda@Edge<a name="function-restrictions-combining-functions"></a>

For a given cache behavior, the following restrictions apply:
+ Each event type \(viewer request, origin request, origin response, and viewer response\) can have only one edge function association\.
+ You cannot combine CloudFront Functions and Lambda@Edge in viewer events \(viewer request and viewer response\)\.

All other combinations of edge functions are allowed\. The following table explains the allowed combinations\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions-restrictions.html)

### HTTP status codes<a name="function-restrictions-status-codes"></a>

CloudFront does not invoke edge functions for viewer response events when the origin returns HTTP status code 400 or higher\.

Edge functions for viewer response events cannot modify the HTTP status code of the response, regardless of whether the response came from the origin or the CloudFront cache\.

Lambda@Edge functions for origin response events are invoked for *all* origin responses, including when the origin returns HTTP status code 400 or higher\. For more information, see [Updating HTTP responses in origin response triggers](lambda-updating-http-responses.md)\.

### HTTP headers<a name="function-restrictions-headers"></a>

Certain HTTP headers are disallowed, which means they’re not exposed to edge functions and functions can’t add them\. Other headers are read\-only, which means functions can read them but can’t add or modify them\.

#### Disallowed headers<a name="function-restrictions-disallowed-headers"></a>

The following HTTP headers are not exposed to edge functions, and functions can’t add them\. If your function adds one of these headers, it fails CloudFront validation and CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.
+ `Connection` 
+ `Expect`
+ `Keep-Alive`
+ `Proxy-Authenticate`
+ `Proxy-Authorization`
+ `Proxy-Connection`
+ `Trailer`
+ `Upgrade`
+ `X-Accel-Buffering`
+ `X-Accel-Charset`
+ `X-Accel-Limit-Rate`
+ `X-Accel-Redirect`
+ `X-Amz-Cf-*`
+ `X-Cache`
+ `X-Edge-*`
+ `X-Forwarded-Proto`
+ `X-Real-IP`

#### Read\-only headers<a name="function-restrictions-read-only-headers"></a>

The following headers are read\-only\. Your function can read them and use them as input to the function logic, but it can’t change the values\. If your function adds or edits a read\-only header, the request fails CloudFront validation and CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.

##### Read\-only headers in viewer request events<a name="function-restrictions-read-only-headers-viewer-request"></a>

The following headers are read\-only in viewer request events\.
+ `Content-Length`
+ `Host`
+ `Transfer-Encoding`
+ `Via`

##### Read\-only headers in origin request events \(Lambda@Edge only\)<a name="function-restrictions-read-only-headers-origin-request"></a>

The following headers are read\-only in origin request events, which exist only in Lambda@Edge\.
+ `Accept-Encoding`
+ `Content-Length`
+ `If-Modified-Since`
+ `If-None-Match`
+ `If-Range`
+ `If-Unmodified-Since`
+ `Transfer-Encoding`
+ `Via`

##### Read\-only headers in origin response events \(Lambda@Edge only\)<a name="function-restrictions-read-only-headers-origin-response"></a>

The following headers are read\-only in origin response events, which exist only in Lambda@Edge\.
+ `Transfer-Encoding`
+ `Via`

##### Read\-only headers in viewer response events<a name="function-restrictions-read-only-headers-viewer-response"></a>

The following headers are read\-only in viewer response events\.
+ `Content-Encoding`
+ `Content-Length`
+ `Transfer-Encoding`
+ `Warning`
+ `Via`

### Query strings<a name="function-restrictions-query-strings"></a>

The following restrictions apply to functions that read, update, or create a query string in a request URI\.
+ \(Lambda@Edge only\) To access the query string in an origin request or origin response function, your cache policy or origin request policy must be set to **All** for **Query strings**\.
+ A function can create or update a query string for viewer request and origin request events \(origin request events exist only in Lambda@Edge\)\.
+ A function can read a query string, but cannot create or update one, for origin response and viewer response events \(origin response events exist only in Lambda@Edge\)\.
+ If a function creates or updates a query string, the following restrictions apply:
  + The query string can’t include spaces, control characters, or the fragment identifier \(`#`\)\.
  + The total size of the URI, including the query string, must be less than 8,192 characters\.
  + We recommend that you use percent encoding for the URI and query string\. For more information, see [URI and query string encoding](#function-restrictions-encoding)\.

### URI<a name="function-restrictions-uri"></a>

If a function changes the URI for a request, that doesn’t change the cache behavior for the request or the origin that the request is forwarded to\.

The total size of the URI, including the query string, must be less than 8,192 characters\.

### URI and query string encoding<a name="function-restrictions-encoding"></a>

URI and query string values passed to edge functions are UTF\-8 encoded\. Your function should use UTF\-8 encoding for the URI and query string values that it returns\. Percent encoding is compatible with UTF\-8 encoding\.

The following list explains how CloudFront handles URI and query string value encoding:
+ When values in the request are UTF\-8 encoded, CloudFront forwards the values to your function without changing them\.
+ When values in the request are [ISO\-8859\-1 encoded](https://en.wikipedia.org/wiki/ISO/IEC_8859-1), CloudFront converts the values to UTF\-8 encoding before forwarding them to your function\.
+ When values in the request are encoded using some other character encoding, CloudFront assumes that they’re ISO\-8859\-1 encoded and tries to convert from ISO\-8859\-1 to UTF\-8\.
**Important**  
The converted characters might be an inaccurate interpretation of the values in the original request\. This might cause your function or your origin to produce an unintended result\.

The URI and query string values that CloudFront forwards to your origin depend on whether a function changes the values:
+ If a function does not change the URI or query string, CloudFront forwards the values that it received in the request to your origin\.
+ If a function changes the URI or query string, CloudFront forwards the UTF\-8 encoded values\.

### Microsoft Smooth Streaming<a name="function-restrictions-microsoft-smooth-streaming"></a>

You cannot use edge functions with a CloudFront distribution that you’re using for streaming media files that you’ve transcoded into the Microsoft Smooth Streaming format\.

### Tagging<a name="function-restrictions-tagging"></a>

You cannot add tags to edge functions\. To learn more about tagging in CloudFront, see [Tagging Amazon CloudFront distributions](tagging.md)\.

## Restrictions on CloudFront Functions<a name="cloudfront-function-restrictions"></a>

The following restrictions apply only to CloudFront Functions\.

### Logs<a name="cloudfront-function-restrictions-logs"></a>

Function logs in CloudFront Functions are truncated at 10 KB\.

### Request body<a name="cloudfront-function-restrictions-request-body"></a>

CloudFront Functions cannot access the body of the HTTP request\.

### Runtime<a name="cloudfront-function-runtime-restrictions"></a>

The CloudFront Functions runtime environment does not support dynamic code evaluation, and it restricts access to the network, file system, and timers\. For more information, see [Restricted features](functions-javascript-runtime-features.md#writing-functions-javascript-features-restricted-features)\.

### Compute utilization<a name="cloudfront-function-restrictions-compute-utilization"></a>

CloudFront Functions have a limit on the time they can take to run, measured as *compute utilization*\. Compute utilization is a number between 0 and 100 that indicates the amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a compute utilization of 35 means that the function completed in 35% of the maximum allowed time\.

When you [test a function](test-function.md), you can see the compute utilization value in the output of the test event\. For production functions, you can view the [compute utilization metric](monitoring-functions.md#monitoring-functions-metrics) on the [Monitoring page in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home?#/monitoring), or in CloudWatch\.

## Restrictions on Lambda@Edge<a name="lambda-at-edge-function-restrictions"></a>

The following restrictions apply only to Lambda@Edge\.

### Lambda function version<a name="lambda-at-edge-restrictions-version"></a>

You must use a numbered version of the Lambda function, not `$LATEST` or aliases\.

### Lambda region<a name="lambda-at-edge-restrictions-region"></a>

The Lambda function must be in the US East \(N\. Virginia\) Region\.

### Lambda role permissions<a name="lambda-at-edge-restrictions-role-permissions"></a>

The IAM execution role associated with the Lambda function must allow the service principals `lambda.amazonaws.com` and `edgelambda.amazonaws.com` to assume the role\. For more information, see [Setting IAM permissions and roles for Lambda@Edge](lambda-edge-permissions.md)\.

### Lambda features and supported runtimes<a name="lambda-at-edge-runtime-restrictions"></a>

The following Lambda features are not supported by Lambda@Edge:
+ You can’t configure your Lambda function to access resources inside your VPC\.
+ [Lambda function dead letter queues](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html#dlq) are not supported\.
+ [Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) are not supported\.
+ Lambda functions with [AWS Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html) are not supported\.
+ [Using AWS X\-Ray](https://docs.aws.amazon.com/lambda/latest/dg/lambda-x-ray.html) is not supported\.
+ [Lambda reserved concurrency and provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) are not supported\.
+ [Lambda functions defined as container images](https://docs.aws.amazon.com/lambda/latest/dg/configuration-images.html) are not supported\.
+ [Lambda functions that use the arm64 architecture](https://docs.aws.amazon.com/lambda/latest/dg/foundation-arch.html) are not supported\.

Lambda@Edge supports Lambda functions with the following runtimes:


|  Node\.js  |  Python  | 
| --- | --- | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions-restrictions.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions-restrictions.html)  | 

¹This version of Node\.js has reached end of life\. You can’t create or update functions with this version\. If you have an existing function with this version, you can associate it with a CloudFront distribution\. Functions with this version that are already associated with a distribution continue to run\. However, we recommend moving your function to a newer version of Node\.js\. For more information, see [ Runtime Support Policy](https://docs.aws.amazon.com/lambda/latest/dg/runtime-support-policy.html) in the *AWS Lambda Developer Guide* and the [Node\.js release schedule](https://github.com/nodejs/Release#release-schedule) on GitHub\.

²This version of Node\.js has reached end of life, and it reaches end of support phase 2 in Lambda on August 30, 2021\. Starting on August 30, 2021, you can’t create or update functions with this version\. If you have an existing function with this version, you can associate it with a CloudFront distribution\. Functions with this version that are already associated with a distribution continue to run\. However, we recommend moving your function to a newer version of Node\.js\. For more information, see [ Runtime Support Policy](https://docs.aws.amazon.com/lambda/latest/dg/runtime-support-policy.html) in the *AWS Lambda Developer Guide* and the [Node\.js release schedule](https://github.com/nodejs/Release#release-schedule) on GitHub\.

### CloudFront headers<a name="lambda-at-edge-restrictions-cloudfront-headers"></a>

Lambda@Edge functions can read, edit, remove, or add any of the following CloudFront headers:
+ `CloudFront-Forwarded-Proto`
+ `CloudFront-Is-Desktop-Viewer`
+ `CloudFront-Is-Mobile-Viewer`
+ `CloudFront-Is-SmartTV-Viewer`
+ `CloudFront-Is-Tablet-Viewer`
+ `CloudFront-Viewer-Country`¹

Note the following:
+ If you want CloudFront to add these headers, you must configure CloudFront to add them using a [cache policy](controlling-the-cache-key.md) or [origin request policy](controlling-origin-requests.md)\.
+ CloudFront adds the headers after the viewer request event, which means they are not available to Lambda@Edge in a viewer request function\.
+ If the viewer request includes headers that have these names, and you configured CloudFront to add these headers using a [cache policy](controlling-the-cache-key.md) or [origin request policy](controlling-origin-requests.md), then CloudFront overwrites the header values that were in the viewer request\. Viewer\-facing functions see the header value from the viewer request, while origin\-facing functions see the header value that CloudFront added\.
+ ¹`CloudFront-Viewer-Country` header – If a viewer request function adds this header, it fails validation and CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.

### Restrictions on the request body with the include body option<a name="lambda-at-edge-restrictions-request-body"></a>

When you choose the **Include Body** option to expose the request body to your Lambda@Edge function, the following information and size quotas apply to the portions of the body that are exposed or replaced\.
+ CloudFront always base64 encodes the request body before exposing it to Lambda@Edge\.
+ If the request body is large, CloudFront truncates it before exposing it to Lambda@Edge, as follows:
  + For viewer request events, the body is truncated at 40 KB\.
  + For origin request events, the body is truncated at 1 MB\.
+ If you access the request body as read\-only, CloudFront sends the full original request body to the origin\.
+ If your Lambda@Edge function replaces the request body, the following size quotas apply to the body that the function returns:
  + If the Lambda@Edge function returns the body as plain text:
    + For viewer request events, the body is truncated at 40 KB\.
    + For origin request events, the body is truncated at 1 MB\.
  + If the Lambda@Edge function returns the body as base64 encoded text:
    + For viewer request events, the body is truncated at 53\.2 KB\.
    + For origin request events, the body is truncated at 1\.33 MB\.