# Lambda@Edge Event Structure<a name="lambda-event-structure"></a>

The following topics describe the request and response event objects that CloudFront passes to a Lambda@Edge function when it’s triggered\.

**Topics**
+ [Dynamic Origin Selection](#lambda-event-content-based-routing)
+ [Request Events](#lambda-event-structure-request)
+ [Response Events](#lambda-event-structure-response)

## Dynamic Origin Selection<a name="lambda-event-content-based-routing"></a>

You can use [the path pattern in a cache behavior](distribution-web-values-specify.md#DownloadDistValuesPathPattern) to route requests to an origin based on the path and name of the requested object, such as `images/*.jpg`\. Using Lambda@Edge, you can also route requests to an origin based on other characteristics, such as the values in request headers\. 

There are a number of ways that this dynamic origin selection can be useful\. For example, you can distribute requests across origins in different geographic areas to help with global load balancing\. Or you can selectively route requests to different origins that each serve a particular function: bot handling, SEO optimization, authentication, and so on\. For code examples that demonstrate how to use this feature, see [Content\-Based Dynamic Origin Selection \- Examples](lambda-examples.md#lambda-examples-content-based-routing-examples)\.

In the CloudFront origin request event, the `origin` object in the event structure contains information about the origin that the request would be routed to, based on the path pattern\. You can update the values in the `origin` object to route a request to a different origin\. When you update the `origin` object, you don’t need to define the origin in the distribution\. You can also replace an Amazon S3 origin object with a custom origin object, and vice versa\. You can only specify a single origin per request, though; either a custom origin or an Amazon S3 origin, but not both\.

## Request Events<a name="lambda-event-structure-request"></a>

The following topics show the structure of the object that CloudFront passes to a Lambda function for [viewer and origin request events](lambda-cloudfront-trigger-events.md)\. These examples show a `GET` request with no body\. Following the examples is a list of all the possible fields in viewer and origin request events\.

**Topics**
+ [Example Viewer Request](#example-viewer-request)
+ [Example Origin Request](#example-origin-request)
+ [Request Event Fields](#request-event-fields)

### Example Viewer Request<a name="example-viewer-request"></a>

The following example shows a viewer request event object\.

```
{
  "Records": [
    {
      "cf": {
        "config": {
          "distributionDomainName": "d111111abcdef8.cloudfront.net",
          "distributionId": "EDFDVBD6EXAMPLE",
          "eventType": "viewer-request",
          "requestId": "4TyzHTaYWb1GX1qTfsHhEqV6HUDd_BzoBZnwfnvQc_1oF26ClkoUSEQ=="
        },
        "request": {
          "clientIp": "203.0.113.178",
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
                "value": "curl/7.66.0"
              }
            ],
            "accept": [
              {
                "key": "accept",
                "value": "*/*"
              }
            ]
          },
          "method": "GET",
          "querystring": "",
          "uri": "/"
        }
      }
    }
  ]
}
```

### Example Origin Request<a name="example-origin-request"></a>

The following example shows an origin request event object\.

```
{
  "Records": [
    {
      "cf": {
        "config": {
          "distributionDomainName": "d111111abcdef8.cloudfront.net",
          "distributionId": "EDFDVBD6EXAMPLE",
          "eventType": "origin-request",
          "requestId": "4TyzHTaYWb1GX1qTfsHhEqV6HUDd_BzoBZnwfnvQc_1oF26ClkoUSEQ=="
        },
        "request": {
          "clientIp": "203.0.113.178",
          "headers": {
            "x-forwarded-for": [
              {
                "key": "X-Forwarded-For",
                "value": "203.0.113.178"
              }
            ],
            "user-agent": [
              {
                "key": "User-Agent",
                "value": "Amazon CloudFront"
              }
            ],
            "via": [
              {
                "key": "Via",
                "value": "2.0 2afae0d44e2540f472c0635ab62c232b.cloudfront.net (CloudFront)"
              }
            ],
            "host": [
              {
                "key": "Host",
                "value": "example.org"
              }
            ],
            "cache-control": [
              {
                "key": "Cache-Control",
                "value": "no-cache, cf-no-cache"
              }
            ]
          },
          "method": "GET",
          "origin": {
            "custom": {
              "customHeaders": {},
              "domainName": "example.org",
              "keepaliveTimeout": 5,
              "path": "",
              "port": 443,
              "protocol": "https",
              "readTimeout": 30,
              "sslProtocols": [
                "TLSv1",
                "TLSv1.1",
                "TLSv1.2"
              ]
            }
          },
          "querystring": "",
          "uri": "/"
        }
      }
    }
  ]
}
```

### Request Event Fields<a name="request-event-fields"></a>

Request event object data is contained in two subobjects: `config` \(`Records.cf.config`\) and `request` \(`Records.cf.request`\)\. The following lists describe each subobject’s fields\.

#### Fields in the Config Object<a name="request-event-fields-config"></a>

The following list describes the fields in the `config` object \(`Records.cf.config`\)\.

**`distributionDomainName` \(read\-only\)**  
The domain name of the distribution that’s associated with the request\.

**`distributionID` \(read\-only\)**  
The ID of the distribution that’s associated with the request\.

**`eventType` \(read\-only\)**  
The type of trigger that’s associated with the request: `viewer-request` or `origin-request`\.

**`requestId` \(read\-only\)**  
An encrypted string that uniquely identifies a viewer\-to\-CloudFront request\. The `requestId` value also appears in CloudFront access logs as `x-edge-request-id`\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md) and [Web Distribution Standard Log File Format](AccessLogs.md#BasicDistributionFileFormat)\.

#### Fields in the Request Object<a name="request-event-fields-request"></a>

The following list describes the fields in the `request` object \(`Records.cf.request`\)\.

**`clientIp` \(read\-only\)**  
The IP address of the viewer that made the request\. If the viewer used an HTTP proxy or a load balancer to send the request, the value is the IP address of the proxy or load balancer\.

**headers \(read/write\)**  
The headers in the request\. Note the following:  
+ The keys in the `headers` object are lowercase versions of standard HTTP header names\. Using lowercase keys gives you case\-insensitive access to the header values\.
+ Each header object \(for example, `headers["accept"]` or `headers["host"]`\) is an array of key–value pairs\. For a given header, the array contains one key–value pair for each value in the request\.
+ `key` contains the case\-sensitive name of the header as it appeared in the HTTP request; for example, `Host`, `User-Agent`, `X-Forwarded-For`, and so on\.
+ `value` contains the header value as it appeared in the HTTP request\.
+ When your Lambda function adds or modifies request headers and you don’t include the header `key` field, Lambda@Edge automatically inserts a header `key` using the header name that you provide\. Regardless of how you’ve formatted the header name, the header key that’s inserted automatically is formatted with initial capitalization for each part, separated by hyphens \(\-\)\.

  For example, you can add a header like the following, without a header `key`:

  ```
  "user-agent": [
    {
      "value": "ExampleCustomUserAgent/1.X.0"
    }
  ]
  ```

  In this example, Lambda@Edge automatically inserts `"key": "User-Agent"`\.
For information about restrictions on header usage, see [Headers](lambda-requirements-limits.md#lambda-header-restrictions)\.

**`method` \(read\-only\)**  
The HTTP method of the request\.

**`querystring` \(read/write\)**  
The query string, if any, in the request\. If the request doesn’t include a query string, the event object still includes `querystring` with an empty value\. For more information about query strings, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.

**`uri` \(read/write\)**  
The relative path of the requested object\. If your Lambda function modifies the `uri` value, note the following:  
+ The new `uri` value must begin with a forward slash \(/\)\.
+ When a function changes the `uri` value, that changes the object that the viewer is requesting\.
+ When a function changes the `uri` value, that *doesn’t* change the cache behavior for the request or the origin that the request is sent to\.

**`body` \(read/write\)**  
The body of the HTTP request\. The `body` structure can contain the following fields:    
**`inputTruncated` \(read\-only\)**  
A Boolean flag that indicates whether the body was truncated by Lambda@Edge\. For more information, see [Size Quotas on Request Body with the Include Body Option](lambda-requirements-limits.md#lambda-at-the-edge-body-size-limits-lambda-at-edge)\.  
**`action` \(read/write\)**  
The action that you intend to take with the body\. The options for `action` are the following:  
+ `read-only:` This is the default\. When returning the response from the Lambda function, if `action` is read\-only, Lambda@Edge ignores any changes to `encoding` or `data`\.
+ `replace:` Specify this when you want to replace the body sent to the origin\.  
**`encoding` \(read/write\)**  
The encoding for the body\. When Lambda@Edge exposes the body to the Lambda function, it first converts the body to base64\-encoding\. If you choose `replace` for the `action` to replace the body, you can opt to use `base64` \(the default\) or `text` encoding\. If you specify `encoding` as `base64` but the body is not valid base64, CloudFront returns an error\.  
**`data` \(read/write\)**  
The request body content\. 

**`origin` \(read/write\) \(origin events only\)**  
The origin to send the request to\. The `origin` structure must contain exactly one origin, which can be a custom origin or an Amazon S3 origin\. The origin structure can contain the following fields:    
**`customHeaders` \(read/write\) \(custom and Amazon S3 origins\)**  
You can include custom headers with the request by specifying a header name and value pair for each custom header\. You can’t add headers that are blacklisted, and a header with the same name can’t be present in `Records.cf.request.headers`\. The [notes about request headers](#request-event-fields-request-headers) also apply to custom headers\. For more information, see [Custom Headers that CloudFront Can’t Add to Origin Requests](add-origin-custom-headers.md#add-origin-custom-headers-blacklist) and [Blacklisted Headers](lambda-requirements-limits.md#lambda-blacklisted-headers)\.  
**`domainName` \(read/write\) \(custom and Amazon S3 origins\)**  
The domain name of the origin\. The domain name can’t be empty\.  
+ **For custom origins** – Specify a DNS domain name, such as `www.example.com`\. The domain name can’t include a colon \(:\), and can’t be an IP address\. The domain name can be up to 253 characters\.
+ **For Amazon S3 origins** – Specify the DNS domain name of the Amazon S3 bucket, such as `awsexamplebucket.s3.eu-west-1.amazonaws.com`\. The name can be up to 128 characters, and must be all lowercase\.  
**`path` \(read/write\) \(custom and Amazon S3 origins\)**  
The directory path at the origin where the request should locate content\. The path should start with a forward slash \(/\) but shouldn’t end with one \(for example, it shouldn’t end with `example-path/`\)\. For custom origins only, the path should be URL encoded and have a maximum length of 255 characters\.  
**`keepaliveTimeout` \(read/write\) \(custom origins only\)**  
How long, in seconds, that CloudFront should try to maintain the connection to the origin after receiving the last packet of the response\. The value must be a number from 1–60, inclusive\.  
**`port` \(read/write\) \(custom origins only\)**  
The port that CloudFront should connect to at your custom origin\. The port must be 80, 443, or a number in the range of 1024–65535, inclusive\.  
**`protocol` \(read/write\) \(custom origins only\)**  
The connection protocol that CloudFront should use when connecting to your origin\. The value can be `http` or `https`\.  
**`readTimeout` \(read/write\) \(custom origins only\)**  
How long, in seconds, CloudFront should wait for a response after sending a request to your origin\. This also specifies how long CloudFront should wait after receiving a packet of a response before receiving the next packet\. The value must be a number from 4–60, inclusive\.  
**`sslProtocols` \(read/write\) \(custom origins only\)**  
The minimum SSL/TLS protocol that CloudFront can use when establishing an HTTPS connection with your origin\. Values can be any of the following: `TLSv1.2`, `TLSv1.1`, `TLSv1`, or `SSLv3`\.  
**`authMethod` \(read/write\) \(Amazon S3 origins only\)**  
If you’re using an [origin access identity \(OAI\)](private-content-restricting-access-to-s3.md), set this field to `origin-access-identity`, or set it to `none` if you aren’t using an OAI\. If you set `authMethod` to `origin-access-identity`, there are several requirements:   
+ You must specify the `region` \(see the following field\)\.
+ You must use the same OAI when you change the request from one Amazon S3 origin to another\.
+ You can’t use an OAI when you change the request from a custom origin to an Amazon S3 origin\.  
**`region` \(read/write\) \(Amazon S3 origins only\)**  
The AWS Region of your Amazon S3 bucket\. This is required only when you set `authMethod` to `origin-access-identity`\.

## Response Events<a name="lambda-event-structure-response"></a>

The following topics show the structure of the object that CloudFront passes to a Lambda function for [viewer and origin response events](lambda-cloudfront-trigger-events.md)\. Following the examples is a list of all the possible fields in viewer and origin response events\.

**Topics**
+ [Example Origin Response](#lambda-event-structure-response-origin)
+ [Example Viewer Response](#lambda-event-structure-response-viewer)
+ [Response Event Fields](#response-event-fields)

### Example Origin Response<a name="lambda-event-structure-response-origin"></a>

The following example shows an origin response event object\.

```
{
  "Records": [
    {
      "cf": {
        "config": {
          "distributionDomainName": "d111111abcdef8.cloudfront.net",
          "distributionId": "EDFDVBD6EXAMPLE",
          "eventType": "origin-response",
          "requestId": "4TyzHTaYWb1GX1qTfsHhEqV6HUDd_BzoBZnwfnvQc_1oF26ClkoUSEQ=="
        },
        "request": {
          "clientIp": "203.0.113.178",
          "headers": {
            "x-forwarded-for": [
              {
                "key": "X-Forwarded-For",
                "value": "203.0.113.178"
              }
            ],
            "user-agent": [
              {
                "key": "User-Agent",
                "value": "Amazon CloudFront"
              }
            ],
            "via": [
              {
                "key": "Via",
                "value": "2.0 8f22423015641505b8c857a37450d6c0.cloudfront.net (CloudFront)"
              }
            ],
            "host": [
              {
                "key": "Host",
                "value": "example.org"
              }
            ],
            "cache-control": [
              {
                "key": "Cache-Control",
                "value": "no-cache, cf-no-cache"
              }
            ]
          },
          "method": "GET",
          "origin": {
            "custom": {
              "customHeaders": {},
              "domainName": "example.org",
              "keepaliveTimeout": 5,
              "path": "",
              "port": 443,
              "protocol": "https",
              "readTimeout": 30,
              "sslProtocols": [
                "TLSv1",
                "TLSv1.1",
                "TLSv1.2"
              ]
            }
          },
          "querystring": "",
          "uri": "/"
        },
        "response": {
          "headers": {
            "access-control-allow-credentials": [
              {
                "key": "Access-Control-Allow-Credentials",
                "value": "true"
              }
            ],
            "access-control-allow-origin": [
              {
                "key": "Access-Control-Allow-Origin",
                "value": "*"
              }
            ],
            "date": [
              {
                "key": "Date",
                "value": "Mon, 13 Jan 2020 20:12:38 GMT"
              }
            ],
            "referrer-policy": [
              {
                "key": "Referrer-Policy",
                "value": "no-referrer-when-downgrade"
              }
            ],
            "server": [
              {
                "key": "Server",
                "value": "ExampleCustomOriginServer"
              }
            ],
            "x-content-type-options": [
              {
                "key": "X-Content-Type-Options",
                "value": "nosniff"
              }
            ],
            "x-frame-options": [
              {
                "key": "X-Frame-Options",
                "value": "DENY"
              }
            ],
            "x-xss-protection": [
              {
                "key": "X-XSS-Protection",
                "value": "1; mode=block"
              }
            ],
            "content-type": [
              {
                "key": "Content-Type",
                "value": "text/html; charset=utf-8"
              }
            ],
            "content-length": [
              {
                "key": "Content-Length",
                "value": "9593"
              }
            ]
          },
          "status": "200",
          "statusDescription": "OK"
        }
      }
    }
  ]
}
```

### Example Viewer Response<a name="lambda-event-structure-response-viewer"></a>

The following example shows a viewer response event object\.

```
{
  "Records": [
    {
      "cf": {
        "config": {
          "distributionDomainName": "d111111abcdef8.cloudfront.net",
          "distributionId": "EDFDVBD6EXAMPLE",
          "eventType": "viewer-response",
          "requestId": "4TyzHTaYWb1GX1qTfsHhEqV6HUDd_BzoBZnwfnvQc_1oF26ClkoUSEQ=="
        },
        "request": {
          "clientIp": "203.0.113.178",
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
                "value": "curl/7.66.0"
              }
            ],
            "accept": [
              {
                "key": "accept",
                "value": "*/*"
              }
            ]
          },
          "method": "GET",
          "querystring": "",
          "uri": "/"
        },
        "response": {
          "headers": {
            "access-control-allow-credentials": [
              {
                "key": "Access-Control-Allow-Credentials",
                "value": "true"
              }
            ],
            "access-control-allow-origin": [
              {
                "key": "Access-Control-Allow-Origin",
                "value": "*"
              }
            ],
            "date": [
              {
                "key": "Date",
                "value": "Mon, 13 Jan 2020 20:14:56 GMT"
              }
            ],
            "referrer-policy": [
              {
                "key": "Referrer-Policy",
                "value": "no-referrer-when-downgrade"
              }
            ],
            "server": [
              {
                "key": "Server",
                "value": "ExampleCustomOriginServer"
              }
            ],
            "x-content-type-options": [
              {
                "key": "X-Content-Type-Options",
                "value": "nosniff"
              }
            ],
            "x-frame-options": [
              {
                "key": "X-Frame-Options",
                "value": "DENY"
              }
            ],
            "x-xss-protection": [
              {
                "key": "X-XSS-Protection",
                "value": "1; mode=block"
              }
            ],
            "age": [
              {
                "key": "Age",
                "value": "2402"
              }
            ],
            "content-type": [
              {
                "key": "Content-Type",
                "value": "text/html; charset=utf-8"
              }
            ],
            "content-length": [
              {
                "key": "Content-Length",
                "value": "9593"
              }
            ]
          },
          "status": "200",
          "statusDescription": "OK"
        }
      }
    }
  ]
}
```

### Response Event Fields<a name="response-event-fields"></a>

Response event object data is contained in three subobjects: `config` \(`Records.cf.config`\), `request` \(`Records.cf.request`\), and `response` \(`Records.cf.response`\)\. For more information about the fields in the request object, see [Fields in the Request Object](#request-event-fields-request)\. The following lists describe the fields in the `config` and `response` subobjects\.

#### Fields in the Config Object<a name="response-event-fields-config"></a>

The following list describes the fields in the `config` object \(`Records.cf.config`\)\.

**`distributionDomainName` \(read\-only\)**  
The domain name of the distribution that’s associated with the response\.

**`distributionID` \(read\-only\)**  
The ID of the distribution that’s associated with the response\.

**`eventType` \(read\-only\)**  
The type of trigger that’s associated with the response: `origin-response` or `viewer-response`\.

**`requestId` \(read\-only\)**  
An encrypted string that uniquely identifies the viewer\-to\-CloudFront request that this response is associated with\. The `requestId` value also appears in CloudFront access logs as `x-edge-request-id`\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md) and [Web Distribution Standard Log File Format](AccessLogs.md#BasicDistributionFileFormat)\.

#### Fields in the Response Object<a name="response-event-fields-response"></a>

The following list describes the fields in the `response` object \(`Records.cf.response`\)\. For information about using a Lambda@Edge function to generate an HTTP response, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses-in-requests.md)\.

**`headers` \(read/write\)**  
The headers in the response\. Note the following:  
+ The keys in the `headers` object are lowercase versions of standard HTTP header names\. Using lowercase keys gives you case\-insensitive access to the header values\.
+ Each header object \(for example, `headers["content-type"]` or `headers["content-length"]`\) is an array of key–value pairs\. For a given header, the array contains one key–value pair for each value in the response\.
+ `key` contains the case\-sensitive name of the header as it appears in the HTTP response; for example, `Content-Type`, `Content-Length`, and so on\.
+ `value` contains the header value as it appears in the HTTP response\.
+ When your Lambda function adds or modifies response headers and you don’t include the header `key` field, Lambda@Edge automatically inserts a header `key` using the header name that you provide\. Regardless of how you’ve formatted the header name, the header key that’s inserted automatically is formatted with initial capitalization for each part, separated by hyphens \(\-\)\.

  For example, you can add a header like the following, without a header `key`:

  ```
  "content-type": [
    {
      "value": "text/html;charset=UTF-8"
    }
  ]
  ```

  In this example, Lambda@Edge automatically inserts `"key": "Content-Type"`\.
For information about restrictions on header usage, see [Headers](lambda-requirements-limits.md#lambda-header-restrictions)\.

**`status`**  
The HTTP status code of the response\.

**`statusDescription`**  
The HTTP status description of the response\.