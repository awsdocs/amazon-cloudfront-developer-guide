# Configuring and Using Standard Logs \(Access Logs\)<a name="AccessLogs"></a>

You can configure CloudFront to create log files that contain detailed information about every user request that CloudFront receives\. These are called *standard logs*, also known as *access logs*\. These standard logs are available for both web and RTMP distributions\. If you enable standard logs, you can also specify the Amazon S3 bucket that you want CloudFront to save files in\.

You can enable standard logs when you create or update a distribution\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

CloudFront also offers real\-time logs, which give you information about requests made to a distribution in real time \(logs are delivered within seconds of receiving the requests\)\. You can use real\-time logs to monitor, analyze, and take action based on content delivery performance\. For more information, see [Real\-time logs](real-time-logs.md)\.

**Topics**
+ [How Standard Logging Works](#AccessLogsOverview)
+ [Choosing an Amazon S3 Bucket for Your Standard Logs](#access-logs-choosing-s3-bucket)
+ [Permissions Required to Configure Standard Logging and to Access Your Log Files](#AccessLogsBucketAndFileOwnership)
+ [Required CMK Key Policy for Use with SSE\-KMS Buckets](#AccessLogsKMSPermissions)
+ [File Name Format](#AccessLogsFileNaming)
+ [Timing of Standard Log File Delivery](#access-logs-timing)
+ [How Requests Are Logged When the Request URL or Headers Exceed the Maximum Size](#access-logs-request-URL-size)
+ [Analyzing Standard Logs](#access-logs-analyzing)
+ [Editing Your Standard Logging Settings](#ChangeSettings)
+ [Deleting Standard Log Files from an Amazon S3 Bucket](#DeletingLogFiles)
+ [Standard Log File Format](#LogFileFormat)
+ [Charges for Standard Logs](#AccessLogsCharges)

## How Standard Logging Works<a name="AccessLogsOverview"></a>

The following diagram shows how CloudFront logs information about requests for your objects\.

![\[Basic flow for access logs\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

The following explains how CloudFront logs information about requests for your objects, as illustrated in the previous diagram\.

1. In this diagram, you have two websites, A and B, and two corresponding CloudFront distributions\. Users request your objects using URLs that are associated with your distributions\.

1. CloudFront routes each request to the appropriate edge location\.

1. CloudFront writes data about each request to a log file specific to that distribution\. In this example, information about requests related to Distribution A goes into a log file just for Distribution A, and information about requests related to Distribution B goes into a log file just for Distribution B\.

1. CloudFront periodically saves the log file for a distribution in the Amazon S3 bucket that you specified when you enabled logging\. CloudFront then starts saving information about subsequent requests in a new log file for the distribution\.

If no users access your content during a given hour, you don't receive any log files for that hour\.

Each entry in a log file gives details about a single request\. For more information about log file format, see [Standard Log File Format](#LogFileFormat)\.

**Important**  
We recommend that you use the logs to understand the nature of the requests for your content, not as a complete accounting of all requests\. CloudFront delivers access logs on a best\-effort basis\. The log entry for a particular request might be delivered long after the request was actually processed and, in rare cases, a log entry might not be delivered at all\. When a log entry is omitted from access logs, the number of entries in the access logs won't match the usage that appears in the AWS usage and billing reports\.

## Choosing an Amazon S3 Bucket for Your Standard Logs<a name="access-logs-choosing-s3-bucket"></a>

When you enable logging for a distribution, you specify the Amazon S3 bucket that you want CloudFront to store log files in\. If you’re using Amazon S3 as your origin, we recommend that you don’t use the same bucket for your log files; using a separate bucket simplifies maintenance\.

**Note**  
Don’t choose an Amazon S3 bucket in any of the following Regions, because CloudFront doesn’t deliver access logs to buckets in these Regions:  
Africa \(Cape Town\)  af\-south\-1
Asia Pacific \(Hong Kong\)  ap\-east\-1
Europe \(Milan\)  eu\-south\-1
Middle East \(Bahrain\)  me\-south\-1
The [Amazon S3 console](https://console.aws.amazon.com/s3/home) shows the bucket’s Region\.

You can store the log files for multiple distributions in the same bucket\. When you enable logging, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions\.

## Permissions Required to Configure Standard Logging and to Access Your Log Files<a name="AccessLogsBucketAndFileOwnership"></a>

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

## Required CMK Key Policy for Use with SSE\-KMS Buckets<a name="AccessLogsKMSPermissions"></a>

If you enabled server\-side encryption for your Amazon S3 bucket using AWS KMS\-managed keys \(SSE\-KMS\) with a customer\-managed Customer Master Key \(CMK\), you must add the following to the key policy for your CMK to enable writing log files to the bucket\. You cannot use the default CMK because CloudFront won't be able to upload the log files to the bucket\.

```
{
    "Sid": "Allow CloudFront Flow Logs to use the key",
    "Effect": "Allow",
    "Principal": {
        "Service": "delivery.logs.amazonaws.com"
    },
    "Action": "kms:GenerateDataKey*",
    "Resource": "*"
}
```

## File Name Format<a name="AccessLogsFileNaming"></a>

The name of each log file that CloudFront saves in your Amazon S3 bucket uses the following file name format:

`<optional prefix>/<distribution ID>.YYYY-MM-DD-HH.unique-ID.gz`

The date and time are in Coordinated Universal Time \(UTC\)\.

For example, if you use `example-prefix` as the prefix, and your distribution ID is `EMLARXS9EXAMPLE`, your file names look similar to this:

`example-prefix/EMLARXS9EXAMPLE.2019-11-14-20.RT4KCN4SGK9.gz`

When you enable logging for a distribution, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions\. If you include a value for the log file prefix and your prefix doesn't end with a forward slash \(`/`\), CloudFront appends one automatically\. If your prefix does end with a forward slash, CloudFront doesn't add another one\.

The `.gz` at the end of the file name indicates that CloudFront has compressed the log file using gzip\.

## Timing of Standard Log File Delivery<a name="access-logs-timing"></a>

CloudFront delivers standard logs for a distribution up to several times an hour\. In general, a log file contains information about the requests that CloudFront received during a given time period\. CloudFront usually delivers the log file for that time period to your Amazon S3 bucket within an hour of the events that appear in the log\. Note, however, that some or all log file entries for a time period can sometimes be delayed by up to 24 hours\. When log entries are delayed, CloudFront saves them in a log file for which the file name includes the date and time of the period in which the requests occurred, not the date and time when the file was delivered\.

When creating a log file, CloudFront consolidates information for your distribution from all of the edge locations that received requests for your objects during the time period that the log file covers\.

CloudFront can save more than one file for a time period depending on how many requests CloudFront receives for the objects associated with a distribution\.

CloudFront begins to reliably deliver access logs about four hours after you enable logging\. You might get a few access logs before that time\.

**Note**  
If no users request your objects during the time period, you don't receive any log files for that period\.

CloudFront also offers real\-time logs, which give you information about requests made to a distribution in real time \(logs are delivered within seconds of receiving the requests\)\. You can use real\-time logs to monitor, analyze, and take action based on content delivery performance\. For more information, see [Real\-time logs](real-time-logs.md)\.

## How Requests Are Logged When the Request URL or Headers Exceed the Maximum Size<a name="access-logs-request-URL-size"></a>

If the total size of all request headers, including cookies, exceeds 20 KB, or if the URL exceeds 8192 bytes, CloudFront can't parse the request completely and can't log the request\. Because the request isn't logged, you won't see in the log files the HTTP error status code returned\.

If the request body exceeds the maximum size, the request is logged, including the HTTP error status code\.

## Analyzing Standard Logs<a name="access-logs-analyzing"></a>

Because you can receive multiple access logs per hour, we recommend that you combine all the log files you receive for a given time period into one file\. You can then analyze the data for that period more accurately and completely\.

One way to analyze your access logs is to use [Amazon Athena](http://aws.amazon.com/athena/)\. Athena is an interactive query service that can help you analyze data for AWS services, including CloudFront\. To learn more, see [ Querying Amazon CloudFront Logs](https://docs.aws.amazon.com/athena/latest/ug/cloudfront-logs.html) in the *Amazon Athena User Guide*\.

In addition, the following AWS blog posts discuss some ways to analyze access logs\.
+ [ Amazon CloudFront Request Logging](http://aws.amazon.com/blogs/aws/amazon-cloudfront-request-logging/) \(for content delivered via HTTP\)
+ [ Amazon CloudFront Now Supports Streaming Access Logs](http://aws.amazon.com/blogs/aws/amazon-cloudfront-supports-streaming-access-logs/) \(for content delivered via RTMP\)
+ [ Enhanced CloudFront Logs, Now With Query Strings](http://aws.amazon.com/blogs/aws/enhanced-cloudfront-logs-now-with-query-strings/)

**Important**  
We recommend that you use the logs to understand the nature of the requests for your content, not as a complete accounting of all requests\. CloudFront delivers access logs on a best\-effort basis\. The log entry for a particular request might be delivered long after the request was actually processed and, in rare cases, a log entry might not be delivered at all\. When a log entry is omitted from access logs, the number of entries in the access logs won't match the usage that appears in the AWS usage and billing reports\.

## Editing Your Standard Logging Settings<a name="ChangeSettings"></a>

You can enable or disable logging, change the Amazon S3 bucket where your logs are stored, and change the prefix for log files by using the [CloudFront console](https://console.aws.amazon.com/cloudfront/home) or the CloudFront API\. Your changes to logging settings take effect within 12 hours\.

For more information, see the following topics:
+ To update a web or RTMP distribution using the CloudFront console, see [Updating a Distribution](HowToUpdateDistribution.md)\.
+ To update a web distribution using the CloudFront API, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.
+ To update an RTMP distribution using the CloudFront API, see [UpdateStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateStreamingDistribution.html) in the *Amazon CloudFront API Reference*\.

To use the CloudFront API to change access log settings for web distributions, you must use API version `2009-04-02` or later\. To use the CloudFront API to change access log settings for RTMP distributions, you must use API version `2010-05-01` or later\.

## Deleting Standard Log Files from an Amazon S3 Bucket<a name="DeletingLogFiles"></a>

CloudFront does not automatically delete log files from your Amazon S3 bucket\. For information about deleting log files from an Amazon S3 bucket, see the following topics:
+ Using the Amazon S3 console: [Deleting Objects](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-objects.html) in the *Amazon Simple Storage Service Console User Guide*\.
+ Using the REST API: [DeleteObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html) in the *Amazon Simple Storage Service API Reference*\.

## Standard Log File Format<a name="LogFileFormat"></a>

**Topics**
+ [Web Distribution Standard Log File Format](#BasicDistributionFileFormat)
+ [RTMP Distribution Log File Format](#StreamingDistributionLogFileFormat)

Each entry in a log file gives details about a single viewer request\. The log files for web and for RTMP distributions are not identical, but they share the following characteristics:
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

### Web Distribution Standard Log File Format<a name="BasicDistributionFileFormat"></a>

The log file for a web distribution includes the following fields in the listed order\.


| Field Number | Field Name | Description | 
| --- | --- | --- | 
| 1 | date | The date on which the event occurred in the format YYYY\-MM\-DD\. For example, 2019\-06\-30\. The date and time are in Coordinated Universal Time \(UTC\)\. For WebSocket connections, this is the date when the connection closed\. | 
| 2 | time | The time when the CloudFront server finished responding to the request \(in UTC\), for example, 01:42:39\. For WebSocket connections, this is the time when the connection is closed\. | 
| 3 | x\-edge\-location | The edge location that served the request\. Each edge location is identified by a three\-letter code and an arbitrarily assigned number, for example, DFW3\. The three\-letter code typically corresponds with the International Air Transport Association airport code for an airport near the edge location\. \(These abbreviations might change in the future\.\) For a list of edge locations, see the [Amazon CloudFront Infrastructure](http://aws.amazon.com/cloudfront/features/?#Amazon_CloudFront_Infrastructure) page\. | 
| 4 | sc\-bytes | The total number of bytes that CloudFront served to the viewer in response to the request, including headers, for example, 1045619\. For WebSocket connections, this is the total number of bytes sent from the server to the client through the connection\. | 
| 5 | c\-ip | The IP address of the viewer that made the request, for example, 192\.0\.2\.183 or 2001:0db8:85a3:0000:0000:8a2e:0370:7334\. If the viewer used an HTTP proxy or a load balancer to send the request, the value of c\-ip is the IP address of the proxy or load balancer\. See also X\-Forwarded\-For in field 20\.  | 
| 6 | cs\-method | The HTTP request method: DELETE, GET, HEAD, OPTIONS, PATCH, POST, or PUT\. | 
| 7 | cs\(Host\) | The domain name of the CloudFront distribution, for example, d111111abcdef8\.cloudfront\.net\. | 
| 8 | cs\-uri\-stem | The portion of the URI that identifies the path and object, for example, /images/cat\.jpg\. Question marks \(?\) in URLs and query strings are not included in the log\. | 
| 9 | sc\-status | One of the following values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 10 | cs\(Referer\) | The name of the domain that originated the request\. Common referrers include search engines, other websites that link directly to your objects, and your own website\. | 
| 11 | cs\(User\-Agent\) | The value of the User\-Agent header in the request\. The User\-Agent header identifies the source of the request, such as the type of device and browser that submitted the request and, if the request came from a search engine, which search engine\. For more information, see [User\-Agent Header](RequestAndResponseBehaviorCustomOrigin.md#request-custom-user-agent-header)\. | 
| 12 | cs\-uri\-query | The query string portion of the URI, if any\. When a URI doesn't contain a query string, this field's value is a hyphen \(\-\)\. For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.  | 
| 13 | cs\(Cookie\) | The cookie header in the request, including name\-value pairs and the associated attributes\. If you enable cookie logging, CloudFront logs the cookies in all requests regardless of which cookies you choose to forward to the origin\. When a request doesn't include a cookie header, this field's value is a hyphen \(\-\)\. For more information about cookies, see [Caching Content Based on Cookies](Cookies.md)\.  | 
| 14 | x\-edge\-result\-type | How CloudFront classifies the response after the last byte left the edge location\. In some cases, the result type can change between the time that CloudFront is ready to send the response and the time that CloudFront has finished sending the response\. See also `x-edge-response-result-type` in field 23\. For example, in HTTP streaming, suppose CloudFront finds a segment in the edge cache\. In that scenario, the value of this field would ordinarily be `Hit`\. However, if the viewer closes the connection before CloudFront has delivered the entire segment, the final result type, and thus the value of this field, is `Error`\. As another example, WebSocket connections will have a value of `Miss` for this field because the content is not cacheable and is proxied directly back to the origin server\. Possible values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 15 | x\-edge\-request\-id | An encrypted string that uniquely identifies a request\. In the response header, this is x\-amz\-cf\-id\. | 
| 16 | x\-host\-header | The value that the viewer included in the `Host` header for this request\. This is the domain name in the request: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 17 | cs\-protocol | The protocol that the viewer specified in the request: http, https, ws, or wss\. | 
| 18 | cs\-bytes | The number of bytes of data that the viewer included in the request, including headers\. For WebSocket connections, this is the total number of bytes sent from the client to the server on the connection\. | 
| 19 | time\-taken | The number of seconds \(to the thousandth of a second, for example, 0\.002\) between the time that a CloudFront edge server receives a viewer's request and the time that CloudFront writes the last byte of the response to the edge server's output queue as measured on the server\. From the perspective of the viewer, the total time to get the full object will be longer than this value due to network latency and TCP buffering\. | 
| 20 | x\-forwarded\-for | If the viewer used an HTTP proxy or a load balancer to send the request, the value of `c-ip` in field 5 is the IP address of the proxy or load balancer\. In that case, this field is the IP address of the viewer that originated the request\. This field contains IPv4 addresses \(such as 192\.0\.2\.44\) and IPv6 addresses \(such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334\), as applicable\. If the viewer did not use an HTTP proxy or a load balancer, the value of `x-forwarded-for` is a hyphen \(\-\)\.  | 
| 21 | ssl\-protocol | When `cs-protocol` in field 17 is `https`, this field contains the SSL/TLS protocol that the client and CloudFront negotiated for transmitting the request and response\. Possible values include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html) When `cs-protocol` in field 17 is `http`, the value for this field is a hyphen \(\-\)\.  | 
| 22 | ssl\-cipher |  When `cs-protocol` in field 17 is `https`, this field contains the SSL/TLS cipher that the client and CloudFront negotiated for encrypting the request and response\. Possible values include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html) When `cs-protocol` in field 17 is `http`, the value for this field is a hyphen \(\-\)\.  | 
| 23 | x\-edge\-response\-result\-type | How CloudFront classified the response just before returning the response to the viewer\. See also `x-edge-result-type` in field 14\.  Possible values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 24 | cs\-protocol\-version |  The HTTP version that the viewer specified in the request\. Possible values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 25 | fle\-status | When [field\-level encryption](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field-level-encryption.html) is configured for a distribution, this field contains a code that indicates whether the request body was successfully processed\. If field\-level encryption is not configured for the distribution, the value of this field is a hyphen \(\-\)\. When CloudFront successfully processes the request body, encrypts values in the specified fields, and forwards the request to the origin, the value of this field is `Processed`\. The value of `x-edge-result-type`, field 14, can still indicate a client\-side or server\-side error in this case\. If the request exceeds a field\-level encryption quota, `fle-status` contains one of the following error codes, and CloudFront returns HTTP status code `400` to the viewer\. For a list of the current quotas on field\-level encryption, see [Quotas on Field\-Level Encryption](cloudfront-limits.md#limits-field-level-encryption)\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html) Other possible values for this field include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 26 | fle\-encrypted\-fields | The number of fields that CloudFront encrypted and forwarded to the origin\. CloudFront streams the processed request to the origin as it encrypts data, so fle\-encrypted\-fields can have a value even if the value of fle\-status is an error\. If field\-level encryption is not configured for the distribution, the value of fle\-encrypted\-fields is a hyphen \(\-\)\.  | 
| 27 | c\-port | The port number of the request from the viewer\. | 
| 28 | time\-to\-first\-byte | The number of seconds between receiving the request and writing the first byte of the response, as measured on the server\. | 
| 29 | x\-edge\-detailed\-result\-type |  When `x-edge-result-type` \(field 14\) is not `Error`, this field contains the same value as `x-edge-result-type`\. When `x-edge-result-type` is `Error`, this field contains the specific type of error\. Possible error type values for this field include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 30 | sc\-content\-type | The value of the HTTP Content\-Type header of the response\. | 
| 31 | sc\-content\-len | The value of the HTTP Content\-Length header of the response\. | 
| 32 | sc\-range\-start | When the response contains the HTTP Content\-Range header, this field contains the range start value\. | 
| 33 | sc\-range\-end | When the response contains the HTTP Content\-Range header, this field contains the range end value\. | 

The following is an example log file for a web distribution:

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

### RTMP Distribution Log File Format<a name="StreamingDistributionLogFileFormat"></a>

Each record in an RTMP access log represents a playback event, for example, connect, play, pause, stop, disconnect, and so on\. As a result, CloudFront generates multiple log records each time a viewer watches a video\. To relate log records that stem from the same stream ID, use the `x-sid` field\.

**Note**  
Some fields have values for all events, and some have values only for play, stop, pause, unpause, and seek events\. Usually, when the log file contains a hyphen \(\-\) for a field, it means that field isn't relevant for the corresponding event\. 

The following table describes the fields that are present in each record in the RTMP distribution log file, regardless of the type of event\. The fields appear in the log in the order listed\.


| Field Number | Field Name | Description | 
| --- | --- | --- | 
| 1 | date | The date on which the event occurred in the format YYYY\-MM\-DD, for example, 2019\-05\-23\. The date and time are in Coordinated Universal Time \(UTC\)\. | 
| 2 | time | The time when the server received the request \(in UTC\), for example, 01:42:39\. | 
| 3 | x\-edge\-location | The edge location where the playback event occurred\. Each edge location is identified by a three\-letter code and an arbitrarily assigned number, for example, DFW3\. The three\-letter code typically corresponds with the International Air Transport Association airport code for an airport near the edge location\. \(These abbreviations might change in the future\.\) For a list of edge locations, see the [Amazon CloudFront Infrastructure](http://aws.amazon.com/cloudfront/features/?#Amazon_CloudFront_Infrastructure) page\. | 
| 4 | c\-ip | Client IP, for example, 192\.0\.2\.183\. | 
| 5 | x\-event | The event type\. This is a Connect, Disconnect, Play, Stop, Pause, Unpause, or Seek event\. | 
| 6 | sc\-bytes | The running total number of bytes sent from the server to the client, up to the time of the event\. | 
| 7 | x\-cf\-status | A code indicating the status of the event\. Currently, "OK" is the only value for this field\. New functionality in the future could require new status codes\. | 
| 8 | x\-cf\-client\-id | An opaque string identifier that can be used to differentiate clients\. This value is unique for each connection\. | 
| 9 | cs\-uri\-stem | The stem portion of the URI, including the application and the application instance\. This is sometimes referred to as the FMS connect string\. For example, rtmp://shqshne4jdp4b6\.cloudfront\.net/cfx/st\. Question marks \(?\) in URLs and query strings are not included in the log\. | 
| 10 | cs\-uri\-query | The query string portion of the URI that is included on the connect string\. Question marks \(?\) in URLs and query strings are not included in the log\. | 
| 11 | c\-referrer | The URI of the referrer\. | 
| 12 | x\-page\-url | The URL of the page from which the SWF is linked\. | 
| 13 | c\-user\-agent | The value of the User\-Agent header in the request\. The User\-Agent header identifies the type of device that submitted the request\. For more information, see [User\-Agent Header](RequestAndResponseBehaviorCustomOrigin.md#request-custom-user-agent-header)\. | 

The following fields usually have values only for Play, Stop, Pause, Unpause, and Seek events\. For other events, they contain a single hyphen \(\-\)\. These fields appear in the log after the fields in the preceding table and in the order listed\.


| Field Number | Field Name | Description | 
| --- | --- | --- | 
| 14 | x\-sname | The stream name\. | 
| 15 | x\-sname\-query | The stream query string, if any\. | 
| 16 | x\-file\-ext | The stream type, for example, FLV\. | 
| 17 | x\-sid | The stream ID\. This is a unique integer identifier for the connection\. | 

The following is an example of a log file for an RTMP distribution:

```
#Version: 1.0
#Fields: date time x-edge-location c-ip x-event sc-bytes x-cf-status x-cf-client-id cs-uri-stem cs-uri-query c-referrer x-page-url​	c-user-agent x-sname x-sname-query x-file-ext x-sid
2010-03-12	 23:51:20	 SEA4	 192.0.2.147	 connect	 2014	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.example.com/player.swf	 http://www.example.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 -	 -	 -	 -
2010-03-12	 23:51:21	 SEA4	 192.0.2.222	 play	 3914	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.example.com/player.swf	 http://www.example.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 myvideo	 p=2&q=4	 flv	 1
2010-03-12	 23:53:44	 SEA4	 192.0.2.4	 stop	 323914	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.example.com/player.swf	 http://www.example.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 dir/other/myvideo	 p=2&q=4	 flv	 1
2010-03-12	 23:53:44	 SEA4	 192.0.2.103	 play	 8783724	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.example.com/player.swf	 http://www.example.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 dir/favs/myothervideo	 p=42&q=14	 mp4	 2
2010-03-12	 23:56:21	 SEA4	 192.0.2.199	 stop	 429822014	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.example.com/player.swf	 http://www.example.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 dir/favs/myothervideo	 p=42&q=14	 mp4	 2
2010-03-12	 23:59:44	 SEA4	 192.0.2.14	 disconnect	 429824092	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.example.com/player.swf	 http://www.example.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 -	 -	 -	 -
```

## Charges for Standard Logs<a name="AccessLogsCharges"></a>

Standard logging is an optional feature of CloudFront\. There is no extra charge for enabling standard logging\. However, you accrue the usual Amazon S3 charges for storing and accessing the files on Amazon S3 \(you can delete them at any time\)\.

For more information about Amazon S3 pricing, see [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing/)\.

For more information about CloudFront pricing, see [CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.