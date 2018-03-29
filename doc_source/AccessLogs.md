# Access Logs<a name="AccessLogs"></a>

You can configure CloudFront to create log files that contain detailed information about every user request that CloudFront receives\. These access logs are available for both web and RTMP distributions\. If you enable logging, you can also specify the Amazon S3 bucket that you want CloudFront to save files in\. 

**Topics**
+ [How Logging Works](#AccessLogsOverview)
+ [Choosing an Amazon S3 Bucket for Your Access Logs](#access-logs-choosing-s3-bucket)
+ [Permissions Required to Configure Logging and to Access Your Log Files](#AccessLogsBucketAndFileOwnership)
+ [File Name Format](#AccessLogsFileNaming)
+ [Timing of Log File Delivery](#access-logs-timing)
+ [Analyzing Access Logs](#access-logs-analyzing)
+ [Editing Your Logging Settings](#ChangeSettings)
+ [Deleting Log Files from an Amazon S3 Bucket](#DeletingLogFiles)
+ [Log File Format](#LogFileFormat)
+ [Charges for Access Logs](#AccessLogsCharges)

## How Logging Works<a name="AccessLogsOverview"></a>

The following diagram shows how CloudFront logs information about requests for your objects\.

![\[Basic flow for access logs\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

The following explains how CloudFront logs information about requests for your objects, as illustrated in the previous graphic\.

1. In this diagram, you have two websites, A and B, and two corresponding CloudFront distributions\. Users request your objects using URLs that are associated with your distributions\.

1. CloudFront routes each request to the appropriate edge location\.

1. CloudFront writes data about each request to a log file specific to that distribution\. In this example, information about requests related to Distribution A goes into a log file just for Distribution A, and information about requests related to Distribution B goes into a log file just for Distribution B\.

1. CloudFront periodically saves the log file for a distribution in the Amazon S3 bucket that you specified when you enabled logging\. CloudFront then starts saving information about subsequent requests in a new log file for the distribution\.

Each entry in a log file gives details about a single request\. For more information about log file format, see [Log File Format](#LogFileFormat)\.

## Choosing an Amazon S3 Bucket for Your Access Logs<a name="access-logs-choosing-s3-bucket"></a>

When you enable logging for a distribution, you specify the Amazon S3 bucket that you want CloudFront to store log files in\. If you're using Amazon S3 as your origin, we recommend that you do not use the same bucket for your log files; using a separate bucket simplifies maintenance\. 

You can store the log files for multiple distributions in the same bucket\. When you enable logging, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions\.

If no users access your content during a given hour, you don't receive any log files for that hour\.

## Permissions Required to Configure Logging and to Access Your Log Files<a name="AccessLogsBucketAndFileOwnership"></a>

Your AWS account must have the following permissions for the bucket that you specify for log files:
+ The S3 access control list \(ACL\) for the bucket must grant you `FULL_CONTROL`\. If you're the bucket owner, your account has this permission by default\. If you're not, the bucket owner must update the ACL for the bucket\.
+ `s3:GetBucketAcl`
+ `s3:PutBucketAcl`

Note the following:

**ACL for the bucket**  
When you create or update a distribution and enable logging, CloudFront uses these permissions to update the ACL for the bucket to give the awsdatafeeds account `FULL_CONTROL` permission\. The awsdatafeeds account writes log files to the bucket\. If your account doesn't have the required permissions, creating or updating the distribution will fail\.  
If you update the ACL for the bucket to remove permissions for the awsdatafeeds account, CloudFront won't be able to save logs to the S3 bucket any longer\. 
In some circumstances, if you programmatically submit a request to create a bucket but a bucket with the specified name already exists, S3 resets permissions on the bucket to the default value\. If you configured CloudFront to save access logs in an S3 bucket and you stop getting logs in that bucket, check permissions on the bucket to ensure that CloudFront has the necessary permissions\.

**ACL for each log file**  
In addition to the ACL on the bucket, there's an ACL on each log file\. The bucket owner has `FULL_CONTROL` permission on each log file, the distribution owner \(if different from the bucket owner\) has no permission, and the awsdatafeeds account has read and write permissions\. 

**Disabling logging**  
If you disable logging, CloudFront doesn't delete the ACLs for either the bucket or the log files\. If you want, you can do that yourself\.

## File Name Format<a name="AccessLogsFileNaming"></a>

The name of each log file that CloudFront saves in your Amazon S3 bucket uses the following file name format: 

 `bucket-name.s3.amazonaws.com/optional-prefix/distribution-ID.YYYY-MM-DD-HH.unique-ID.gz ` 

The date and time are in Coordinated Universal time \(UTC\)\.

For example, if your bucket name is `mylogs`, your prefix is `myprefix/`, and your distribution ID is `EMLARXS9EXAMPLE`, your file names look similar to this:

 `mylogs.s3.amazonaws.com/myprefix/EMLARXS9EXAMPLE.2014-11-14-20.RT4KCN4SGK9.gz` 

When you enable logging for a distribution, you can specify an optional prefix for the file names, so you can keep track of which log files are associated with which distributions\. If you include a value for the log file prefix and your prefix doesn't include a `/`, CloudFront adds one automatically\. If your value does include a `/`, CloudFront doesn't add another one\. 

The \.gz at the end of the file name indicates that CloudFront has compressed the log file using gzip\.

## Timing of Log File Delivery<a name="access-logs-timing"></a>

CloudFront delivers access logs for a distribution up to several times an hour\. In general, a log file contains information about the requests that CloudFront received during a given time period\. CloudFront usually delivers the log file for that time period to your Amazon S3 bucket within an hour of the events that appear in the log\. Note, however, that some or all log file entries for a time period can sometimes be delayed by up to 24 hours\. When log entries are delayed, CloudFront saves them in a log file for which the file name includes the date and time of the period in which the requests occurred, not the date and time when the file was delivered\.

When creating a log file, CloudFront consolidates information for your distribution from all of the edge locations that received requests for your objects during the time period that the log file covers\.

CloudFront can save more than one file for a time period depending on how many requests CloudFront receives for the objects associated with a distribution\.

CloudFront begins to reliably deliver access logs about four hours after you enable logging\. You might get a few access logs before that time\.

**Note**  
If no users request your objects during the time period, you don't receive any log files for that period\.

## Analyzing Access Logs<a name="access-logs-analyzing"></a>

Because you can receive multiple access logs per hour, we recommend that you combine all the log files you receive for a given period into one file\. You can then analyze the data for that period more quickly and accurately\.

One way to analyze your access logs is to use Amazon Athena\. Athena is an interactive query service that can help you analyze data for AWS services, including CloudFront\. To learn more, see [ Querying Amazon CloudFront Logs](https://docs.aws.amazon.com/athena/latest/ug/cloudfront-logs.html) in the Amazon Athena User Guide\.

In addition, the following AWS blog posts discuss some ways to analyze access logs\.

AWS Blog: [ Amazon CloudFront Request Logging](http://aws.typepad.com/aws/2009/05/amazon-cloudfront-request-logging.html) \(for content delivered via HTTP\)

AWS Blog: [ Amazon CloudFront Now Supports Streaming Access Logs](http://aws.typepad.com/aws/2010/05/amazon-cloudfront-supports-streaming-access-logs.html) \(for content delivered via RTMP\)

AWS Blog: [ Enhanced CloudFront Logs, Now With Query Strings](http://aws.typepad.com/aws/2010/07/enhanced-cloudfront-logs-now-with-query-strings.html)

**Important**  
We recommend that you use the logs to understand the nature of the requests for your content, not as a complete accounting of all requests\. CloudFront delivers access logs on a best\-effort basis\. The log entry for a particular request might be delivered long after the request was actually processed and, in rare cases, a log entry might not be delivered at all\. When a log entry is omitted from access logs, the number of entries in the access logs won't match the usage that appears in the AWS usage and billing reports\.

## Editing Your Logging Settings<a name="ChangeSettings"></a>

You can enable or disable logging, change the Amazon S3 bucket where your logs are stored, and change the prefix for log files by using the CloudFront console or the CloudFront API\. Your changes to logging settings take effect within 12 hours\.

For more information, see the following topics:
+ Updating a web or an RTMP distribution using the CloudFront console: [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md)\.
+ Updating a web distribution using the CloudFront API: [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) in the *Amazon CloudFront API Reference*\.
+ Updating an RTMP distribution using the CloudFront API: [PUT Streaming Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutStreamingDistConfig.html) in the *Amazon CloudFront API Reference*\.

To use the CloudFront API to change access log settings for web distributions, you must use the 2009\-04\-02 or later version of the API\. To use the CloudFront API to change access log settings for RTMP distributions, you must use the 2010\-05\-01 or later version of the API\. 

## Deleting Log Files from an Amazon S3 Bucket<a name="DeletingLogFiles"></a>

CloudFront does not automatically delete log files from your Amazon S3 bucket\. For information about deleting log files from an Amazon S3 bucket, see the following topics:
+ Using the Amazon S3 console: [Deleting an Object](http://docs.aws.amazon.com/AmazonS3/latest/UG/DeletinganObject.html) in the *Amazon Simple Storage Service Console User Guide*\.
+ Using the REST API: [DELETE Object](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectDELETE.html) in the *Amazon Simple Storage Service API Reference*\.
+ Using the SOAP API: [DeleteObject](http://docs.aws.amazon.com/AmazonS3/latest/API/SOAPDeleteObject.html) in the *Amazon Simple Storage Service API Reference*\.

## Log File Format<a name="LogFileFormat"></a>

**Topics**
+ [Web Distribution Log File Format](#BasicDistributionFileFormat)
+ [RTMP Distribution Log File Format](#StreamingDistributionLogFileFormat)

Each entry in a log file gives details about a single user request\. The log files for web and for RTMP distributions are not identical, but they share the following characteristics:
+ Use the W3C extended log file format\. \(For more information, go to [http://www\.w3\.org/TR/WD\-logfile\.html](http://www.w3.org/TR/WD-logfile.html)\.\)
+ Contain tab\-separated values\.
+ Contain records that are not necessarily in chronological order\.
+ Contain two header lines: one with the file\-format version, and another that lists the W3C fields included in each record\.
+ Substitute URL\-encoded equivalents for spaces and non\-standard characters in field values\.

  These non\-standard characters consist of all ASCII codes below 32 and above 127, plus the characters in the following table\. The URL encoding standard is RFC 1738\. For more information, go to [http://www\.ietf\.org/rfc/rfc1738\.txt](http://www.ietf.org/rfc/rfc1738.txt)\. 


****  

| URL\-Encoded Value | Character | 
| --- | --- | 
| %3C | < | 
| %3E | > | 
| %2522 | " | 
| %23 | \# | 
| %25 | % | 
| %7B | \{ | 
| %7D | \} | 
| %7C | \| | 
| %255C | \\ | 
| %5E | ^ | 
| %7E | \~ | 
| %5B | \[ | 
| %5D | \] | 
| %60 | ` | 
| %27 | ' | 
| %2520 | space | 

### Web Distribution Log File Format<a name="BasicDistributionFileFormat"></a>

The log file for a web distribution includes the following fields in the listed order\.


****  

| Field Number | Field Name | Description | 
| --- | --- | --- | 
| 1 | date | The date on which the event occurred in the format yyyy\-mm\-dd, for example, 2015\-06\-30\. The date and time are in Coordinated Universal Time \(UTC\)\. | 
| 2 | time | The time when the CloudFront server finished responding to the request \(in UTC\), for example, 01:42:39\. | 
| 3 | x\-edge\-location | The edge location that served the request\. Each edge location is identified by a three\-letter code and an arbitrarily assigned number, for example, DFW3\. The three\-letter code typically corresponds with the International Air Transport Association airport code for an airport near the edge location\. \(These abbreviations might change in the future\.\) For a list of edge locations, see the Amazon CloudFront detail page, [http://aws\.amazon\.com/cloudfront](http://aws.amazon.com/cloudfront)\. | 
| 4 | sc\-bytes | The total number of bytes that CloudFront served to the viewer in response to the request, including headers, for example, 1045619\. | 
| 5 | c\-ip | The IP address of the viewer that made the request, for example, `192.0.2.183` or `2001:0db8:85a3:0000:0000:8a2e:0370:7334`\. If the viewer used an HTTP proxy or a load balancer to send the request, the value of `c-ip` is the IP address of the proxy or load balancer\. See also `X-Forwarded-For` in field 20\.  | 
| 6 | cs\-method | The HTTP access method: DELETE, GET, HEAD, OPTIONS, PATCH, POST, or PUT\. | 
| 7 | cs\(Host\) | The domain name of the CloudFront distribution, for example, d111111abcdef8\.cloudfront\.net\. | 
| 8 | cs\-uri\-stem | The portion of the URI that identifies the path and object, for example, /images/daily\-ad\.jpg\. | 
| 9 | sc\-status | One of the following values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 10 | cs\(Referer\) | The name of the domain that originated the request\. Common referrers include search engines, other websites that link directly to your objects, and your own website\. | 
| 11 | cs\(User\-Agent\) | The value of the User\-Agent header in the request\. The User\-Agent header identifies the source of the request, such as the type of device and browser that submitted the request and, if the request came from a search engine, which search engine\. For more information, see [User\-Agent Header](RequestAndResponseBehaviorCustomOrigin.md#request-custom-user-agent-header)\. | 
| 12 | cs\-uri\-query | The query string portion of the URI, if any\. When a URI doesn't contain a query string, the value of `cs-uri-query` is a hyphen \(\-\)\. For more information, see [Configuring CloudFront to Cache Based on Query String Parameters](QueryStringParameters.md)\.  | 
| 13 | cs\(Cookie\) | The cookie header in the request, including name\-value pairs and the associated attributes\. If you enable cookie logging, CloudFront logs the cookies in all requests regardless of which cookies you choose to forward to the origin: none, all, or a whitelist of cookie names\. When a request doesn't include a cookie header, the value of `cs(Cookie)` is a hyphen \(\-\)\.  For more information about cookies, see [Configuring CloudFront to Cache Objects Based on Cookies](Cookies.md)\.  | 
| 14 | x\-edge\-result\-type | How CloudFront classifies the response after the last byte left the edge location\. In some cases, the result type can change between the time that CloudFront is ready to send the response and the time that CloudFront has finished sending the response\. For example, in HTTP streaming, suppose CloudFront finds a segment in the edge cache\. The value of `x-edge-response-result-type`, the result type immediately before CloudFront begins to respond to the request, is `Hit`\. However, if the user closes the viewer before CloudFront has delivered the entire segment, the final result type—the value of `x-edge-result-type`—changes to `Error`\.  Possible values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 15 | x\-edge\-request\-id | An encrypted string that uniquely identifies a request\. | 
| 16 | x\-host\-header | The value that the viewer included in the `Host` header for this request\. This is the domain name in the request: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 17 | cs\-protocol | The protocol that the viewer specified in the request, either http or https\. | 
| 18 | cs\-bytes | The number of bytes of data that the viewer included in the request \(client to server bytes\), including headers\. | 
| 19 | time\-taken | The number of seconds \(to the thousandth of a second, for example, 0\.002\) between the time that a CloudFront edge server receives a viewer's request and the time that CloudFront writes the last byte of the response to the edge server's output queue as measured on the server\. From the perspective of the viewer, the total time to get the full object will be longer than this value due to network latency and TCP buffering\. | 
| 20 | x\-forwarded\-for | If the viewer used an HTTP proxy or a load balancer to send the request, the value of `c-ip` in field 5 is the IP address of the proxy or load balancer\. In that case, `x-forwarded-for` is the IP address of the viewer that originated the request\. If the viewer did not use an HTTP proxy or a load balancer, the value of `x-forwarded-for` is a hyphen \(\-\)\.  The `X-Forwarded-For` header contains IPv4 addresses \(such as 192\.0\.2\.44\) and IPv6 addresses \(such as 2001:0db8:85a3:0000:0000:8a2e:0370:7334\), as applicable\.   | 
| 21 | ssl\-protocol | When `cs-protocol` in field 17 is `https`, the SSL protocol that the client and CloudFront negotiated for transmitting the request and response\. When `cs-protocol` is `http`, the value for `ssl-protocol` is a hyphen \(\-\)\. Possible values include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 22 | ssl\-cipher | When `cs-protocol` in field 17 is `https`, the SSL cipher that the client and CloudFront negotiated for encrypting the request and response\. When `cs-protocol` is `http`, the value for `ssl-cipher` is a hyphen \(\-\)\. Possible values include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 23 | x\-edge\-response\-result\-type | How CloudFront classified the response just before returning the response to the viewer\. See also `x-edge-result-type` in field 14\.  Possible values include: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 24 | cs\-protocol\-version | The HTTP version that the viewer specified in the request\. Possible values include HTTP/0\.9, HTTP/1\.0, HTTP/1\.1, and HTTP/2\.0\. | 
| 25 | fle\-status | When [field\-level encryption](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field-level-encryption.html) is configured for a distribution, a code that indicates whether the request body was successfully processed\. If field\-level encryption is not configured for the distribution, the value of `fle-status` is a hyphen \(`-`\)\. When CloudFront successfully processes the request body, encrypts values in the specified fields, and forwards the request to the origin, the value of the `fle-status` column is `Processed`\. The value of `x-edge-result-type`, column 14, can still indicate a client\-side or server\-side error\. If the request exceeds a field\-level encryption limit, `fle-status` contains one of the following error codes, and CloudFront returns HTTP status code 400 to the viewer\. For a list of the current limits on field\-level encryption, see [Limits on Field\-Level Encryption](cloudfront-limits.md#limits-field-level-encryption)\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html) Other possible values for fle\-status include the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/AccessLogs.html)  | 
| 26 | fle\-encrypted\-fields | The number of fields that CloudFront encrypted and forwarded to the origin\. CloudFront streams the processed request to the origin as it encrypts data, so `fle-encrypted-fields` can have a value even if the value of `fle-status` is an error\. If field\-level encryption is not configured for the distribution, the value of `fle-encrypted-fields` is a hyphen \(`-`\)\.  | 

**Note**  
Question marks \(?\) in URLs and query strings are not included in the log\.

The following is an example log file for a web distribution:

```
#Version: 1.0
#Fields: date time x-edge-location sc-bytes c-ip cs-method cs(Host) cs-uri-stem sc-status cs(Referer) cs(User-Agent) cs-uri-query cs(Cookie) x-edge-result-type x-edge-request-id x-host-header cs-protocol cs-bytes time-taken x-forwarded-for ssl-protocol ssl-cipher x-edge-response-result-type cs-protocol-version fle-status fle-encrypted-fields 
2014-05-23 01:13:11 FRA2 182 192.0.2.10 GET d111111abcdef8.cloudfront.net /view/my/file.html 200 www.displaymyfiles.com Mozilla/4.0%20(compatible;%20MSIE%205.0b1;%20Mac_PowerPC) - zip=98101 RefreshHit MRVMF7KydIvxMWfJIglgwHQwZsbG2IhRJ07sn9AkKUFSHS9EXAMPLE== d111111abcdef8.cloudfront.net http - 0.001 - - - RefreshHit HTTP/1.1 Processed 1
2014-05-23 01:13:12 LAX1 2390282 192.0.2.202 GET d111111abcdef8.cloudfront.net /soundtrack/happy.mp3 304 www.unknownsingers.com Mozilla/4.0%20(compatible;%20MSIE%207.0;%20Windows%20NT%205.1) a=b&c=d zip=50158 Hit xGN7KWpVEmB9Dp7ctcVFQC4E-nrcOcEKS3QyAez--06dV7TEXAMPLE== d111111abcdef8.cloudfront.net http - 0.002 - - - Hit HTTP/1.1 - -
```

### RTMP Distribution Log File Format<a name="StreamingDistributionLogFileFormat"></a>

Each record in an RTMP access log represents a playback event, for example, connect, play, pause, stop, disconnect, and so on\. As a result, CloudFront generates multiple log records each time a viewer watches a video\. To relate log records that stem from the same stream ID, use the `x-sid` field\.

**Note**  
Some fields have values for all events, and some have values only for Play, Stop, Pause, Unpause, and Seek events\. Usually, when the log file contains a hyphen \(\-\) for a field, the field isn't relevant for the corresponding event\. 

The following table describes the fields that are present in each record in the RTMP distribution log file, regardless of the type of event\. The fields appear in the log in the order listed\.


****  

| Field Number | Field Name | Description | 
| --- | --- | --- | 
| 1 | date | The date on which the event occurred in the format yyyy\-mm\-dd, for example, 2014\-05\-23\. The date and time are in Coordinated Universal Time \(UTC\)\. | 
| 2 | time | The time when the server received the request \(in UTC\), for example, 01:42:39\. | 
| 3 | x\-edge\-location | The edge location where the playback event occurred\. Each edge location is identified by a three\-letter code and an arbitrarily assigned number, for example, DFW3\. The three\-letter code typically corresponds with the International Air Transport Association airport code for an airport near the edge location\. \(These abbreviations might change in the future\.\) For a list of edge locations, see the Amazon CloudFront detail page, [http://aws\.amazon\.com/cloudfront](http://aws.amazon.com/cloudfront)\. | 
| 4 | c\-ip | Client IP, for example, 192\.0\.2\.183\. | 
| 5 | x\-event | The event type\. This is a Connect, Disconnect, Play, Stop, Pause, Unpause, or Seek event\. | 
| 6 | sc\-bytes | The running total number of bytes sent from the server to the client, up to the time of the event\. | 
| 7 | x\-cf\-status | A code indicating the status of the event\. Currently, "OK" is the only value for this field\. New functionality in the future could require new status codes\. | 
| 8 | x\-cf\-client\-id | An opaque string identifier that can be used to differentiate clients\. This value is unique for each connection\. | 
| 9 | cs\-uri\-stem | The stem portion of the URI, including the application and the application instance\. This is sometimes referred to as the FMS connect string\. For example, rtmp://shqshne4jdp4b6\.cloudfront\.net/cfx/st\.  | 
| 10 | cs\-uri\-query | The query string portion of the URI that is included on the connect string\. | 
| 11 | c\-referrer | The URI of the referrer\. | 
| 12 | x\-page\-url | The URL of the page from which the SWF is linked\. | 
| 13 | c\-user\-agent | The value of the User\-Agent header in the request\. The User\-Agent header identifies the type of device that submitted the request\. For more information, see [User\-Agent Header](RequestAndResponseBehaviorCustomOrigin.md#request-custom-user-agent-header)\. | 

The following fields usually have values only for Play, Stop, Pause, Unpause, and Seek events\. For other events, they contain a single hyphen \(\-\)\. These fields appear in the log after the fields in the preceding table and in the order listed\.


****  

| Field | Description | 
| --- | --- | 
| x\-sname | The stream name\. | 
| x\-sname\-query | The stream query string, if any\. | 
| x\-file\-ext | The stream type, for example, FLV\. | 
| x\-sid | The stream ID\. This is a unique integer identifier for the connection\. | 

**Note**  
Question marks \(?\) in URLs and query strings are not included in the log\.

The following is an example of a log file for an RTMP distribution:

```
#Version: 1.0
#Fields: date time x-edge-location c-ip x-event sc-bytes x-cf-status x-cf-client-id cs-uri-stem cs-uri-query c-referrer x-page-url​	c-user-agent x-sname x-sname-query x-file-ext x-sid
2010-03-12	 23:51:20	 SEA4	 192.0.2.147	 connect	 2014	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.longtailvideo.com/player.swf	 http://www.longtailvideo.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 -	 -	 -	 -
2010-03-12	 23:51:21	 SEA4	 192.0.2.222	 play	 3914	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.longtailvideo.com/player.swf	 http://www.longtailvideo.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 myvideo	 p=2&q=4	 flv	 1
2010-03-12	 23:53:44	 SEA4	 192.0.2.4	 stop	 323914	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.longtailvideo.com/player.swf	 http://www.longtailvideo.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 dir/other/myvideo	 p=2&q=4	 flv	 1
2010-03-12	 23:53:44	 SEA4	 192.0.2.103	 play	 8783724	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.longtailvideo.com/player.swf	 http://www.longtailvideo.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 dir/favs/myothervideo	 p=42&q=14	 mp4	 2
2010-03-12	 23:56:21	 SEA4	 192.0.2.199	 stop	 429822014	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.longtailvideo.com/player.swf	 http://www.longtailvideo.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 dir/favs/myothervideo	 p=42&q=14	 mp4	 2
2010-03-12	 23:59:44	 SEA4	 192.0.2.14	 disconnect	 429824092	 OK	 bfd8a98bee0840d9b871b7f6ade9908f	 rtmp://shqshne4jdp4b6.cloudfront.net/cfx/st​	key=value	 http://player.longtailvideo.com/player.swf	 http://www.longtailvideo.com/support/jw-player-setup-wizard?example=204	 LNX%2010,0,32,18	 -	 -	 -	 -
```

## Charges for Access Logs<a name="AccessLogsCharges"></a>

Access logging is an optional feature of CloudFront\. There is no extra charge for enabling access logging\. However, you accrue the usual Amazon S3 charges for storing and accessing the files on Amazon S3 \(you can delete them at any time\)\. For more information about charges for CloudFront, see [CloudFront Reports](reports.md)\.