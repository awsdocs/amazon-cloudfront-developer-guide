# Requirements and Restrictions on Lambda Functions<a name="lambda-requirements-limits"></a>

See the following sections for requirements and restrictions on using Lambda functions with CloudFront\.

**Topics**
+ [CloudFront Distributions and Associations](#lambda-requirements-distributions)
+ [CloudFront Triggers for Lambda Functions](#lambda-requirements-cloudfront-triggers)
+ [CloudWatch Logs](#lambda-requirements-cloudwatch-logs)
+ [Headers](#lambda-header-restrictions)
+ [HTTP Status Codes](#lambda-requirements-http-status-codes)
+ [Lambda Function Configuration and Execution Environment](#lambda-requirements-lambda-function-configuration)
+ [Limits](#lambda-requirements-see-limits)
+ [Microsoft Smooth Streaming](#lambda-requirements-microsoft-smooth-streaming)
+ [Network Access](#lambda-requirements-network-access)
+ [Query String Parameters](#lambda-requirements-query-strings)
+ [URI](#lambda-requirements-uri)
+ [URI and Query String Encoding](#lambda-requirements-encoding)

## CloudFront Distributions and Associations<a name="lambda-requirements-distributions"></a>
+ You can create triggers \(associations\) for Lambda functions for a maximum of 25 distributions per AWS account\.
+ You can create a maximum of 100 triggers \(associations\) for a distribution\. 
+ You cannot associate a Lambda function with a CloudFront distribution owned by another AWS account\. 

## CloudFront Triggers for Lambda Functions<a name="lambda-requirements-cloudfront-triggers"></a>
+ You can add triggers only for a numbered version, not for `$LATEST` or for aliases\.
+ You can add triggers only for functions in the US East \(N\. Virginia\) Region\.
+ To add triggers, the IAM execution role associated with your Lambda function must be assumable by the service principals `lambda.amazonaws.com` and `edgelambda.amazonaws.com`\. For more information, see [Setting IAM Permissions and Roles for Lambda@Edge](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-permissions) in the *IAM User Guide*\.

## CloudWatch Logs<a name="lambda-requirements-cloudwatch-logs"></a>

For information about Amazon CloudWatch Logs limits, see [CloudWatch Logs Limits](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch User Guide*\.

## Headers<a name="lambda-header-restrictions"></a>

Note the following requirements and restrictions on using headers with Lambda@Edge\.

**Topics**
+ [Blacklisted Headers](#lambda-blacklisted-headers)
+ [Read\-only Headers](#lambda-read-only-headers)
+ [Restricted Headers for CloudFront Origin Request Events](#lambda-restricted-headers)
+ [CloudFront\-\* Headers](#lambda-cloudfront-star-headers)

### Blacklisted Headers<a name="lambda-blacklisted-headers"></a>

Blacklisted headers aren't exposed and can't be added by Lambda@Edge functions\. If your Lambda function adds a blacklisted header, the request fails CloudFront validation\. CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.
+ Connection 
+ Expect
+ Keep\-alive
+ Proxy\-Authenticate
+ Proxy\-Authorization
+ Proxy\-Connection
+ Trailer
+ Upgrade
+ X\-Accel\-Buffering
+ X\-Accel\-Charset
+ X\-Accel\-Limit\-Rate
+ X\-Accel\-Redirect
+ X\-Amz\-Cf\-\*
+ X\-Amzn\-\*
+ X\-Cache
+ X\-Edge\-\*
+ X\-Forwarded\-Proto
+ X\-Real\-IP

### Read\-only Headers<a name="lambda-read-only-headers"></a>

Read\-only headers can be read but not edited\. You can use them as input to CloudFront caching logic, and your Lambda function can read the header values, but it can't change the values\. If your Lambda function adds or edits a read\-only header, the request fails CloudFront validation\. CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.

#### Read\-only Headers for CloudFront Viewer Request Events<a name="lambda-read-only-headers-viewer-request-events"></a>
+ Content\-Length
+ Host
+ Transfer\-Encoding
+ Via

#### Read\-only Headers for CloudFront Origin Request Events<a name="lambda-read-only-headers-origin-request-events"></a>
+ Accept\-Encoding
+ Content\-Length
+ If\-Modified\-Since
+ If\-None\-Match
+ If\-Range
+ If\-Unmodified\-Since
+ Range
+ Transfer\-Encoding
+ Via

#### Read\-only Headers for CloudFront Origin Response Events<a name="lambda-read-only-headers-origin-response-events"></a>
+ Transfer\-Encoding
+ Via

#### Read\-only Headers for CloudFront Viewer Response Events<a name="lambda-read-only-headers-viewer-response-events"></a>
+ Content\-Encoding
+ Content\-Length
+ Transfer\-Encoding
+ Warning
+ Via

### Restricted Headers for CloudFront Origin Request Events<a name="lambda-restricted-headers"></a>

By default, CloudFront removes the following headers from viewer requests\. You can add or edit these headers in CloudFront origin request events only if the CloudFront distribution is configured to cache based on the headers\. This causes CloudFront to forward the headers to your origin instead of removing them\.

If a Lambda function adds or changes a restricted header and the CloudFront distribution is not configured to cache based on that header, the request fails CloudFront validation\. CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\. 

For information about how to configure CloudFront to cache based on specified headers, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders) in the topic [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md)\.
+ Accept
+ Accept\-Charset
+ Accept\-Language
+ Authorization
+ Referer
+ TE

### CloudFront\-\* Headers<a name="lambda-cloudfront-star-headers"></a>

A Lambda function can read, edit, remove, or add any of the following headers\. 
+ CloudFront\-Forwarded\-Proto
+ CloudFront\-Is\-Desktop\-Viewer
+ CloudFront\-Is\-Mobile\-Viewer
+ CloudFront\-Is\-SmartTV\-Viewer
+ CloudFront\-Is\-Tablet\-Viewer
+ CloudFront\-Viewer\-Country

Note the following:
+ If you want CloudFront to add these headers, you must configure CloudFront to cache based on these headers\. For information about configuring CloudFront to cache based on specified headers, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders) in the topic [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md)\.
+ CloudFront adds the headers after the viewer request event\. 
+ If the viewer adds headers that have these names, CloudFront overwrites the header values\.
+ For viewer events, CloudFront\-Viewer\-Country is blacklisted\. Blacklisted headers aren't exposed and can't be added by Lambda@Edge functions\. If your Lambda function adds a blacklisted header, the request fails CloudFront validation, and CloudFront returns HTTP status code 502 \(Bad Gateway\) to the viewer\.

For more information, see the following examples:
+ [Example: Redirecting Viewer Requests to a Country\-Specific URL](lambda-examples.md#lambda-examples-redirect-based-on-country)
+ [Example: Serving Different Versions of an Object Based on the Device](lambda-examples.md#lambda-examples-vary-on-device-type)

## HTTP Status Codes<a name="lambda-requirements-http-status-codes"></a>

CloudFront doesn't execute Lambda functions for viewer response events if the origin returns HTTP status code 400 or higher\.

## Lambda Function Configuration and Execution Environment<a name="lambda-requirements-lambda-function-configuration"></a>
+ You must create functions with the `nodejs6.10` runtime property\.
+ You can't configure your Lambda function to access resources inside your VPC\.
+ You can't associate your Lambda function to a CloudFront distribution owned by another AWS account\. 
+ The Dead Letter Queue \(DLQ\) isn't supported\.
+ Environment variables aren't supported\.

## Limits<a name="lambda-requirements-see-limits"></a>

For information about limits, see the following documentation:
+ [Limits on Lambda@Edge](cloudfront-limits.md#limits-lambda-at-edge) in this guide
+ [AWS Lambda Limits](http://docs.aws.amazon.com/lambda/latest/dg/limits.html) in the *AWS Lambda Developer Guide*

## Microsoft Smooth Streaming<a name="lambda-requirements-microsoft-smooth-streaming"></a>

You can't create triggers for a CloudFront distribution that you're using for on\-demand streaming of media files that you've transcoded into the Microsoft Smooth Streaming format\.

## Network Access<a name="lambda-requirements-network-access"></a>

Functions triggered by origin request and response events as well as functions triggered by viewer request and response events can make network calls to resources on the internet, and to services in AWS regions such as Amazon S3 buckets, DynamoDB tables, or Amazon EC2 instances\.

## Query String Parameters<a name="lambda-requirements-query-strings"></a>
+ To access a query string in a Lambda function, use `event.Records[0].cf.request.querystring`\.
+ A function can update a query string for viewer and origin request events\. The updated query string can't include spaces, control characters, or the fragment identifier \(\#\)\.
+ A function can read a query string only for origin and viewer response events\.
+ Configuring CloudFront to cache based on query string parameters affects whether a function can access the query string:
  + **Viewer request and response events** – A function can access a query string regardless of the setting for [Query String Forwarding and Caching](distribution-web-values-specify.md#DownloadDistValuesQueryString)\.
  + **Origin request and response events** – A function can access a query string only if [Query String Forwarding and Caching](distribution-web-values-specify.md#DownloadDistValuesQueryString) is set either to **Forward All, Cache Based on Whitelist** or to **Forward All, Cache Based on All**\.
+ We recommend that you use percent encoding for the URI and query string\. For more information, see [URI and Query String Encoding](#lambda-requirements-encoding)\.
+ The total size of the URI \(`event.Records[0].cf.request.uri`\) and the query string \(`event.Records[0].cf.request.querystring`\) must be less than 8,192 characters\.

For more information, see [Configuring CloudFront to Cache Based on Query String Parameters](QueryStringParameters.md)\.

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