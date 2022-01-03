# Quotas<a name="cloudfront-limits"></a>

CloudFront is subject to the following quotas \(formerly referred to as limits\)\.

**Topics**
+ [General quotas](#limits-general)
+ [General quotas on distributions](#limits-web-distributions)
+ [General quotas on policies](#limits-policies)
+ [Quotas on CloudFront Functions](#limits-functions)
+ [Quotas on Lambda@Edge](#limits-lambda-at-edge)
+ [Quotas on SSL certificates](#limits-ssl-certificates)
+ [Quotas on invalidations](#limits-invalidations)
+ [Quotas on key groups](#limits-key-groups)
+ [Quotas on WebSocket connections](#limits-websockets)
+ [Quotas on field\-level encryption](#limits-field-level-encryption)
+ [Quotas on cookies \(legacy cache settings\)](#limits-whitelisted-cookies)
+ [Quotas on query strings \(legacy cache settings\)](#limits-whitelisted-query-strings)
+ [Quotas on headers](#limits-custom-headers)

## General quotas<a name="limits-general"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Data transfer rate per distribution | 150 Gbps [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Requests per second per distribution | 250,000 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Tags that can be added to a distribution | 50 | 
| Files that you can serve per distribution | No quota | 
| Maximum length of a request, including headers and query strings, but not including the body content | 20,480 bytes | 
| Maximum length of a URL | 8,192 bytes | 

## General quotas on distributions<a name="limits-web-distributions"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Distributions per AWS account For more information, see [Creating a distribution](distribution-web-creating-console.md)\.  | 200 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Maximum cacheable file size for HTTP GET, POST, and PUT requests | 30 GB | 
|  Request timeout For more information, see [Origin response timeout](RequestAndResponseBehaviorCustomOrigin.md#request-custom-request-timeout)\.  |  30 seconds [ Request a higher quota](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/RequestAndResponseBehaviorCustomOrigin.html#request-custom-request-timeout)  | 
| Response timeout per origin For more information, see [Origin response timeout](distribution-web-values-specify.md#DownloadDistValuesOriginResponseTimeout)\.  | 1\-60 seconds [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Connection timeout per origin For more information, see [Origin connection timeout](distribution-web-values-specify.md#origin-connection-timeout)\.  |  1\-10 seconds  | 
|  Connection attempts per origin For more information, see [Origin connection attempts](distribution-web-values-specify.md#origin-connection-attempts)\.  |  1\-3  | 
| File compression: range of file sizes that CloudFront compresses For more information, see [Serving compressed files](ServingCompressedFiles.md)\.  | 1,000 to 10,000,000 bytes | 
| Alternate domain names \(CNAMEs\) per distribution For more information, see [Using custom URLs by adding alternate domain names \(CNAMEs\)](CNAMEs.md)\.  | 100 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Origins per distribution | 25 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Origin groups per distribution | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Origin access identities per account  |  100 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Cache behaviors per distribution | 25 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 

## General quotas on policies<a name="limits-policies"></a>


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
|  Response headers policies per AWS account  |  20  | 
|  Distributions associated with the same response headers policy  |  100  | 
|  Custom headers per response headers policy  |  10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 

## Quotas on CloudFront Functions<a name="limits-functions"></a>


****  

|  Entity  |  Default quota  | 
| --- | --- | 
|  Functions per AWS account  |  100  | 
|  Maximum function size  |  10 KB  | 
|  Maximum function memory  |  2 MB  | 
|  Distributions associated with the same function  |  100  | 

In addition to these quotas, there are some other restrictions when using CloudFront Functions\. For more information, see [Restrictions on CloudFront Functions](edge-functions-restrictions.md#cloudfront-function-restrictions)\.

## Quotas on Lambda@Edge<a name="limits-lambda-at-edge"></a>

The quotas in this section apply to Lambda@Edge\. These quotas are in addition to the default AWS Lambda quotas, which also apply\. For the Lambda quotas, see [Quotas](https://docs.aws.amazon.com/lambda/latest/dg/limits.html) in the *AWS Lambda Developer Guide*\.

**Note**  
Lambda dynamically scales capacity in response to increased traffic, within your account’s quotas\. For more information, see [Function scaling](https://docs.aws.amazon.com/lambda/latest/dg/scaling.html) in the *AWS Lambda Developer Guide*\.


**General quotas**  

|  Entity  |  Default quota  | 
| --- | --- | 
|  Distributions per AWS account that can have Lambda@Edge functions  |  25 [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
|  Lambda@Edge functions per distribution  |  100 [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
|  Requests per second  |  10,000 \(in each AWS Region\) [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
|  Concurrent executions For more information, see [Function scaling](https://docs.aws.amazon.com/lambda/latest/dg/scaling.html) in the *AWS Lambda Developer Guide*\.  |  1,000 \(in each AWS Region\) [ Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-aws-lambda-edge)  | 
|  Distributions associated with the same function  |  100  | 


**Quotas that differ by event type**  

|  Entity  |  Viewer request and viewer response events  |  Origin request and origin response events  | 
| --- | --- | --- | 
|  Function memory size  |  128 MB  |  Same as [Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/limits.html)  | 
|  Function timeout\. The function can make network calls to resources such as Amazon S3 buckets, DynamoDB tables, or Amazon EC2 instances in AWS Regions\.  |  5 seconds  |  30 seconds  | 
|  Size of a response that is generated by a Lambda function, including headers and body  |  40 KB  |  1 MB  | 
|  Maximum compressed size of a Lambda function and any included libraries  |  1 MB  |  50 MB  | 

In addition to these quotas, there are some other restrictions when using Lambda@Edge functions\. For more information, see [Restrictions on Lambda@Edge](edge-functions-restrictions.md#lambda-at-edge-function-restrictions)\.

## Quotas on SSL certificates<a name="limits-ssl-certificates"></a>


****  

| Entity | Default quota | 
| --- | --- | 
|  SSL certificates per AWS account when serving HTTPS requests using dedicated IP addresses \(no quota when serving HTTPS requests using SNI\) For more information, see [Using HTTPS with CloudFront](using-https.md)\.  |  2 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  SSL certificates that can be associated with a CloudFront distribution  |  1  | 

## Quotas on invalidations<a name="limits-invalidations"></a>


****  

| Entity | Default quota | 
| --- | --- | 
|  File invalidation: maximum number of files allowed in active invalidation requests, excluding wildcard invalidations For more information, see [Invalidating files](Invalidation.md)\.  |  3,000  | 
|  File invalidation: maximum number of active wildcard invalidations allowed  |  15  | 
|  File invalidation: maximum number of files that one wildcard invalidation can process  |  No quota  | 

## Quotas on key groups<a name="limits-key-groups"></a>


| Entity | Default quota | 
| --- | --- | 
|  Public keys in a single key group  |  5 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
|  Key groups associated with a single cache behavior  | 4[Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) | 
|  Key groups per AWS account  | 10[Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) | 
|  Distributions associated with a single key group  | 100[Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) | 

## Quotas on WebSocket connections<a name="limits-websockets"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Origin response timeout \(idle timeout\) | 10 minutes If CloudFront hasn’t detected any bytes sent from the origin to the client within the past 10 minutes, the connection is assumed to be idle and is closed\. | 

## Quotas on field\-level encryption<a name="limits-field-level-encryption"></a>


****  

| Entity | Default quota | 
| --- | --- | 
|  Maximum length of a field to encrypt For more information, see [Using field\-level encryption to help protect sensitive data](field-level-encryption.md)\.  |  16 KB  | 
|  Maximum number of fields in a request body when field\-level encryption is configured  |  10  | 
|  Maximum length of a request body when field\-level encryption is configured  |  1 MB  | 
|  Maximum number of field\-level encryption configurations that can be associated with one AWS account  |  10  | 
|  Maximum number of field\-level encryption profiles that can be associated with one AWS account  |  10  | 
|  Maximum number of public keys that can be added to one AWS account  |  10  | 
|  Maximum number of fields to encrypt that can be specified in one profile  |  10  | 
|  Maximum number of CloudFront distributions that can be associated with a field\-level encryption configuration  |  20  | 
|  Maximum number of query argument profile mappings that can be included in a field\-level encryption configuration  |  5  | 

## Quotas on cookies \(legacy cache settings\)<a name="limits-whitelisted-cookies"></a>

These quotas apply to CloudFront’s legacy cache settings\. We recommend using a [cache policy or origin request policy](working-with-policies.md) instead of the legacy settings\.


****  

| Entity | Default quota | 
| --- | --- | 
| Cookies per cache behavior For more information, see [Caching content based on cookies](Cookies.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Total number of bytes in cookie names \(doesn’t apply if you configure CloudFront to forward all cookies to the origin\) | 512 minus the number of cookies | 

## Quotas on query strings \(legacy cache settings\)<a name="limits-whitelisted-query-strings"></a>

These quotas apply to CloudFront’s legacy cache settings\. We recommend using a [cache policy or origin request policy](working-with-policies.md) instead of the legacy settings\.


****  

| Entity | Default quota | 
| --- | --- | 
| Maximum number of characters in a query string  | 128 characters  | 
| Maximum number of characters total for all query strings in the same parameter  | 512 characters  | 
| Query strings per cache behavior For more information, see [Caching content based on query string parameters](QueryStringParameters.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 

## Quotas on headers<a name="limits-custom-headers"></a>


****  

| Entity | Default quota | 
| --- | --- | 
| Headers per cache behavior \(legacy cache settings\) For more information, see [Caching content based on request headers](header-caching.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Custom headers: maximum number of custom headers that you can configure CloudFront to add to origin requests For more information, see [Adding custom headers to origin requests](add-origin-custom-headers.md)\.  | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions)  | 
| Custom headers: maximum number of custom headers that you can add to a response headers policy | 10 [Request a higher quota](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) | 
| Custom headers: maximum length of a header name | 256 characters | 
| Custom headers: maximum length of a header value | 1,783 characters | 
| Custom headers: maximum length of all header values and names combined | 10,240 characters | 