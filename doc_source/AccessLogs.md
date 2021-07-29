# Configuring and using standard logs \(access logs\)<a name="AccessLogs"></a>

You can configure CloudFront to create log files that contain detailed information about every user request that CloudFront receives\. These are called *standard logs*, also known as *access logs*\. If you enable standard logs, you can also specify the Amazon S3 bucket that you want CloudFront to save files in\.

You can enable standard logs when you create or update a distribution\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

CloudFront also offers real\-time logs, which give you information about requests made to a distribution in real time \(logs are delivered within seconds of receiving the requests\)\. You can use real\-time logs to monitor, analyze, and take action based on content delivery performance\. For more information, see [Real\-time logs](real-time-logs.md)\.

**Topics**
+ [How standard logging works](#AccessLogsOverview)
+ [Choosing an Amazon S3 bucket for your standard logs](#access-logs-choosing-s3-bucket)
+ [Permissions required to configure standard logging and to access your log files](#AccessLogsBucketAndFileOwnership)
+ [Required CMK key policy for SSE\-KMS buckets](#AccessLogsKMSPermissions)
+ [File name format](#AccessLogsFileNaming)
+ [Timing of standard log file delivery](#access-logs-timing)
+ [How requests are logged when the request URL or headers exceed the maximum size](#access-logs-request-URL-size)
+ [Analyzing standard logs](#access-logs-analyzing)
+ [Editing your standard logging settings](#ChangeSettings)
+ [Deleting standard log files from an Amazon S3 bucket](#DeletingLogFiles)
+ [Standard log file format](#LogFileFormat)
+ [Charges for standard logs](#AccessLogsCharges)

## How standard logging works<a name="AccessLogsOverview"></a>

The following diagram shows how CloudFront logs information about requests for your objects\.

![\[Basic flow for access logs\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/Logging.png)

The following explains how CloudFront logs information about requests for your objects, as illustrated in the previous diagram\.

1. In this diagram, you have two websites, A and B, and two corresponding CloudFront distributions\. Users request your objects using URLs that are associated with your distributions\.

1. CloudFront routes each request to the appropriate edge location\.

1. CloudFront writes data about each request to a log file specific to that distribution\. In this example, information about requests related to Distribution A goes into a log file just for Distribution A, and information about requests related to Distribution B goes into a log file just for Distribution B\.

1. CloudFront periodically saves the log file for a distribution in the Amazon S3 bucket that you specified when you enabled logging\. CloudFront then starts saving information about subsequent requests in a new log file for the distribution\.

If no users access your content during a given hour, you don't receive any log files for that hour\.

Each entry in a log file gives details about a single request\. For more information about log file format, see [Standard log file format](#LogFileFormat)\.

**Important**  
We recommend that you use the logs to understand the nature of the requests for your content, not as a complete accounting of all requests\. CloudFront delivers access logs on a best\-effort basis\. The log entry for a particular request might be delivered long after the request was actually processed and, in rare cases, a log entry might not be delivered at all\. When a log entry is omitted from access logs, the number of entries in the access logs won't match the usage that appears in the AWS usage and billing reports\.

## Choosing an Amazon S3 bucket for your standard logs<a name="access-logs-choosing-s3-bucket"></a>

When you enable logging for a distribution, you specify the Amazon S3 bucket that you want CloudFront to store log files in\. If you’re using Amazon S3 as your origin, we recommend that you don’t use the same bucket for your log files; using a separate bucket simplifies maintenance\.

**Note**  
Don’t choose an Amazon S3 bucket in any of the following Regions, because CloudFront doesn’t deliver access logs to buckets in these Regions:  
Africa \(Cape Town\)  af\-south\-1
Asia Pacific \(Hong Kong\)  ap\-east\-1
Europe \(Milan\)  eu\-south\-1
Middle East \(Bahrain\)  me\-south\-1
The [Amazon S3 console](https://console.aws.amazon.com/s3/home) shows the bucket’s Region\.

You can store the log files for multiple distributions in the same bucket\. When you enable logging, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions\.

## Permissions required to configure standard logging and to access your log files<a name="AccessLogsBucketAndFileOwnership"></a>

Your AWS account must have the following permissions for the bucket that you specify for log files:
+ The S3 access control list \(ACL\) for the bucket must grant you `FULL_CONTROL`\. If you're the bucket owner, your account has this permission by default\. If you're not, the bucket owner must update the ACL for the bucket\.
+ `s3:GetBucketAcl`
+ `s3:PutBucketAcl`

Note the following:

**ACL for the bucket**  
When you create or update a distribution and enable logging, CloudFront uses these permissions to update the ACL for the bucket to give the `awslogsdelivery` account `FULL_CONTROL` permission\. The `awslogsdelivery` account writes log files to the bucket\. If your account doesn't have the required permissions to update the ACL, creating or updating the distribution will fail\.  
In some circumstances, if you programmatically submit a request to create a bucket but a bucket with the specified name already exists, S3 resets permissions on the bucket to the default value\. If you configured CloudFront to save access logs in an S3 bucket and you stop getting logs in that bucket, check permissions on the bucket to ensure that CloudFront has the necessary permissions\.

**Restoring the ACL for the bucket**  
If you remove permissions for the `awslogsdelivery` account, CloudFront won't be able to save logs to the S3 bucket\. To enable CloudFront to start saving logs for your distribution again, restore the ACL permission by doing one of the following:  
+ Disable logging for your distribution in CloudFront, and then enable it again\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.
+ Add the ACL permission for `awslogsdelivery` manually by navigating to the S3 bucket in the Amazon S3 console and adding permission\. To add the ACL for `awslogsdelivery`, you must provide the canonical ID for the account, which is the following:

  `c4c1ede66af53448b93c283ce9448c4ba468c9432aa01d700d3878632f77d2d0`

  

  For more information about adding ACLs to S3 buckets, see [How Do I Set ACL Bucket Permissions?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-bucket-permissions.html) in the *Amazon Simple Storage Service Console User Guide*\.

**ACL for each log file**  
In addition to the ACL on the bucket, there's an ACL on each log file\. The bucket owner has `FULL_CONTROL` permission on each log file, the distribution owner \(if different from the bucket owner\) has no permission, and the `awslogsdelivery` account has read and write permissions\. 

**Disabling logging**  
If you disable logging, CloudFront doesn't delete the ACLs for either the bucket or the log files\. If you want, you can do that yourself\.

## Required CMK key policy for SSE\-KMS buckets<a name="AccessLogsKMSPermissions"></a>

If the S3 bucket for your standard logs uses server\-side encryption with AWS KMS\-managed keys \(SSE\-KMS\) using a customer\-managed Customer Master Key \(CMK\), you must add the following statement to the key policy for your CMK\. This allows CloudFront to write log files to the bucket\. \(You can’t use SSE\-KMS with the default CMK because CloudFront won’t be able to write log files to the bucket\.\)

```
{
    "Sid": "Allow CloudFront to use the key to deliver logs",
    "Effect": "Allow",
    "Principal": {
        "Service": "delivery.logs.amazonaws.com"
    },
    "Action": "kms:GenerateDataKey*",
    "Resource": "*"
}
```

If the S3 bucket for your standard logs uses SSE\-KMS with an [S3 Bucket Key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html), you also need to add the `kms:Decrypt` permission to policy statement\. In that case, the full policy statement looks like the following\.

```
{
    "Sid": "Allow CloudFront to use the key to deliver logs",
    "Effect": "Allow",
    "Principal": {
        "Service": "delivery.logs.amazonaws.com"
    },
    "Action": [
        "kms:GenerateDataKey*",
        "kms:Decrypt"
    ],
    "Resource": "*"
}
```

## File name format<a name="AccessLogsFileNaming"></a>

The name of each log file that CloudFront saves in your Amazon S3 bucket uses the following file name format:

`<optional prefix>/<distribution ID>.YYYY-MM-DD-HH.unique-ID.gz`

The date and time are in Coordinated Universal Time \(UTC\)\.

For example, if you use `example-prefix` as the prefix, and your distribution ID is `EMLARXS9EXAMPLE`, your file names look similar to this:

`example-prefix/EMLARXS9EXAMPLE.2019-11-14-20.RT4KCN4SGK9.gz`

When you enable logging for a distribution, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions\. If you include a value for the log file prefix and your prefix doesn't end with a forward slash \(`/`\), CloudFront appends one automatically\. If your prefix does end with a forward slash, CloudFront doesn't add another one\.

The `.gz` at the end of the file name indicates that CloudFront has compressed the log file using gzip\.

## Timing of standard log file delivery<a name="access-logs-timing"></a>

CloudFront delivers standard logs for a distribution up to several times an hour\. In general, a log file contains information about the requests that CloudFront received during a given time period\. CloudFront usually delivers the log file for that time period to your Amazon S3 bucket within an hour of the events that appear in the log\. Note, however, that some or all log file entries for a time period can sometimes be delayed by up to 24 hours\. When log entries are delayed, CloudFront saves them in a log file for which the file name includes the date and time of the period in which the requests occurred, not the date and time when the file was delivered\.

When creating a log file, CloudFront consolidates information for your distribution from all of the edge locations that received requests for your objects during the time period that the log file covers\.

CloudFront can save more than one file for a time period depending on how many requests CloudFront receives for the objects associated with a distribution\.

CloudFront begins to reliably deliver access logs about four hours after you enable logging\. You might get a few access logs before that time\.

**Note**  
If no users request your objects during the time period, you don't receive any log files for that period\.

CloudFront also offers real\-time logs, which give you information about requests made to a distribution in real time \(logs are delivered within seconds of receiving the requests\)\. You can use real\-time logs to monitor, analyze, and take action based on content delivery performance\. For more information, see [Real\-time logs](real-time-logs.md)\.

## How requests are logged when the request URL or headers exceed the maximum size<a name="access-logs-request-URL-size"></a>

If the total size of all request headers, including cookies, exceeds 20 KB, or if the URL exceeds 8192 bytes, CloudFront can't parse the request completely and can't log the request\. Because the request isn't logged, you won't see in the log files the HTTP error status code returned\.

If the request body exceeds the maximum size, the request is logged, including the HTTP error status code\.

## Analyzing standard logs<a name="access-logs-analyzing"></a>

Because you can receive multiple access logs per hour, we recommend that you combine all the log files you receive for a given time period into one file\. You can then analyze the data for that period more accurately and completely\.

One way to analyze your access logs is to use [Amazon Athena](http://aws.amazon.com/athena/)\. Athena is an interactive query service that can help you analyze data for AWS services, including CloudFront\. To learn more, see [ Querying Amazon CloudFront Logs](https://docs.aws.amazon.com/athena/latest/ug/cloudfront-logs.html) in the *Amazon Athena User Guide*\.

In addition, the following AWS blog posts discuss some ways to analyze access logs\.
+ [ Amazon CloudFront Request Logging](http://aws.amazon.com/blogs/aws/amazon-cloudfront-request-logging/) \(for content delivered via HTTP\)
+ [ Enhanced CloudFront Logs, Now With Query Strings](http://aws.amazon.com/blogs/aws/enhanced-cloudfront-logs-now-with-query-strings/)

**Important**  
We recommend that you use the logs to understand the nature of the requests for your content, not as a complete accounting of all requests\. CloudFront delivers access logs on a best\-effort basis\. The log entry for a particular request might be delivered long after the request was actually processed and, in rare cases, a log entry might not be delivered at all\. When a log entry is omitted from access logs, the number of entries in the access logs won't match the usage that appears in the AWS usage and billing reports\.

## Editing your standard logging settings<a name="ChangeSettings"></a>

You can enable or disable logging, change the Amazon S3 bucket where your logs are stored, and change the prefix for log files by using the [CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home) or the CloudFront API\. Your changes to logging settings take effect within 12 hours\.

For more information, see the following topics:
+ To update a distribution using the CloudFront console, see [Updating a Distribution](HowToUpdateDistribution.md)\.
+ To update a distribution using the CloudFront API, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.

## Deleting standard log files from an Amazon S3 bucket<a name="DeletingLogFiles"></a>

CloudFront does not automatically delete log files from your Amazon S3 bucket\. For information about deleting log files from an Amazon S3 bucket, see the following topics:
+ Using the Amazon S3 console: [Deleting Objects](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-objects.html) in the *Amazon Simple Storage Service Console User Guide*\.
+ Using the REST API: [DeleteObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html) in the *Amazon Simple Storage Service API Reference*\.

## Standard log file format<a name="LogFileFormat"></a>

Each entry in a log file gives details about a single viewer request\. The log files have the following characteristics:
+ Use the [W3C extended log file format](https://www.w3.org/TR/WD-logfile.html)\.
+ Contain tab\-separated values\.
+ Contain records that are not necessarily in chronological order\.
+ Contain two header lines: one with the file format version, and another that lists the W3C fields included in each record\.
+ Contain URL\-encoded equivalents for spaces and certain other characters in field values\.

  URL\-encoded equivalents are used for the following characters:
  + ASCII character codes 0 through 32, inclusive
  + ASCII character codes 127 and higher
  + All characters in the following table

  The URL encoding standard is defined in [RFC 1738](https://tools.ietf.org/html/rfc1738.html)\.


| URL\-Encoded Value | Character | 
| --- | --- | 
| %3C | < | 
| %3E | > | 
| %22 | " | 
| %23 | \# | 
| %25 | % | 
| %7B | \{ | 
| %7D | \} | 
| %7C | \| | 
| %5C | \\ | 
| %5E | ^ | 
| %7E | \~ | 
| %5B | \[ | 
| %5D | \] | 
| %60 | ` | 
| %27 | ' | 
| %20 | space | 

### Standard log file fields<a name="BasicDistributionFileFormat"></a>

The log file for a distribution contains 33 fields\. The following list contains each field name, in order, along with a description of the information in that field\.

1. **`date`**

   The date on which the event occurred in the format `YYYY-MM-DD`\. For example, `2019-06-30`\. The date and time are in Coordinated Universal Time \(UTC\)\. For WebSocket connections, this is the date when the connection closed\.

1. **`time`**

   The time when the CloudFront server finished responding to the request \(in UTC\), for example, `01:42:39`\. For WebSocket connections, this is the time when the connection is closed\.

1. **`x-edge-location`**

   The edge location that served the request\. Each edge location is identified by a three\-letter code and an arbitrarily assigned number \(for example, DFW3\)\. The three\-letter code typically corresponds with the International Air Transport Association \(IATA\) airport code for an airport near the edge location’s geographic location\. \(These abbreviations might change in the future\.\)

1. **`sc-bytes`**

   The total number of bytes that the server sent to the viewer in response to the request, including headers\. For WebSocket connections, this is the total number of bytes sent from the server to the client through the connection\.

1. **`c-ip`**

   The IP address of the viewer that made the request, for example, `192.0.2.183` or `2001:0db8:85a3:0000:0000:8a2e:0370:7334`\. If the viewer used an HTTP proxy or a load balancer to send the request, the value of this field is the IP address of the proxy or load balancer\. See also the `x-forwarded-for` field\.

1. **`cs-method`**

   The HTTP request method received from the viewer\.

1. **`cs(Host)`**

   The domain name of the CloudFront distribution \(for example, d111111abcdef8\.cloudfront\.net\)\.

1. **`cs-uri-stem`**

   The portion of the request URL that identifies the path and object \(for example, `/images/cat.jpg`\)\. Question marks \(?\) in URLs and query strings are not included in the log\.

1. **`sc-status`**

   Contains one of the following values:
   + The HTTP status code of the server’s response \(for example, `200`\)\.
   + `000`, which indicates that the viewer closed the connection before the server could respond to the request\. If the viewer closes the connection after the server starts to send the response, this field contains the HTTP status code of the response that the server started to send\.

1. **`cs(Referer)`**

   The value of the `Referer` header in the request\. This is the name of the domain that originated the request\. Common referrers include search engines, other websites that link directly to your objects, and your own website\.

1. **`cs(User-Agent)`**

   The value of the `User-Agent` header in the request\. The `User-Agent` header identifies the source of the request, such as the type of device and browser that submitted the request or, if the request came from a search engine, which search engine\.

1. **`cs-uri-query`**

   The query string portion of the request URL, if any\.

   When a URL doesn’t contain a query string, this field's value is a hyphen \(\-\)\. For more information, see [Caching content based on query string parameters](QueryStringParameters.md)\.

1. **`cs(Cookie)`**

   The `Cookie` header in the request, including name—value pairs and the associated attributes\.

   If you enable cookie logging, CloudFront logs the cookies in all requests regardless of which cookies you choose to forward to the origin\. When a request doesn’t include a cookie header, this field’s value is a hyphen \(\-\)\. For more information about cookies, see [Caching content based on cookies](Cookies.md)\.

1. **`x-edge-result-type`**

   How the server classified the response after the last byte left the server\. In some cases, the result type can change between the time that the server is ready to send the response and the time that it finishes sending the response\. See also the `x-edge-response-result-type` field\.

   For example, in HTTP streaming, suppose the server finds a segment of the stream in the cache\. In that scenario, the value of this field would ordinarily be `Hit`\. However, if the viewer closes the connection before the server has delivered the entire segment, the final result type \(and the value of this field\) is `Error`\.

   WebSocket connections will have a value of `Miss` for this field because the content is not cacheable and is proxied directly to the origin\.

   Possible values include:
   + `Hit` – The server served the object to the viewer from the cache\.
   + `RefreshHit` – The server found the object in the cache but the object had expired, so the server contacted the origin to verify that the cache had the latest version of the object\.
   + `OriginShieldHit` – The object was served to the viewer from the Origin Shield cache\.
   + `Miss` – The request could not be satisfied by an object in the cache, so the server forwarded the request to the origin and returned the result to the viewer\.
   + `LimitExceeded` – The request was denied because a CloudFront quota \(formerly referred to as a limit\) was exceeded\.
   + `CapacityExceeded` – The server returned an HTTP 503 status code because it didn’t have enough capacity at the time of the request to serve the object\.
   + `Error` – Typically, this means the request resulted in a client error \(the value of the `sc-status` field is in the `4xx` range\) or a server error \(the value of the `sc-status` field is in the `5xx` range\)\. If the value of the `sc-status` field is `200`, or if the value of this field is `Error` and the value of the `x-edge-response-result-type` field is not `Error`, it means the HTTP request was successful but the client disconnected before receiving all of the bytes\.
   + `Redirect` – The server redirected the viewer from HTTP to HTTPS according to the distribution settings\.

1. **`x-edge-request-id`**

   An opaque string that uniquely identifies a request\. CloudFront also sends this string in the `x-amz-cf-id` response header\.

1. **`x-host-header`**

   The value that the viewer included in the `Host` header of the request\. If you’re using the CloudFront domain name in your object URLs \(such as d111111abcdef8\.cloudfront\.net\), this field contains that domain name\. If you’re using alternate domain names \(CNAMEs\) in your object URLs \(such as www\.example\.com\), this field contains the alternate domain name\.

   If you’re using alternate domain names, see `cs(Host)` in field 7 for the domain name that is associated with your distribution\.

1. **`cs-protocol`**

   The protocol of the viewer request \(`http`, `https`, `ws`, or `wss`\)\.

1. **`cs-bytes`**

   The total number of bytes of data that the viewer included in the request, including headers\. For WebSocket connections, this is the total number of bytes sent from the client to the server on the connection\.

1. **`time-taken`**

   The number of seconds \(to the thousandth of a second, for example, 0\.082\) from when the server receives the viewer’s request to when the server writes the last byte of the response to the output queue, as measured on the server\. From the perspective of the viewer, the total time to get the full response will be longer than this value because of network latency and TCP buffering\.

1. **`x-forwarded-for`**

   If the viewer used an HTTP proxy or a load balancer to send the request, the value of the `c-ip` field is the IP address of the proxy or load balancer\. In that case, this field is the IP address of the viewer that originated the request\. This field contains an IPv4 address \(for example, `192.0.2.183`\) or an IPv6 address \(for example, `2001:0db8:85a3:0000:0000:8a2e:0370:7334`\)\.

   If the viewer did not use an HTTP proxy or a load balancer, the value of this field is a hyphen \(\-\)\.

1. **`ssl-protocol`**

   When the request used HTTPS, this field contains the SSL/TLS protocol that the viewer and server negotiated for transmitting the request and response\. For a list of possible values, see the supported SSL/TLS protocols in [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)\.

   When `cs-protocol` in field 17 is `http`, the value for this field is a hyphen \(\-\)\.

1. **`ssl-cipher`**

   When the request used HTTPS, this field contains the SSL/TLS cipher that the viewer and server negotiated for encrypting the request and response\. For a list of possible values, see the supported SSL/TLS ciphers in [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)\.

   When `cs-protocol` in field 17 is `http`, the value for this field is a hyphen \(\-\)\.

1. **`x-edge-response-result-type`**

   How the server classified the response just before returning the response to the viewer\. See also the `x-edge-result-type` field\. Possible values include:
   + `Hit` – The server served the object to the viewer from the cache\.
   + `RefreshHit` – The server found the object in the cache but the object had expired, so the server contacted the origin to verify that the cache had the latest version of the object\.
   + `Miss` – The request could not be satisfied by an object in the cache, so the server forwarded the request to the origin server and returned the result to the viewer\.
   + `LimitExceeded` – The request was denied because a CloudFront quota \(formerly referred to as a limit\) was exceeded\.
   + `CapacityExceeded` – The server returned a 503 error because it didn’t have enough capacity at the time of the request to serve the object\.
   + `Error` – Typically, this means the request resulted in a client error \(the value of the `sc-status` field is in the `4xx` range\) or a server error \(the value of the `sc-status` field is in the `5xx` range\)\.

     If the value of the `x-edge-result-type` field is `Error` and the value of this field is not `Error`, the client disconnected before finishing the download\.
   + `Redirect` – The server redirected the viewer from HTTP to HTTPS according to the distribution settings\.

1. **`cs-protocol-version`**

   The HTTP version that the viewer specified in the request\. Possible values include `HTTP/0.9`, `HTTP/1.0`, `HTTP/1.1`, and `HTTP/2.0`\.

1. **`fle-status`**

   When [field\-level encryption](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field-level-encryption.html) is configured for a distribution, this field contains a code that indicates whether the request body was successfully processed\. When the server successfully processes the request body, encrypts values in the specified fields, and forwards the request to the origin, the value of this field is `Processed`\. The value of `x-edge-result-type` can still indicate a client\-side or server\-side error in this case\.

   Possible values for this field include:
   + `ForwardedByContentType` – The server forwarded the request to the origin without parsing or encryption because no content type was configured\.
   + `ForwardedByQueryArgs` – The server forwarded the request to the origin without parsing or encryption because the request contains a query argument that wasn’t in the configuration for field\-level encryption\.
   + `ForwardedDueToNoProfile` – The server forwarded the request to the origin without parsing or encryption because no profile was specified in the configuration for field\-level encryption\.
   + `MalformedContentTypeClientError` – The server rejected the request and returned an HTTP 400 status code to the viewer because the value of the `Content-Type` header was in an invalid format\.
   + `MalformedInputClientError` – The server rejected the request and returned an HTTP 400 status code to the viewer because the request body was in an invalid format\.
   + `MalformedQueryArgsClientError` – The server rejected the request and returned an HTTP 400 status code to the viewer because a query argument was empty or in an invalid format\.
   + `RejectedByContentType` – The server rejected the request and returned an HTTP 400 status code to the viewer because no content type was specified in the configuration for field\-level encryption\.
   + `RejectedByQueryArgs` – The server rejected the request and returned an HTTP 400 status code to the viewer because no query argument was specified in the configuration for field\-level encryption\.
   + `ServerError` – The origin server returned an error\.

   If the request exceeds a field\-level encryption quota \(formerly referred to as a limit\), this field contains one of the following error codes, and the server returns HTTP status code 400 to the viewer\. For a list of the current quotas on field\-level encryption, see [Quotas on field\-level encryption](cloudfront-limits.md#limits-field-level-encryption)\.
   + `FieldLengthLimitClientError` – A field that is configured to be encrypted exceeded the maximum length allowed\.
   + `FieldNumberLimitClientError` – A request that the distribution is configured to encrypt contains more than the number of fields allowed\.
   + `RequestLengthLimitClientError` – The length of the request body exceeded the maximum length allowed when field\-level encryption is configured\.

   If field\-level encryption is not configured for the distribution, the value of this field is a hyphen \(\-\)\.

1. **`fle-encrypted-fields`**

   The number of [field\-level encryption](field-level-encryption.md) fields that the server encrypted and forwarded to the origin\. CloudFront servers stream the processed request to the origin as they encrypt data, so this field can have a value even if the value of `fle-status` is an error\.

   If field\-level encryption is not configured for the distribution, the value of this field is a hyphen \(\-\)\.

1. **`c-port`**

   The port number of the request from the viewer\.

1. **`time-to-first-byte`**

   The number of seconds between receiving the request and writing the first byte of the response, as measured on the server\.

1. **`x-edge-detailed-result-type`**

   When the `x-edge-result-type` field is not `Error`, this field contains the same value as `x-edge-result-type`\. When the `x-edge-result-type` field is `Error`, this field contains the specific type of error\. Possible values include:
   + `AbortedOrigin` – The server encountered an issue with the origin\.
   + `ClientCommError` – The response to the viewer was interrupted due to a communication problem between the server and the viewer\.
   + `ClientGeoBlocked` – The distribution is configured to refuse requests from the viewer’s geographic location\.
   + `ClientHungUpRequest` – The viewer stopped prematurely while sending the request\.
   + `Error` – An error occurred for which the error type doesn’t fit any of the other categories\. This error type can occur when the server serves an error response from the cache\.
   + `InvalidRequest` – The server received an invalid request from the viewer\.
   + `InvalidRequestBlocked` – Access to the requested resource is blocked\.
   + `InvalidRequestCertificate` – The distribution doesn’t match the SSL/TLS certificate for which the HTTPS connection was established\.
   + `InvalidRequestHeader` – The request contained an invalid header\.
   + `InvalidRequestMethod` – The distribution is not configured to handle the HTTP request method that was used\. This can happen when the distribution supports only cacheable requests\.
   + `OriginConnectError` – The server couldn’t connect to the origin\.
   + `OriginContentRangeLengthError` – The `Content-Length` header in the origin’s response doesn’t match the length in the `Content-Range` header\.
   + `OriginDnsError` – The server couldn’t resolve the origin’s domain name\.
   + `OriginError` – The origin returned an incorrect response\.
   + `OriginHeaderTooBigError` – A header returned by the origin is too big for the edge server to process\.
   + `OriginInvalidResponseError` – The origin returned an invalid response\.
   + `OriginReadError` – The server couldn’t read from the origin\.
   + `OriginWriteError` – The server couldn’t write to the origin\.
   + `OriginZeroSizeObjectError` – A zero size object sent from the origin resulted in an error\.
   + `SlowReaderOriginError` – The viewer was slow to read the message that caused the origin error\.

1. **`sc-content-type`**

   The value of the HTTP `Content-Type` header of the response\.

1. **`sc-content-len`**

   The value of the HTTP `Content-Length` header of the response\.

1. **`sc-range-start`**

   When the response contains the HTTP `Content-Range` header, this field contains the range start value\.

1. **`sc-range-end`**

   When the response contains the HTTP `Content-Range` header, this field contains the range end value\.

The following is an example log file for a distribution:

```
#Version: 1.0
#Fields: date time x-edge-location sc-bytes c-ip cs-method cs(Host) cs-uri-stem sc-status cs(Referer) cs(User-Agent) cs-uri-query cs(Cookie) x-edge-result-type x-edge-request-id x-host-header cs-protocol cs-bytes time-taken x-forwarded-for ssl-protocol ssl-cipher x-edge-response-result-type cs-protocol-version fle-status fle-encrypted-fields c-port time-to-first-byte x-edge-detailed-result-type sc-content-type sc-content-len sc-range-start sc-range-end
2019-12-04	21:02:31	LAX1	392	192.0.2.100	GET	d111111abcdef8.cloudfront.net	/index.html	200	-	Mozilla/5.0%20(Windows%20NT%2010.0;%20Win64;%20x64)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/78.0.3904.108%20Safari/537.36	-	-	Hit	SOX4xwn4XV6Q4rgb7XiVGOHms_BGlTAC4KyHmureZmBNrjGdRLiNIQ==	d111111abcdef8.cloudfront.net	https	23	0.001	-	TLSv1.2	ECDHE-RSA-AES128-GCM-SHA256	Hit	HTTP/2.0	-	-	11040	0.001	Hit	text/html	78	-	-
2019-12-04	21:02:31	LAX1	392	192.0.2.100	GET	d111111abcdef8.cloudfront.net	/index.html	200	-	Mozilla/5.0%20(Windows%20NT%2010.0;%20Win64;%20x64)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/78.0.3904.108%20Safari/537.36	-	-	Hit	k6WGMNkEzR5BEM_SaF47gjtX9zBDO2m349OY2an0QPEaUum1ZOLrow==	d111111abcdef8.cloudfront.net	https	23	0.000	-	TLSv1.2	ECDHE-RSA-AES128-GCM-SHA256	Hit	HTTP/2.0	-	-	11040	0.000	Hit	text/html	78	-	-
2019-12-04	21:02:31	LAX1	392	192.0.2.100	GET	d111111abcdef8.cloudfront.net	/index.html	200	-	Mozilla/5.0%20(Windows%20NT%2010.0;%20Win64;%20x64)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/78.0.3904.108%20Safari/537.36	-	-	Hit	f37nTMVvnKvV2ZSvEsivup_c2kZ7VXzYdjC-GUQZ5qNs-89BlWazbw==	d111111abcdef8.cloudfront.net	https	23	0.001	-	TLSv1.2	ECDHE-RSA-AES128-GCM-SHA256	Hit	HTTP/2.0	-	-	11040	0.001	Hit	text/html	78	-	-	
2019-12-13	22:36:27	SEA19-C1	900	192.0.2.200	GET	d111111abcdef8.cloudfront.net	/favicon.ico	502	http://www.example.com/	Mozilla/5.0%20(Windows%20NT%2010.0;%20Win64;%20x64)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/78.0.3904.108%20Safari/537.36	-	-	Error	1pkpNfBQ39sYMnjjUQjmH2w1wdJnbHYTbag21o_3OfcQgPzdL2RSSQ==	www.example.com	http	675	0.102	-	-	-	Error	HTTP/1.1	-	-	25260	0.102	OriginDnsError	text/html	507	-	-
2019-12-13	22:36:26	SEA19-C1	900	192.0.2.200	GET	d111111abcdef8.cloudfront.net	/	502	-	Mozilla/5.0%20(Windows%20NT%2010.0;%20Win64;%20x64)%20AppleWebKit/537.36%20(KHTML,%20like%20Gecko)%20Chrome/78.0.3904.108%20Safari/537.36	-	-	Error	3AqrZGCnF_g0-5KOvfA7c9XLcf4YGvMFSeFdIetR1N_2y8jSis8Zxg==	www.example.com	http	735	0.107	-	-	-	Error	HTTP/1.1	-	-	3802	0.107	OriginDnsError	text/html	507	-	-
2019-12-13	22:37:02	SEA19-C2	900	192.0.2.200	GET	d111111abcdef8.cloudfront.net	/	502	-	curl/7.55.1	-	-	Error	kBkDzGnceVtWHqSCqBUqtA_cEs2T3tFUBbnBNkB9El_uVRhHgcZfcw==	www.example.com	http	387	0.103	-	-	-	Error	HTTP/1.1	-	-	12644	0.103	OriginDnsError	text/html	507	-	-
```

## Charges for standard logs<a name="AccessLogsCharges"></a>

Standard logging is an optional feature of CloudFront\. There is no extra charge for enabling standard logging\. However, you accrue the usual Amazon S3 charges for storing and accessing the files on Amazon S3 \(you can delete them at any time\)\.

For more information about Amazon S3 pricing, see [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing/)\.

For more information about CloudFront pricing, see [CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.