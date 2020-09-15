# Quotas<a name="cloudfront-limits"></a>

CloudFront is subject to the following quotas \(formerly referred to as limits\)\. Note that Lambda@Edge has specific quotas as well, that are in addition to the default CloudFront quotas\.

**Topics**
+ [General Quotas](#limits-general)
+ [General Quotas on Web Distributions](#limits-web-distributions)
+ [General Quotas on Policies](#limits-policies)
+ [Quotas on WebSocket Connections](#limits-websockets)
+ [Quotas on Whitelisted Cookies \(Web Distributions Only\)](#limits-whitelisted-cookies)
+ [Quotas on Whitelisted Query Strings \(Web Distributions Only\)](#limits-whitelisted-query-strings)
+ [Quotas on Custom Headers \(Web Distributions Only\)](#limits-custom-headers)
+ [Quotas on SSL Certificates \(Web Distributions Only\)](#limits-ssl-certificates)
+ [Quotas on Invalidations](#limits-invalidations)
+ [Quotas on Field\-Level Encryption](#limits-field-level-encryption)
+ [Quotas on Lambda@Edge](#limits-lambda-at-edge)
+ [Request Timeout](#limits-request-timeout)
+ [Quotas on RTMP Distributions](#limits-rtmp-distributions)

## General Quotas<a name="limits-general"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Data transfer rate per distribution | 150 Gbps [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Requests per second per distribution | 250,000 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Tags that can be added to a distribution | 50 | 
| Files that you can serve per distribution | No quota | 
| Maximum length of a request, including headers and query strings, but not including the body content | 20,480 bytes | 
| Maximum length of a URL | 8,192 bytes | 
| Active CloudFront key pairs for trusted signers For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.  | 2 | 

## General Quotas on Web Distributions<a name="limits-web-distributions"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Web distributions per AWS account For more information, see [Creating a Distribution](distribution-web-creating-console.md)\.  | 200 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Maximum file size for HTTP GET, POST, and PUT requests | 20 GB | 
| Response timeout per origin For more information, see [Origin Response Timeout](distribution-web-values-specify.md#DownloadDistValuesOriginResponseTimeout)\.  | 1\-60 seconds [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Connection timeout per origin For more information, see [Origin Connection Timeout](distribution-web-values-specify.md#origin-connection-timeout)\.  |  1\-10 seconds  | 
|  Connection attempts per origin For more information, see [Origin Connection Attempts](distribution-web-values-specify.md#origin-connection-attempts)\.  |  1\-3  | 
| File compression: range of file sizes that CloudFront compresses For more information, see [Serving compressed files](ServingCompressedFiles.md)\.  | 1,000 to 10,000,000 bytes | 
| Alternate domain names \(CNAMEs\) per distribution For more information, see [Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.  | 100 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Origins per distribution | 25 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Origin groups per distribution | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Origin access identities per account  |  100 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Cache behaviors per distribution | 25 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 

## General Quotas on Policies<a name="limits-policies"></a>


****  

| Entity | Default quota | 
| --- | --- | 
|  Cache policies per AWS account  |  20  | 
|  Distributions associated with the same cache policy  |  100  | 
|  Query strings per cache policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Headers per cache policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Cookies per cache policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Origin request policies per AWS account  |  20  | 
|  Distributions associated with the same origin request policy  |  100  | 
|  Query strings per origin request policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Headers per origin request policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Cookies per origin request policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 

## Quotas on WebSocket Connections<a name="limits-websockets"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Origin response timeout \(idle timeout\) | 10 minutes If CloudFront hasn’t detected any bytes sent from the origin to the client within the past 10 minutes, the connection is assumed to be idle and is closed\. | 

## Quotas on Whitelisted Cookies \(Web Distributions Only\)<a name="limits-whitelisted-cookies"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Whitelisted cookies per cache behavior For more information, see [Caching Content Based on Cookies](Cookies.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Total number of bytes in whitelisted cookie names \(doesn’t apply if you configure CloudFront to forward all cookies to the origin\) | 512 minus the number of whitelisted cookies | 

## Quotas on Whitelisted Query Strings \(Web Distributions Only\)<a name="limits-whitelisted-query-strings"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Maximum number of characters in a whitelisted query string  | 128 characters  | 
| Maximum number of characters total for all whitelisted query strings in the same parameter  | 512 characters  | 
| Whitelisted query strings per cache behavior For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 

## Quotas on Custom Headers \(Web Distributions Only\)<a name="limits-custom-headers"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Whitelisted headers per cache behavior For more information, see [Caching Content Based on Request Headers](header-caching.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Custom headers: maximum number of custom headers that you can configure CloudFront to add to origin requests For more information, see [Adding Custom Headers to Origin Requests](add-origin-custom-headers.md)\.  | 10 name/value pairs [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Custom headers: maximum length of a header name | 256 characters | 
| Custom headers: maximum length of a header value | 1,783 characters | 
| Custom headers: maximum length of all header values and names combined | 10,240 characters | 

## Quotas on SSL Certificates \(Web Distributions Only\)<a name="limits-ssl-certificates"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| SSL certificates per AWS account when serving HTTPS requests using dedicated IP addresses \(no quota when serving HTTPS requests using SNI\) For more information, see [Using HTTPS with CloudFront](using-https.md)\.  | 2 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| SSL certificates that can be associated with a CloudFront web distribution | 1 | 

## Quotas on Invalidations<a name="limits-invalidations"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| File invalidation: maximum number of files allowed in active invalidation requests, excluding wildcard invalidations For more information, see [Invalidating Files](Invalidation.md)\.  | 3,000 | 
| File invalidation: maximum number of active wildcard invalidations allowed | 15 | 
| File invalidation: maximum number of files that one wildcard invalidation can process | No quota | 

## Quotas on Field\-Level Encryption<a name="limits-field-level-encryption"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Maximum length of a field to encrypt For more information, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\.  | 16 KB | 
| Maximum number of fields in a request body when field\-level encryption is configured | 10 | 
| Maximum length of a request body when field\-level encryption is configured | 1 MB | 
| Maximum number of field\-level encryption configurations that can be associated with one AWS account | 10 | 
| Maximum number of field\-level encryption profiles that can be associated with one AWS account | 10 | 
| Maximum number of public keys that can be added to one AWS account | 10 | 
| Maximum number of fields to encrypt that can be specified in one profile | 10 | 
| Maximum number of CloudFront distributions that can be associated with a field\-level encryption configuration | 20 | 
| Maximum number of query argument profile mappings that can be included in a field\-level encryption configuration | 5 | 

## Quotas on Lambda@Edge<a name="limits-lambda-at-edge"></a>

The quotas in this section apply to Lambda@Edge\. These quotas are in addition to the default CloudFront and Lambda quotas, which also apply\. For the default quotas, see [Quotas](#cloudfront-limits) in this guide and [Quotas](https://docs.aws.amazon.com/lambda/latest/dg/limits.html) in the *AWS Lambda Developer Guide*\.

**Note**  
Lambda dynamically scales capacity in response to increased traffic, within your account’s quotas\. For more information, see [Function Scaling](https://docs.aws.amazon.com/lambda/latest/dg/scaling.html) in the *AWS Lambda Developer Guide*\.

In addition, be aware that there are some other restrictions when using Lambda@Edge functions\. For more information, see [Requirements and Restrictions on Lambda Functions](lambda-requirements-limits.md)\.


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

### Size Quotas on URI and Query String<a name="limits-URI-size-limits-lambda-at-edge"></a>

When accessing or updating a URI or query string in a Lambda@Edge function, the total length of the URI including the query string must be less than 8,192 characters\.

### Size Quotas on Request Body with the Include Body Option<a name="limits-body-size-limits-lambda-at-edge"></a>

When you choose the **Include Body** option to expose the request body to your Lambda@Edge function, the following size quotas apply to the portions of the body that are exposed or replaced\.

**Note**  
The body is always base64 encoded by Lambda@Edge before it is exposed\.

#### Size Quotas when Exposing the Body to a Lambda Function<a name="limits-exposing-body-size-limits-lambda-at-edge"></a>

If the request body is large, Lambda@Edge truncates it before exposing it, as follows:
+ For viewer requests, the body is truncated at 40 KB\.
+ For origin requests, the body is truncated at 1 MB\.

#### Size Quotas when Returning a Request Body from a Lambda Function<a name="limits-returning-body-size-limits-lambda-at-edge"></a>

If you access the request body as read\-only, the full original request body is sent to the origin\. However, if you choose to replace the request body, the following body size quotas apply when it’s returned from a Lambda function:


****  

| Type of body encoding | Allowed body size: Viewer request | Allowed body size: Origin request | 
| --- | --- | --- | 
|  text  |  40 KB  |  1 MB  | 
|  base64  |  53\.2 KB  |  1\.33 MB  | 

## Request Timeout<a name="limits-request-timeout"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Request timeout For more information, see [Origin Response Timeout](RequestAndResponseBehaviorCustomOrigin.md#request-custom-request-timeout)\.  |  30 seconds [ Request a higher quota](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/RequestAndResponseBehaviorCustomOrigin.html#request-custom-request-timeout)  | 

## Quotas on RTMP Distributions<a name="limits-rtmp-distributions"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| RTMP distributions per AWS account For more information, see [Working with RTMP Distributions](distribution-rtmp.md)\.  | 100 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 