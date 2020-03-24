# Requirements and Restrictions on Lambda Functions<a name="lambda-requirements-limits"></a>

See the following sections for requirements and restrictions on using Lambda functions with CloudFront\.

**Topics**
+ [CloudFront Distributions and Associations](#lambda-requirements-distributions)
+ [CloudFront Triggers for Lambda Functions](#lambda-requirements-cloudfront-triggers)
+ [CloudWatch Logs](#lambda-requirements-cloudwatch-logs)
+ [Headers](#lambda-header-restrictions)
+ [HTTP Status Codes](#lambda-requirements-http-status-codes)
+ [Lambda Function Supported Runtimes and Configuration](#lambda-requirements-lambda-function-configuration)
+ [Quotas](#lambda-requirements-see-limits)
+ [Microsoft Smooth Streaming](#lambda-requirements-microsoft-smooth-streaming)
+ [Network Access](#lambda-requirements-network-access)
+ [Query String Parameters](#lambda-requirements-query-strings)
+ [Maximum Size for Body with the Include Body Option](#lambda-requirements-size-body-access)
+ [Tagging](#lambda-requirements-tagging)
+ [URI](#lambda-requirements-uri)
+ [URI and Query String Encoding](#lambda-requirements-encoding)

## CloudFront Distributions and Associations<a name="lambda-requirements-distributions"></a>
+ You cannot associate a Lambda function with a CloudFront distribution owned by another AWS account\. 

## CloudFront Triggers for Lambda Functions<a name="lambda-requirements-cloudfront-triggers"></a>
+ You can add triggers only for a numbered version, not for `$LATEST` or for aliases\.
+ You can add triggers only for functions in the US East \(N\. Virginia\) Region\.
+ To add triggers, the IAM execution role associated with your Lambda function must be assumable by the service principals `lambda.amazonaws.com` and `edgelambda.amazonaws.com`\. For more information, [Setting IAM Permissions and Roles for Lambda@Edge](lambda-edge-permissions.md)\.

## CloudWatch Logs<a name="lambda-requirements-cloudwatch-logs"></a>

For information about Amazon CloudWatch Logs quotas \(formerly known as limits\), see [CloudWatch Logs quotas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch User Guide*\.

## Headers<a name="lambda-header-restrictions"></a>

Note the following requirements and restrictions on using headers with Lambda@Edge\.

**Topics**
+ [Blacklisted Headers](#lambda-blacklisted-headers)
+ [Read\-only Headers](#lambda-read-only-headers)
+ [CloudFront\-\* Headers](#lambda-cloudfront-star-headers)

### Blacklisted Headers<a name="lambda-blacklisted-headers"></a>

Blacklisted headers aren't exposed and can't be added by Lambda@Edge functions\. If your Lambda function adds a blacklisted header, the request fails CloudFront validation\. CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.
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
+ `X-Amzn-*`
+ `X-Cache`
+ `X-Edge-*`
+ `X-Forwarded-Proto`
+ `X-Real-IP`

### Read\-only Headers<a name="lambda-read-only-headers"></a>

Read\-only headers can be read but not edited\. You can use them as input to CloudFront caching logic, and your Lambda function can read the header values, but it can't change the values\. If your Lambda function adds or edits a read\-only header, the request fails CloudFront validation\. CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.

#### Read\-only Headers for CloudFront Viewer Request Events<a name="lambda-read-only-headers-viewer-request-events"></a>
+ `Content-Length`
+ `Host`
+ `Transfer-Encoding`
+ `Via`

#### Read\-only Headers for CloudFront Origin Request Events<a name="lambda-read-only-headers-origin-request-events"></a>
+ `Accept-Encoding`
+ `Content-Length`
+ `If-Modified-Since`
+ `If-None-Match`
+ `If-Range`
+ `If-Unmodified-Since`
+ `Transfer-Encoding`
+ `Via`

#### Read\-only Headers for CloudFront Origin Response Events<a name="lambda-read-only-headers-origin-response-events"></a>
+ `Transfer-Encoding`
+ `Via`

#### Read\-only Headers for CloudFront Viewer Response Events<a name="lambda-read-only-headers-viewer-response-events"></a>
+ `Content-Encoding`
+ `Content-Length`
+ `Transfer-Encoding`
+ `Warning`
+ `Via`

### CloudFront\-\* Headers<a name="lambda-cloudfront-star-headers"></a>

A Lambda function can read, edit, remove, or add any of the following headers\. 
+ `CloudFront-Forwarded-Proto`
+ `CloudFront-Is-Desktop-Viewer`
+ `CloudFront-Is-Mobile-Viewer`
+ `CloudFront-Is-SmartTV-Viewer`
+ `CloudFront-Is-Tablet-Viewer`
+ `CloudFront-Viewer-Country`

Note the following:
+ If you want CloudFront to add these headers, you must configure CloudFront to cache based on these headers\. For information about configuring CloudFront to cache based on specified headers, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.
+ CloudFront adds the headers after the viewer request event\. 
+ If the viewer adds headers that have these names, CloudFront overwrites the header values\.
+ For viewer events, `CloudFront-Viewer-Country` is blacklisted\. Blacklisted headers aren't exposed and can't be added by Lambda@Edge functions\. If your Lambda function adds a blacklisted header, the request fails CloudFront validation, and CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.

For more information, see the following examples:
+ [Example: Redirecting Viewer Requests to a Country\-Specific URL](lambda-examples.md#lambda-examples-redirect-based-on-country)
+ [Example: Serving Different Versions of an Object Based on the Device](lambda-examples.md#lambda-examples-vary-on-device-type)

## HTTP Status Codes<a name="lambda-requirements-http-status-codes"></a>

CloudFront doesn't execute Lambda functions for viewer response events if the origin returns HTTP status code 400 or higher\. You also can't modify the HTTP status code from a viewer response event for a successful request\.

You can, however, execute Lambda functions for *origin* response errors, including HTTP status codes 4xx and 5xx\. For more information, see [Updating HTTP Responses in Origin\-Response Triggers](lambda-updating-http-responses.md)\.

## Lambda Function Supported Runtimes and Configuration<a name="lambda-requirements-lambda-function-configuration"></a>
+ Lambda@Edge supports Lambda functions with the following runtimes:
  + Python 3\.8
  + Python 3\.7
  + Node\.js 12
  + Node\.js 10
  + Node\.js 8 and Node\.js 6
**Note**  
Node\.js versions 8 and 6 have reached end of life\. You can’t create or update functions with these runtimes\. If you have an existing function with one of these runtimes you can still associate it with a CloudFront distribution, and functions that are already associated with a distribution will still run\. However, we recommend moving your function to a newer version of Node\.js\. For more information, see [ Runtime Support Policy](https://docs.aws.amazon.com/lambda/latest/dg/runtime-support-policy.html) in the *AWS Lambda Developer Guide* and the [Node\.js release schedule on GitHub](https://github.com/nodejs/Release#release-schedule)\.
+ You can’t configure your Lambda function to access resources inside your VPC\.
+ You can’t associate your Lambda function with a CloudFront distribution owned by another AWS account\.
+ [AWS Lambda function dead letter queues](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html#dlq) are not supported\.
+ [AWS Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) are not supported\.
+ Functions with [AWS Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html) are not supported\.
+ [Using AWS X\-Ray](https://docs.aws.amazon.com/lambda/latest/dg/lambda-x-ray.html) is not supported\.
+ [AWS Lambda reserved concurrency and provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) are not supported\.

## Quotas<a name="lambda-requirements-see-limits"></a>

The quotas in this section apply to Lambda@Edge\. These quotas are in addition to the default CloudFront and Lambda quotas, which also apply\. For the default quotas, see [Quotas](cloudfront-limits.md) in this guide and [Quotas](https://docs.aws.amazon.com/lambda/latest/dg/limits.html) in the *AWS Lambda Developer Guide*\.

**Note**  
Lambda dynamically scales capacity in response to increased traffic, within your account’s quotas\. For more information, see [Function Scaling](https://docs.aws.amazon.com/lambda/latest/dg/scaling.html) in the *AWS Lambda Developer Guide*\.

In addition, be aware that there are some other restrictions when using Lambda@Edge functions\. For more information, see [Requirements and Restrictions on Lambda Functions](#lambda-requirements-limits)\.


**Quotas that differ by event type**  

| Entity | Origin request and response event quotas | Viewer request and response event quotas | 
| --- | --- | --- | 
| Function memory size | Same as [Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/limits.html) | 128 MB | 
| Function timeout\. The function can make network calls to resources such as Amazon S3 buckets, DynamoDB tables, or Amazon EC2 instances in AWS Regions\. | 30 seconds | 5 seconds | 
| Size of a response that is generated by a Lambda function, including headers and body | 1 MB | 40 KB | 
| Maximum compressed size of a Lambda function and any included libraries | 50 MB | 1 MB | 


**Other quotas**  

| Entity | Default quota | 
| --- | --- | 
| Distributions per AWS account that you can create triggers for |  25 [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
| Triggers per distribution |  100 [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
| Requests per second  |  10,000 \(in each Region\) [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
| Concurrent executions For more information, see [Function Scaling](https://docs.aws.amazon.com/lambda/latest/dg/scaling.html) in the *AWS Lambda Developer Guide*\.  |  1000 \(in each Region\) [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 

### Size Quotas on URI and Query String<a name="lambda-at-the-edge-URI-size-limits-lambda-at-edge"></a>

When accessing or updating a URI or query string in a Lambda@Edge function, the total length of the URI including the query string must be less than 8,192 characters\.

### Size Quotas on Request Body with the Include Body Option<a name="lambda-at-the-edge-body-size-limits-lambda-at-edge"></a>

When you choose the **Include Body** option to expose the request body to your Lambda@Edge function, the following size quotas apply to the portions of the body that are exposed or replaced\.

**Note**  
The body is always base64 encoded by Lambda@Edge before it is exposed\.

#### Size Quotas when Exposing the Body to a Lambda Function<a name="lambda-at-the-edge-exposing-body-size-limits-lambda-at-edge"></a>

If the request body is large, Lambda@Edge truncates it before exposing it, as follows:
+ For viewer requests, the body is truncated at 40 KB\.
+ For origin requests, the body is truncated at 1 MB\.

#### Size Quotas when Returning a Request Body from a Lambda Function<a name="lambda-at-the-edge-returning-body-size-limits-lambda-at-edge"></a>

If you access the request body as read\-only, the full original request body is sent to the origin\. However, if you choose to replace the request body, the following body size quotas apply when it’s returned from a Lambda function:


****  

| Type of body encoding | Allowed body size: Viewer request | Allowed body size: Origin request | 
| --- | --- | --- | 
|  text  |  40 KB  |  1 MB  | 
|  base64  |  53\.2 KB  |  1\.33 MB  | 

## Microsoft Smooth Streaming<a name="lambda-requirements-microsoft-smooth-streaming"></a>

You can’t create triggers for a CloudFront distribution that you’re using for video on demand \(VOD\) streaming of media files that you’ve transcoded into the Microsoft Smooth Streaming format\.

## Network Access<a name="lambda-requirements-network-access"></a>

Functions triggered by origin request and response events as well as functions triggered by viewer request and response events can make network calls to resources on the internet, and to AWS services such as Amazon S3 buckets, DynamoDB tables, or Amazon EC2 instances\.

## Query String Parameters<a name="lambda-requirements-query-strings"></a>
+ To access a query string in a Lambda function, use `event.Records[0].cf.request.querystring`\.
+ A function can update a query string for viewer and origin request events\. The updated query string can't include spaces, control characters, or the fragment identifier \(\#\)\.
+ A function can read a query string only for origin and viewer response events\.
+ Configuring CloudFront to cache based on query string parameters affects whether a function can access the query string:
  + **Viewer request and response events** – A function can access a query string regardless of the setting for [Query String Forwarding and Caching](distribution-web-values-specify.md#DownloadDistValuesQueryString)\.
  + **Origin request and response events** – A function can access a query string only if [Query String Forwarding and Caching](distribution-web-values-specify.md#DownloadDistValuesQueryString) is set either to **Forward All, Cache Based on Whitelist** or to **Forward All, Cache Based on All**\.
+ We recommend that you use percent encoding for the URI and query string\. For more information, see [URI and Query String Encoding](#lambda-requirements-encoding)\.
+ The total size of the URI \(`event.Records[0].cf.request.uri`\) and the query string \(`event.Records[0].cf.request.querystring`\) must be less than 8,192 characters\.

For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.

## Maximum Size for Body with the Include Body Option<a name="lambda-requirements-size-body-access"></a>

For all Lambda@Edge quotas \(formerly known as limits\), including size quotas, see [Quotas](#lambda-requirements-see-limits)\.

## Tagging<a name="lambda-requirements-tagging"></a>

Some AWS services, including Amazon CloudFront and AWS Lambda, support adding tags to resources within the service\. However, at this time, you cannot apply tags to Lambda@Edge resources\. To learn more about tagging in CloudFront, see [Tagging Amazon CloudFront Distributions](tagging.md)\.

## URI<a name="lambda-requirements-uri"></a>

If a function changes the URI for a request, that doesn't change the cache behavior for the request or the origin that the request is forwarded to\.

## URI and Query String Encoding<a name="lambda-requirements-encoding"></a>

Lambda functions require the URI and query string to be UTF\-8 encoded\. \(Percent encoding is compatible with UTF\-8 encoding\.\) The behavior of CloudFront and Lambda depends on the following:
+ The encoding of the URI and query string that CloudFront received in the request from the viewer
+ Whether the URI or query string is changed by a function that is triggered by a viewer request or origin request event

**Values Are UTF\-8 Encoded**  
CloudFront forwards the values to your Lambda function without changing them\.

**Values Are ISO 8859\-1 Encoded**  
CloudFront converts [ISO 8859\-1 character encoding](https://en.wikipedia.org/wiki/ISO/IEC_8859-1) to UTF\-8 encoding before forwarding the values to your Lambda function\.

**Values Are Encoded Using Some Other Character Encoding**  
If the values are encoded using any other character encoding, CloudFront assumes that they're ISO 8859\-1 encoded and tries to convert from ISO 8859\-1 encoding to UTF\-8 encoding\.  
The converted version might be an inaccurate interpretation of the values in the original request\. This can cause a Lambda function or your origin to produce an unintended result\.

The value that CloudFront forwards to your origin server depends on whether functions that are triggered by viewer request or origin request events change the URI or query string:
+ **If the functions don't change the URI or query string** – CloudFront forwards the values that CloudFront received in the request from the viewer to your origin server\.
+ **If the functions do change the URI or query string** – CloudFront forwards the UTF\-8 encoded value\.

In both cases, the behavior is unaffected by the character encoding of the request from the viewer\.