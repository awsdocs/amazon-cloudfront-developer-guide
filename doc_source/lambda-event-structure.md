# Lambda@Edge Event Structure<a name="lambda-event-structure"></a>

The examples in this section show request and response events that CloudFront passes to or returns from a Lambda@Edge function when it's triggered\.

**Topics**
+ [Content\-Based Dynamic Origin Selection](#lambda-event-content-based-routing)
+ [Request Event](#lambda-event-structure-request)
+ [Response Event](#lambda-event-structure-response)

## Content\-Based Dynamic Origin Selection<a name="lambda-event-content-based-routing"></a>

You can use the path pattern in a cache behavior to route requests to an origin, based on the path and name of the requested object, such as images/\*\.jpg\. Using Lambda@Edge, you can also route requests to an origin based on other characteristics, such as the values in request headers\. 

There are a number of ways this content\-based dynamic origin selection can be useful\. For example, you can distribute requests across origins in different geographic areas to help with global load balancing\. Or you can selectively route requests to different origins that each serve a particular function: bot handling, SEO optimization, authentication, and so on\. For code samples that demonstrate how to use this feature, see [ Example Functions for Content\-Based Dynamic Origin Selection](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-examples.html#lambda-examples-content-based-routing-examples)\.

In the CloudFront origin request event, the origin element in the event structure contains information about the Amazon S3 or custom origin that the request would be routed to, based on the path pattern\. You can update the values in the origin object to route a request to a different origin\. The origin doesn't need to be defined in the distribution, and you can replace an Amazon S3 origin object with a custom origin object and vice versa\. 

You can specify either a custom origin or an Amazon S3 origin in a single request, but not both\.

## Request Event<a name="lambda-event-structure-request"></a>

Here's the format of the event object that CloudFront passes to a Lambda function when the function is triggered by a CloudFront viewer request event or an origin request event:

```
{
  "Records": [
    {
      "cf": {
        "config": {
          "distributionDomainName": "d123.cloudfront.net",
          "distributionId": "EDFDVBD6EXAMPLE",
          "eventType": "viewer-request",
          "requestId": "MRVMF7KydIvxMWfJIglgwHQwZsbG2IhRJ07sn9AkKUFSHS9EXAMPLE=="
        },
        "request": {
          "body": {
            "action": "read-only",
            "data": "eyJ1c2VybmFtZSI6IkxhbWJkYUBFZGdlIiwiY29tbWVudCI6IlRoaXMgaXMgcmVxdWVzdCBib2R5In0=",
            "encoding": "base64",
            "inputTruncated": false
          },
          "clientIp": "2001:0db8:85a3:0:0:8a2e:0370:7334",
          "querystring": "size=large",
          "uri": "/picture.jpg",
          "method": "GET",
          "headers": {
            "host": [
              {
                "key": "Host",
                "value": "d111111abcdef8.cloudfront.net"
              }
            ],
            "user-agent": [
              {
                "key": "User-Agent",
                "value": "curl/7.51.0"
              }
            ]
          },
          "origin": {
            "custom": {
              "customHeaders": {
                "my-origin-custom-header": [
                  {
                    "key": "My-Origin-Custom-Header",
                    "value": "Test"
                  }
                ]
              },
              "domainName": "example.com",
              "keepaliveTimeout": 5,
              "path": "/custom_path",
              "port": 443,
              "protocol": "https",
              "readTimeout": 5,
              "sslProtocols": [
                "TLSv1",
                "TLSv1.1"
              ]
            },
            "s3": {
              "authMethod": "origin-access-identity",
              "customHeaders": {
                "my-origin-custom-header": [
                  {
                    "key": "My-Origin-Custom-Header",
                    "value": "Test"
                  }
                ]
              },
              "domainName": "my-bucket.s3.amazonaws.com",
              "path": "/s3_path",
              "region": "us-east-1"
            }
          }
        }
      }
    }
  ]
}
```

Request events include the following values:

***Config Values***

**distributionDomainName \(read\-only\)**  
The domain name of the distribution that's associated with the request\.

**distributionID \(read\-only\)**  
The ID of the distribution that's associated with the request\.

**eventType \(read\-only\)**  
The type of trigger that's associated with the request\.

**requestId \(read\-only, viewer request events only\)**  
An encrypted string that uniquely identifies a request\. The `requestId` value also appears in CloudFront access logs as `x-edge-request-id`\. For more information, see [Configuring and Using Access Logs](AccessLogs.md) and [Web Distribution Log File Format](AccessLogs.md#BasicDistributionFileFormat)\.

***Request Values \- General***

**clientIp \(read\-only\)**  
The IP address of the viewer that made the request\. If the viewer used an HTTP proxy or a load balancer to send the request, the value is the IP address of the proxy or load balancer\.

**headers \(read/write\)**  
The headers in the request\. Note the following:  
+ The keys in the `headers` object are lowercase versions of standard HTTP header names\. Using lowercase keys gives you case\-insensitive access to the header values\.
+ Each header \(for example, `headers["accept"]` or `headers["host"]`\) is an array of key\-value pairs\. For a given header, the array contains one key\-value pair for each value in the generated response\.
+ `key` \(optional\) is the case\-sensitive name of the header as it appears in an HTTP request; for example, `accept` or `host`\.
+ Specify `value` as a header value\.
+ If you do not include the header key portion of the key\-value pair, Lambda@Edge will automatically insert a header key using the header name that you provide\. Regardless of how you've formatted the header name, the header key that is inserted automatically will be formatted with initial capitalization for each part, separated by hyphens \(\-\)\.

  For example, you can add a header like the following, without a header key: `'content-type': [{ value: 'text/html;charset=UTF-8' }]`

  In this example, Lambda@Edge creates the following header key: `Content-Type`\.
For information about restrictions on header usage, see [Headers](lambda-requirements-limits.md#lambda-header-restrictions)\.

**method \(read\-only\)**  
The HTTP method of the viewer request\.

**querystring**  
The query string, if any, that CloudFront received in the viewer request\. If the viewer request doesn't include a query string, the event structure still includes `querystring` with an empty value\. For more information about query strings, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.

**uri \(read/write\)**  
The relative path of the requested object\. Note the following:  
+ The new relative path must begin with a slash \(like this: /\)\.
+ If a function changes the URI for a request, that changes the object that the viewer is requesting\. 
+ If a function changes the URI for a request, that doesn't change the cache behavior for the request or the origin that the request is forwarded to\.

***Request Values \- Body***

**inputTruncated \(read\-only\)**  
A Boolean flag that indicates if the body was truncated by Lambda@Edge\. For more information, see [Size Limits for Body with the Include Body Option](lambda-requirements-limits.md#lambda-requirements-size-body-access)\.

**action \(read/write\)**  
The action that you intend to take with the body\. The options for `action` are the following:  
+ **read\-only:** This is the default\. When returning the response from the Lambda function, if `action` is read\-only, Lambda@Edge ignores any changes to `encoding` or `data`\.
+ **replace:** Specify this when you want to replace the body sent to the origin\.

**encoding \(read/write\)**  
The encoding for the body\. When Lambda@Edge exposes the body to the Lambda function, it first converts the body to base64 encoding\. If you choose `replace` for the `action` to replace the body, you can opt to use `text` or `base64` \(the default\) encoding\.  
If you specify `encoding` as `base64` but the body is not valid base64, CloudFront returns an error\.

**data \(read/write\)**  
The request body content\. 

***Request Values \- Custom Origin***

You can specify either a custom origin or an Amazon S3 origin in a single request; not both\.

**customHeaders**  
You can include custom headers with the request by specifying a header name and value pair for each custom header\. You can't add headers that are blacklisted for origin custom headers or hooks, and a header with the same name can't be present in request\.headers or in request\.origin\.custom\.customHeaders\. The restrictions for request\.headers also apply to custom headers\. For more information, see [Custom Headers that CloudFront Can't Forward to Your Origin](forward-custom-headers.md#forward-custom-headers-blacklist) and [Blacklisted Headers](lambda-requirements-limits.md#lambda-blacklisted-headers)\.

**domainName**  
The domain name of the origin server, like www\.example\.com\. The domain name can't be empty, can't include a colon \(:\), and can't use the IPV4 address format\. The domain name can be up to 253 characters\.

**keepaliveTimeout**  
How long, in seconds, that CloudFront should try to maintain the connection to your origin after receiving the last packet of a response\. The value must be a number in the range of 1 to 60 seconds\.

**path**  
The directory path at the server where the request should locate content\. The path should start with a slash \(/\) but should have no trailing / \(like path/\)\. The path should be URL encoded, with a maximum length of 255 characters\.

**port**  
The port at your custom origin\. The port must be 80 or 443, or a number in the range of 1024 to 65535\.

**protocol \(origin requests only\)**  
The origin protocol policy that CloudFront should use when fetching objects from your origin\. The value can be `http` or `https`\.

**readTimeout**  
How long, in seconds, CloudFront should wait for a response after forwarding a request to the origin, and how long CloudFront should wait after receiving a packet of a response before receiving the next packet\. The value must be a number in the range of 4 to 60 seconds\.

**sslProtocols**  
The SSL protocols that CloudFront can use when establishing an HTTPS connection with your origin\. Values can be the following: `TLSv1.2`, `TLSv1.1`, `TLSv1`, `SSLv3`\.

***Request Values \- Amazon S3 Origin***

You can specify either a custom origin or an Amazon S3 origin in a single request, but not both\.

**authMethod**  
Set to `origin-access-identity` if your Amazon S3 bucket has an origin access identity \(OAI\) set up, or `none` if you aren’t using OAI\. If you set authMethod to `origin-access-identity`, there are several requirements:   
+ You must specify a Region in your header\.
+ You must use the same OAI when you switch from one Amazon S3 origin to another\.
+ You can’t use an OAI when you switch from a custom origin to an Amazon S3 origin\.
For more information about using an OAI, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.

**customHeaders**  
You can include custom headers with the request by specifying a header name and value pair for each custom header\. You can't add headers that are blacklisted for origin custom headers or hooks, and a header with the same name can't be present in request\.headers or in request\.origin\.custom\.customHeaders\. The restrictions for request\.headers also apply to custom headers\. For more information, see [Custom Headers that CloudFront Can't Forward to Your Origin](forward-custom-headers.md#forward-custom-headers-blacklist) and [Blacklisted Headers](lambda-requirements-limits.md#lambda-blacklisted-headers)\.

**domainName**  
The domain name of the Amazon S3 origin server, like `my-bucket.s3.amazonaws.com`\. The domain name can't be empty, and must be an allowed bucket name \(as defined by Amazon S3\)\. Do not use a Region\-specific endpoint, like `my-bucket.s3-eu-west-1.amazonaws.com`\. The name can be up to 128 characters, and must be all lowercase\.

**path**  
The directory path at the server where the request should locate content\. The path should start with a slash \(/\) but should have no trailing / \(like path/\)\.

**region**  
The Region for your Amazon S3 bucket\. This is required only if you use OAI\.

## Response Event<a name="lambda-event-structure-response"></a>

Here's the format of the event object that CloudFront passes to a Lambda function when the function is triggered by a CloudFront viewer response event or an origin response event:

```
{
    "Records": [
        {
            "cf": {
                "config": {
                    "distributionDomainName": "d123.cloudfront.net",
                    "distributionId": "EDFDVBD6EXAMPLE",
                    "eventType": "viewer-response",
                    "requestId": "xGN7KWpVEmB9Dp7ctcVFQC4E-nrcOcEKS3QyAez--06dV7TEXAMPLE=="
                },
                "request": {
                    "clientIp": "2001:0db8:85a3:0:0:8a2e:0370:7334",
                    "method": "GET",
                    "uri": "/picture.jpg",
                    "querystring": "size=large",
                    "headers": {
                        "host": [
                            {
                                "key": "Host",
                                "value": "d111111abcdef8.cloudfront.net"
                            }
                        ],
                        "user-agent": [
                            {
                                "key": "User-Agent",
                                "value": "curl/7.18.1"
                            }
                        ]
                    }
                },
                "response": {
                    "status": "200",
                    "statusDescription": "OK",
                    "headers": {
                        "server": [
                            {
                                "key": "Server",
                                "value": "MyCustomOrigin"
                            }
                        ],
                        "set-cookie": [
                            {
                                "key": "Set-Cookie",
                                "value": "theme=light"
                            },
                            {
                                "key": "Set-Cookie",
                                "value": "sessionToken=abc123; Expires=Wed, 09 Jun 2021 10:18:14 GMT"
                            }
                        ]
                    }
                }
            }
        }
    ]
}
```

Response events include the values that appear in the corresponding request event, and in addition, the following values\. If your Lambda@Edge function generates an HTTP response, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses-in-requests.md)\.

**distributionID \(read\-only\)**  
The ID of the distribution that's associated with the request\.

**eventType \(read\-only\)**  
The type of trigger that's associated with the response\.

**headers**  
Headers that you want CloudFront to return in the generated response\. Note the following:  
+ The keys in the `headers` object are lowercase versions of standard HTTP header names\. Using lowercase keys gives you case\-insensitive access to the header values\.
+ Each header \(for example, `headers["accept"]` or `headers["host"]`\) is an array of key\-value pairs\. For a given header, the array contains one key\-value pair for each value in the generated response\.
+ `key` \(optional\) is the case\-sensitive name of the header as it appears in an HTTP request; for example, `accept` or `host`\.
+ Specify `value` as a header value\.
+ If you do not include the header key portion of the key\-value pair, Lambda@Edge will automatically insert a header key using the header name that you provide\. Regardless of how you've formatted the header name, the header key that is inserted automatically will be formatted with initial capitalization for each part, separated by hyphens \(\-\)\.

  For example, you can add a header like the following, without a header key: `'content-type': [{ value: 'text/html;charset=UTF-8' }]`

  In this example, Lambda@Edge creates the following header key: `Content-Type`\.
For information about restrictions on header usage, see [Headers](lambda-requirements-limits.md#lambda-header-restrictions)\.

**requestId \(read\-only, viewer response events only\)**  
An encrypted string that uniquely identifies a request\. The `requestId` value also appears in CloudFront access logs as the `x-edge-request-id`\. For more information, see [Configuring and Using Access Logs](AccessLogs.md) and [Web Distribution Log File Format](AccessLogs.md#BasicDistributionFileFormat)\.

**request – One of the following:**  
+ Viewer response – The request that CloudFront received from the viewer and that might have been modified by the Lambda function that was triggered by a viewer request event
+ Origin response – The request that CloudFront forwarded to the origin and that might have been modified by the Lambda function that was triggered by an origin request event
If the Lambda function modifies the request object, the changes are ignored\.

**response – One of the following:**  
+ Viewer response – The response that CloudFront will return to the viewer for viewer response events\.
+ Origin response – The response that CloudFront received from the origin for origin response events\.

**status**  
The HTTP status code that CloudFront returns to the viewer\.

**statusDescription**  
The HTTP status description that CloudFront returns to the viewer\.