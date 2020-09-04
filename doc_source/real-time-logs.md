# Real\-time logs<a name="real-time-logs"></a>

With CloudFront real\-time logs, you can get information about requests made to a distribution in real time \(logs are delivered within seconds of receiving the requests\)\. You can use real\-time logs to monitor, analyze, and take action based on content delivery performance\.

CloudFront real\-time logs are configurable\. You can choose:
+ The *sampling rate* for your real\-time logs—that is, the percentage of requests for which you want to receive real\-time log records\.
+ The specific fields that you want to receive in the log records\.
+ The specific cache behaviors \(path patterns\) that you want to receive real\-time logs for\.

CloudFront real\-time logs are delivered to the data stream of your choice in Amazon Kinesis Data Streams\. You can build your own [Kinesis data stream consumer](https://docs.aws.amazon.com/streams/latest/dev/amazon-kinesis-consumers.html), or use Amazon Kinesis Data Firehose to send the log data to Amazon Simple Storage Service \(Amazon S3\), Amazon Redshift, Amazon Elasticsearch Service \(Amazon ES\), or a third\-party log processing service\.

CloudFront charges for real\-time logs, in addition to the charges you incur for using Kinesis Data Streams\. For more information about pricing, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/) and [Amazon Kinesis Data Streams pricing](http://aws.amazon.com/kinesis/data-streams/pricing/)\.

## Understanding real\-time log configurations<a name="understand-real-time-log-config"></a>

To use CloudFront real\-time logs, you start by creating a real\-time log configuration\. The real\-time log configuration contains information about which log fields you want to receive, the *sampling rate* for log records, and the Kinesis data stream where you want to deliver the logs\.

Specifically, a real\-time log configuration contains the following settings:
+ [Name](#understand-real-time-log-config-name)
+ [Sampling rate](#understand-real-time-log-config-sampling-rate)
+ [Fields](#understand-real-time-log-config-fields)
+ [Endpoint \(Kinesis data stream\)](#understand-real-time-log-config-endpoint)
+ [IAM role](#understand-real-time-log-config-iam-role)

**Name**  
A name to identify the real\-time log configuration\.

**Sampling rate**  
The sampling rate is a whole number between 1 and 100 \(inclusive\) that determines the percentage of viewer requests that are sent to Kinesis Data Streams as real\-time log records\. To include every viewer request in your real\-time logs, specify 100 for the sampling rate\. You might choose a lower sampling rate to reduce costs while still receiving a representative sample of request data in your real\-time logs\.

**Fields**  
A list of the fields that are included in each real\-time log record\. You can choose to receive all of the available fields, or only the fields that you need for monitoring and analyzing performance\.  
The following list describes each of the available fields\. The fields are listed in the order in which they appear in the log records that are delivered to Kinesis Data Streams\.
+ **`timestamp`** – The date and time at which the edge server finished responding to the request\.
+ **`c-ip`** – The IP address of the viewer that made the request, for example, 192\.0\.2\.183 or 2001:0db8:85a3:0000:0000:8a2e:0370:7334\. If the viewer used an HTTP proxy or a load balancer to send the request, the value of this field is the IP address of the proxy or load balancer\.
+ `time-to-first-byte` – The number of seconds between receiving the request and writing the first byte of the response, as measured on the edge server\.
+ `sc-status` – Contains one of the following values:
  + The HTTP status code of the edge server’s response \(for example, `200`\)\.
  + `000`, which indicates that the viewer closed the connection before the edge server could respond to the request\. If the viewer closes the connection after the edge server starts to send the response, this field contains the applicable HTTP status code\.
+ `sc-bytes` – The total number of bytes that the edge server sent to the viewer in response to the request, including headers\.
+ `cs-method` – The HTTP request method received from the viewer\.
+ `cs-protocol` – The protocol of the viewer request \(`http`, `https`, `ws`, or `wss`\)\.
+ `cs-host` – The domain name of the CloudFront distribution \(for example, d111111abcdef8\.cloudfront\.net\)\.
+ `cs-uri-stem` – The portion of the request URL that identifies the path and object \(for example, /images/cat\.jpg\)\. Question marks \(?\) in URLs and query strings are not included in the log\.
+ `cs-bytes` – The total number of bytes of data that the viewer included in the request, including headers\.
+ `x-edge-location` – The edge location that served the request\. Each edge location is identified by a three\-letter code and an arbitrarily assigned number \(for example, DFW3\)\. The three\-letter code typically corresponds with the International Air Transport Association \(IATA\) airport code for an airport near the edge location’s geographic location\. \(These abbreviations might change in the future\.\)
+ `x-edge-request-id` – An opaque string that uniquely identifies a request\. This string is also sent in the `x-amz-cf-id` response header\.
+ `x-host-header` – The value that the viewer included in the `Host` header of the request\. If you’re using the CloudFront domain name in your object URLs \(such as d111111abcdef8\.cloudfront\.net\), this field contains that domain name\. If you’re using alternate domain names \(CNAMEs\) in your object URLs \(such as www\.example\.com\), this field contains the alternate domain name\.
+ `time-taken` – The number of seconds \(to the thousandth of a second, for example, 0\.082\) from when the edge server receives the viewer’s request to when the edge server writes the last byte of the response to the output queue, as measured on the edge server\. From the perspective of the viewer, the total time to get the full response will be longer than this value because of network latency andTCP buffering\.
+ `cs-protocol-version` – The HTTP version that the viewer specified in the request\. Possible values include `HTTP/0.9`, `HTTP/1.0`, `HTTP/1.1`, and `HTTP/2.0`\.
+ `c-ip-version` – The IP version of the request \(IPv4 or IPv6\)\.
+ `cs-user-agent` – The value of the `User-Agent` header in the request\. The `User-Agent` header identifies the source of the request, such as the type of device and browser that submitted the request or, if the request came from a search engine, which search engine\.
+ `cs-referer` – The value of the `Referer` header in the request\. This is the name of the domain that originated the request\. Common referrers include search engines, other websites that link directly to your objects, and your own website\.
+ `cs-cookie` – The `Cookie` header in the request, including name—value pairs and the associated attributes\.
**Note**  
This field is truncated to 800 bytes\.
+ `cs-uri-query` – The query string portion of the URL, if any\.
+ `x-edge-response-result-type` – How the edge server classified the response just before returning the response to the viewer\. Possible values include:
  + `Hit` – The edge server served the object to the viewer from the cache\.
  + `RefreshHit` – The edge server found the object in the edge cache but the object had expired, so the edge server contacted the origin to verify that the cache had the latest version of the object\.
  + `Miss` – The request could not be satisfied by an object in the edge cache, so the edge server forwarded the request to the origin server and returned the result to the viewer\.
  + `LimitExceeded` – The request was denied because a CloudFront quota \(formerly referred to as a limit\) was exceeded\.
  + `CapacityExceeded` – The edge server returned a 503 error because it didn’t have enough capacity at the time of the request to serve the object\.
  + `Error` – Typically, this means the request resulted in a client error or a server error\.
  + `Redirect` – The edge server redirected the viewer from HTTP to HTTPS according to the distribution settings\.
+ `x-forwarded-for` – If the viewer used an HTTP proxy or a load balancer to send the request, the value of the `c-ip` field is the IP address of the proxy or load balancer\. In that case, this field is the IP address of the viewer that originated the request\.
+ `ssl-protocol` – When the request used HTTPS, this field contains the SSL/TLS protocol that the viewer and edge server negotiated for transmitting the request and response\. For a list of possible values, see the supported SSL/TLS protocols in [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)\.
+ `ssl-cipher` – When the request used HTTPS, this field contains the SSL/TLS cipher that the viewer and edge server negotiated for encrypting the request and response\. For a list of possible values, see the supported SSL/TLS protocols in [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md)\.
+ `x-edge-result-type` – How the edge server classified the response after the last byte left the edge location\. In some cases, the result type can change between the time that the edge server is ready to send the response and the time that it finishes sending the response\. \(See also the `x-edge-response-result-type` field\)\.

  For example, in HTTP streaming, suppose the edge server finds a segment of the stream in the edge cache\. In that scenario, the value of this field would ordinarily be `Hit`\. However, if the viewer closes the connection before the edge server has delivered the entire segment, the final result type \(and the value of this field\) is `Error`\.

  Possible values include:
  + `Hit` – The edge server served the object to the viewer from the cache\.
  + `RefreshHit` – The edge server found the object in the edge cache but the object had expired, so the edge server contacted the origin to verify that the cache had the latest version of the object\.
  + `Miss` – The request could not be satisfied by an object in the edge cache, so the edge server forwarded the request to the origin server and returned the result to the viewer\.
  + `LimitExceeded` – The request was denied because a CloudFront quota \(formerly referred to as a limit\) was exceeded\.
  + `CapacityExceeded` – The edge server returned a 503 error because it didn’t have enough capacity at the time of the request to serve the object\.
  + `Error` – Typically, this means the request resulted in a client error or a server error\.
  + `Redirect` – The edge server redirected the viewer from HTTP to HTTPS according to the distribution settings\.
+ `fle-encrypted-fields` – The number of [field\-level encryption](field-level-encryption.md) fields that the edge server encrypted and forwarded to the origin\. Edge servers stream the processed request to the origin as they encrypt data, so this field can have a value even if the value of `fle-status` is an error\.
+ `fle-status` – When [field\-level encryption](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/field-level-encryption.html) is configured for a distribution, this field contains a code that indicates whether the request body was successfully processed\. When the edge server successfully processes the request body, encrypts values in the specified fields, and forwards the request to the origin, the value of this field is `Processed`\. The value of `x-edge-result-type` can still indicate a client\-side or server\-side error in this case\.

  If the request exceeds a field\-level encryption quota \(formerly referred to as a limit\), this field contains one of the following error codes, and the edge server returns HTTP status code 400 to the viewer\.
  + `FieldLengthLimitClientError` – A field that is configured to be encrypted exceeded the maximum length allowed\.
  + `FieldNumberLimitClientError` – A request that the distribution is configured to encrypt contains more than the number of fields allowed\.
  + `RequestLengthLimitClientError` – The length of the request body exceeded the maximum length allowed when field\-level encryption is configured\.

  Other possible values for this field include the following:
  + `ForwardedByContentType` – The edge server forwarded the request to the origin without parsing or encryption because no content type was configured\.
  + `ForwardedByQueryArgs` – The edge server forwarded the request to the origin without parsing or encryption because the request contains a query argument that wasn’t in the configuration for field\-level encryption\.
  + `ForwardedDueToNoProfile` – The edge server forwarded the request to the origin without parsing or encryption because no profile was specified in the configuration for field\-level encryption\.
  + `MalformedContentTypeClientError` – The edge server rejected the request and returned an HTTP 400 status code to the viewer because the value of the `Content-Type` header was in an invalid format\.
  + `MalformedInputClientError` – The edge server rejected the request and returned an HTTP 400 status code to the viewer because the request body was in an invalid format\.
  + `MalformedQueryArgsClientError` – The edge server rejected the request and returned an HTTP 400 status code to the viewer because a query argument was empty or in an invalid format\.
  + `RejectedByContentType` – The edge server rejected the request and returned an HTTP 400 status code to the viewer because no content type was specified in the configuration for field\-level encryption\.
  + `RejectedByQueryArgs` – The edge server rejected the request and returned an HTTP 400 status code to the viewer because no query argument was specified in the configuration for field\-level encryption\.
  + `ServerError` – The origin server returned an error\.
+ `sc-content-type` – The value of the HTTP `Content-Type` header of the response\.
+ `sc-content-len` – The value of the HTTP `Content-Length` header of the response\.
+ `sc-range-start` – When the response contains the HTTP `Content-Range` header, this field contains the range start value\.
+ `sc-range-end` – When the response contains the HTTP `Content-Range` header, this field contains the range end value\.
+ `c-port` – The port number of the request from the viewer\.
+ `x-edge-detailed-result-type` – When the `x-edge-result-type` field is not `Error`, this field contains the same value as `x-edge-result-type`\. When `x-edge-result-type` is `Error`, this field contains the specific type of error\. Possible values include:
  + `AbortedOrigin` – The edge server encountered an issue with the origin\.
  + `ClientCommError` – The response to the viewer was interrupted due to a communication problem between the edge server and the viewer\.
  + `ClientGeoBlocked` – The distribution is configured to refuse requests from the viewer’s geographic location\.
  + `ClientHungUpRequest` – The viewer stopped prematurely while sending the request\.
  + `Error` – An error occurred for which the error type doesn’t fit any of the other categories\. This error type can occur when the edge server serves an error response from the cache\.
  + `InvalidRequest` – The edge server received an invalid request from the viewer\.
  + `InvalidRequestBlocked` – Access to the requested resource is blocked\.
  + `InvalidRequestCertificate` – The distribution doesn’t match the SSL/TLS certificate for which the HTTPS connection was established\.
  + `InvalidRequestHeader` – The request contained an invalid header\.
  + `InvalidRequestMethod` – The distribution is not configured to handle the HTTP request method that was used\. This can happen when the distribution supports only cacheable requests\.
  + `OriginConnectError` – The edge server couldn’t connect to the origin\.
  + `OriginContentRangeLengthError` – The `Content-Length` header in the origin’s response doesn’t match the length in the `Content-Range` header\.
  + `OriginDnsError` – The edge server couldn’t resolve the origin’s domain name\.
  + `OriginError` – The origin returned an incorrect response\.
  + `OriginHeaderTooBigError` – A header returned by the origin is too big for the edge server to process\.
  + `OriginInvalidResponseError` – The origin returned an invalid response\.
  + `OriginReadError` – The edge server couldn’t read from the origin\.
  + `OriginWriteError` – The edge server couldn’t write to the origin\.
  + `OriginZeroSizeObjectError` – A zero size object sent from the origin resulted in an error\.
  + `SlowReaderOriginError` – The viewer was slow to read the message that caused the origin error\.
+ `c-country` – A country code that represents the viewer’s geographic location, as determined by the viewer’s IP address\.
+ `cs-accept-encoding` – The value of the `Accept-Encoding` header in the viewer request\.
+ `cs-accept` – The value of the `Accept` header in the viewer request\.
+ `cache-behavior-path-pattern` – The path pattern that identifies the cache behavior that matched the viewer request\.
+ `cs-headers` – The HTTP headers \(names and values\) in the viewer request\.
**Note**  
This field is truncated to 800 bytes\.
+ `cs-header-names` – The names of the HTTP headers \(not values\) in the viewer request\.
**Note**  
This field is truncated to 800 bytes\.
+ `cs-headers-count` – The number of HTTP headers in the viewer request\.

**Endpoint \(Kinesis data stream\)**  
The endpoint contains information about the Kinesis data stream where you want to send real\-time logs\. You provide the Amazon Resource Name \(ARN\) of the data stream\.  
For more information about creating a Kinesis data stream, see the following topics in the *Amazon Kinesis Data Streams Developer Guide*\.  
+ [Managing Streams Using the Console](https://docs.aws.amazon.com/streams/latest/dev/managing-streams-console.html)
+ [Perform Basic Kinesis Data Stream Operations Using the AWS CLI](https://docs.aws.amazon.com/streams/latest/dev/fundamental-stream.html)
+ [Creating a Stream](https://docs.aws.amazon.com/streams/latest/dev/kinesis-using-sdk-java-create-stream.html) \(uses the AWS SDK for Java\)
When you create a data stream, you need to specify the number of shards\. Use the following information to help you estimate the number of shards you need\.  

**To estimate the number of shards for your Kinesis data stream**

1. Calculate \(or estimate\) the number of requests per second that your CloudFront distribution receives\.

   You can use the [CloudFront usage reports](https://console.aws.amazon.com/cloudfront/home#usage_reports:) \(in the CloudFront console\) and the [CloudFront metrics](viewing-cloudfront-metrics.md#monitoring-console.distributions) \(in the CloudFront and Amazon CloudWatch consoles\) to help you calculate your requests per second\.

1. Determine the typical size of a single real\-time log record\.

   In general, a single log record is around 500 bytes\. A large record that includes all available fields is around 1 KB\.

   If you’re not sure what your log record size is, you can enable real\-time logs with a low sampling rate \(for example, 1%\), and then calculate the average record size using monitoring data in Kinesis Data Streams \(total number of records divided by total incoming bytes\)\.

1. Multiply the number of requests per second \(from step 1\) by the size of a typical real\-time log record \(from step 2\) to determine the amount of data per second that your real\-time log configuration is likely to send to the Kinesis data stream\.

1. Using the data per second, calculate the number of shards that you need\. A single shard can handle no more than 1 MB per second and 1,000 requests \(log records\) per second\. When calculating the number of shards that you need, we recommend adding 10\-25% as a buffer\.
For example, assume your distribution receives 50,000 requests per second, and that your real\-time log records size is typically 500 bytes\. This means that your real\-time log configuration could generate 25,000,000 bytes \(50,000 multiplied by 500\), or 23\.84 MB, per second\. In this scenario you would need at least 24 shards\. To add a buffer of about 20%, you would specify 29 shards\.

**IAM role**  
The AWS Identity and Access Management \(IAM\) role that gives CloudFront permission to deliver real\-time logs to your Kinesis data stream\.  
When you create a real\-time log configuration with the CloudFront console, you can choose **Create new service role** to let the console create the IAM role for you\.  
When you create a real\-time log configuration with AWS CloudFormation or the CloudFront API \(AWS CLI or SDK\), you must create the IAM role yourself and provide the role ARN\. To create the IAM yourself, use the following policies\.  
**IAM role trust policy**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
**IAM role permissions policy for an unencrypted data stream**  
To use the following policy, replace *Kinesis data stream ARN* with the ARN of your Kinesis data stream\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kinesis:DescribeStreamSummary",
                "kinesis:DescribeStream",
                "kinesis:PutRecord",
                "kinesis:PutRecords"
            ],
            "Resource": [
                "Kinesis data stream ARN"
            ]
        }
    ]
}
```
**IAM role permissions policy for an encrypted data stream**  
To use the following policy, replace *Kinesis data stream ARN* with the ARN of your Kinesis data stream and *AWS KMS key* with the ARN of your customer master key in AWS Key Management Service \(AWS KMS\)\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kinesis:DescribeStreamSummary",
                "kinesis:DescribeStream",
                "kinesis:PutRecord",
                "kinesis:PutRecords"
            ],
            "Resource": [
                "Kinesis data stream ARN"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:GenerateDataKey"
            ],
            "Resource": [
                "AWS KMS key"
            ]
        }
    ]
}
```

## Creating and using real\-time log configurations<a name="create-real-time-log-config"></a>

You can use a real\-time log configurations to get information about requests made to a distribution in real time \(logs are delivered within seconds of receiving the requests\)\. You can create a real\-time log configuration in the CloudFront console, with the AWS Command Line Interface \(AWS CLI\), or with the CloudFront API\.

To use a real\-time log configuration, you attach it to one or more cache behaviors in a CloudFront distribution\.

### Create a real\-time log configuration \(console\)<a name="create-real-time-log-config-console"></a>

**To create a real\-time log configuration \(console\)**

1. Sign in to the AWS Management Console and open the **Logs** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home?#/logs](https://console.aws.amazon.com/cloudfront/v2/home?#/logs)\.

1. Choose **Real\-time log configurations**\.

1. Choose **Create configuration**\.

1. Choose the desired setting for the real\-time log configuration\. Note the following:
   + By default, all **Fields** are chosen\. To remove a field, do one of the following:
     + Use the **Choose fields** drop\-down menu to remove the selection from the fields that you don’t want to include in the real\-time log configuration\.
     + Use the expand button \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/console-expand-button.png)\) to view all fields, then use the remove button \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/console-remove-button.png)\) to remove the fields that you don’t want to include in the real\-time log configuration\.
   + For **IAM role**, you can choose **Create new service role** to let the console create the IAM role for you\. You must have permission to create IAM roles\.
   + You can use the setting in the **Distribution** section to choose a CloudFront distribution and cache behavior to attach to the real\-time log configuration\.

   For more information, see [Understanding real\-time log configurations](#understand-real-time-log-config)\.

1. When finished, choose **Create configuration**\.

If successful, the console shows the details of the real\-time log configuration that you just created\.

### Create a real\-time log configuration \(AWS CLI\)<a name="create-real-time-log-config-cli"></a>

To create a real\-time log configuration with the AWS Command Line Interface \(AWS CLI\), use the aws cloudfront create\-realtime\-log\-config command\. You can use an input file to provide the command’s input parameters, rather than specifying each individual parameter as command line input\.

**To create a real\-time log configuration \(CLI with input file\)**

1. Use the following command to create a file named `rtl-config.yaml` that contains all of the input parameters for the create\-realtime\-log\-config command\.

   ```
   aws cloudfront create-realtime-log-config --generate-cli-skeleton yaml-input > rtl-config.yaml
   ```
**Note**  
The `yaml-input` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate an input file in JSON format\. For more information, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `rtl-config.yaml` that you just created\. Edit the file to specify the real\-time log configuration settings that you want, then save the file\. Note the following:
   + For `StreamType`, the only valid value is `Kinesis`\.

   For more information about the real\-time long configuration settings, see [Understanding real\-time log configurations](#understand-real-time-log-config)\.

1. Use the following command to create the real\-time log configuration using input parameters from the `rtl-config.yaml` file\.

   ```
   aws cloudfront create-realtime-log-config --cli-input-yaml file://rtl-config.yaml
   ```

If successful, the command’s output shows the details of the real\-time log configuration that you just created\.

**To attach a real\-time log configuration to an existing distribution \(CLI with input file\)**

1. Use the following command to save the distribution configuration for the CloudFront distribution that you want to update\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront get-distribution-config --id distribution_ID --output yaml > dist-config.yaml
   ```
**Note**  
The `--output yaml` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate the output in JSON format\. For more information, see [Controlling command output from the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `dist-config.yaml` that you just created\. Edit the file, making the following changes to each cache behavior that you are updating to use a real\-time log configuration\.
   + In the cache behavior, add a field named `RealtimeLogConfigArn`\. For the field’s value, use the ARN of the real\-time log configuration that you want to attach to this cache behavior\.
   + Rename the `ETag` field to `IfMatch`, but don’t change the field’s value\.

   Save the file when finished\.

1. Use the following command to update the distribution to use the real\-time log configuration\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront update-distribution --id distribution_ID --cli-input-yaml file://dist-config.yaml
   ```

If successful, the command’s output shows the details of the distribution that you just updated\.

### Create a real\-time log configuration \(API\)<a name="create-real-time-log-config-api"></a>

To create a real\-time log configuration with the CloudFront API, use [CreateRealtimeLogConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateRealtimeLogConfig.html)\. For more information about the parameters that you specify in this API call, see [Understanding real\-time log configurations](#understand-real-time-log-config) and the API reference documentation for your AWS SDK or other API client\.

After you create a real\-time log configuration, you can attach it to a cache behavior, using one of the following API calls:
+ To attach it to a cache behavior in an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a cache behavior in a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the ARN of the real\-time log configuration in the `RealtimeLogConfigArn` field, inside a cache behavior\. For more information about the other fields that you specify in these API calls, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.

## Creating a Kinesis Data Streams consumer<a name="real-time-log-consumer-guidance"></a>

To read and analyze your real\-time logs, you build or use a Kinesis Data Streams *consumer*\. When you build a consumer for CloudFront real\-time logs, it’s important to know that the fields in every real\-time log record are always delivered in the same order, as listed in the [Fields](#understand-real-time-log-config-fields) section\. Make sure that you build your consumer to accommodate this fixed order\.

For example, consider a real\-time log configuration that includes only these three fields: `time-to-first-byte`, `sc-status`, and `c-country`\. In this scenario, the last field, `c-country`, is always field number 3 in every log record\. However, if you later add fields to the real\-time log configuration, the placement of each field in a record can change\.

For example, if you add the fields `sc-bytes` and `time-taken` to the real\-time log configuration, these fields are inserted into each log record according to the order shown in the [Fields](#understand-real-time-log-config-fields) section\. The resulting order of all five fields is `time-to-first-byte`, `sc-status`, `sc-bytes`, `time-taken`, and `c-country`\. The `c-country` field was originally field number 3, but is now field number 5\. Make sure that your consumer application can handle fields that change position in a log record, in case you add fields to your real\-time log configuration\.

## Troubleshooting real\-time logs<a name="real-time-log-troubleshooting"></a>

After you create a real\-time log configuration, you might find that no records \(or not all records\) are delivered to Kinesis Data Streams\. In this case, you should first verify that your CloudFront distribution is receiving viewer requests\. If it is, you can check the following setting to continue troubleshooting\.

**IAM role permissions**  
To deliver real\-time log records to your Kinesis data stream, CloudFront uses the IAM role in the real\-time log configuration\. Make sure that the role trust policy and the role permissions policy match the policies shown in [IAM role](#understand-real-time-log-config-iam-role)\.

**Kinesis Data Streams throttling**  
If CloudFront writes real\-time log records to your Kinesis data stream faster than the stream can handle, Kinesis Data Streams might throttle the requests from CloudFront\. In this case, you can increase the number of shards in your Kinesis data stream\. Each shard can support writes up to 1,000 records per second, up to a maximum data write of 1 MB per second\.