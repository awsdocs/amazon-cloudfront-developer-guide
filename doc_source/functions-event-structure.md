# CloudFront Functions event structure<a name="functions-event-structure"></a>

CloudFront Functions passes an `event` object to your function code as input when it runs the function\. When you [test a function](test-function.md), you create the `event` object and pass it to your function\. When you create an `event` object for testing a function, you can omit the `distributionDomainName`, `distributionId`, and `requestId` fields in the `context` object\. Also, make sure that the names of headers are lowercase, which is always the case in the `event` object that CloudFront Functions passes to your function in production\.

The following shows an overview of the structure of this event object\. For more information, see the topics that follow\.

```
{
    "version": "1.0",
    "context": {
        <context object>
    },
    "viewer": {
        <viewer object>
    },
    "request": {
        <request object>
    },
    "response": {
        <response object>
    }
}
```

**Topics**
+ [Version field](#functions-event-structure-version)
+ [Context object](#functions-event-structure-context)
+ [Viewer object](#functions-event-structure-viewer)
+ [Request object](#functions-event-structure-request)
+ [Response object](#functions-event-structure-response)
+ [Query string, header, and cookie structure](#functions-event-structure-query-header-cookie)
+ [Example event](#functions-event-structure-example)

## Version field<a name="functions-event-structure-version"></a>

The `version` field contains a string that specifies the version of the CloudFront Functions event object\. The current version is `1.0`\.

## Context object<a name="functions-event-structure-context"></a>

The `context` object contains contextual information about the event\. It includes the following fields:

**`distributionDomainName`**  
The CloudFront domain name \(for example, d111111abcdef8\.cloudfront\.net\) of the distribution that’s associated with the event\.

**`distributionId`**  
The ID of the distribution \(for example, EDFDVBD6EXAMPLE\) that’s associated with the event\.

**`eventType`**  
The event type, either `viewer-request` or `viewer-response`\.

**`requestId`**  
A string that uniquely identifies a CloudFront request \(and its associated response\)\.

## Viewer object<a name="functions-event-structure-viewer"></a>

The `viewer` object contains an `ip` field whose value is the IP address of the viewer \(client\) that sent the request\. If the viewer request came through an HTTP proxy or a load balancer, the value is the IP address of the proxy or load balancer\.

## Request object<a name="functions-event-structure-request"></a>

The `request` object contains a representation of a viewer\-to\-CloudFront HTTP request\. In the `event` object that’s passed to your function, the `request` object represents the actual request that CloudFront received from the viewer\.

If your function code returns a `request` object to CloudFront, it must use this same structure\.

The `request` object contains the following fields:

**`method`**  
The HTTP method of the request\. If your function code returns a `request`, it cannot modify this field\. This is the only read\-only field in the `request` object\.

**`uri`**  
The relative path of the requested object\. If your function modifies the `uri` value, note the following:  
+ The new `uri` value must begin with a forward slash \(`/`\)\.
+ When a function changes the `uri` value, it changes the object that the viewer is requesting\.
+ When a function changes the `uri` value, it *doesn’t* change the cache behavior for the request or the origin that an origin request is sent to\.

**`querystring`**  
An object that represents the query string in the request\. If the request doesn’t include a query string, the `request` object still includes an empty `querystring` object\.  
The `querystring` object contains one field for each query string parameter in the request\.

**`headers`**  
An object that represents the HTTP headers in the request\. If the request contains any `Cookie` headers, those headers are not part of the `headers` object\. Cookies are represented separately in the `cookies` object\.  
The `headers` object contains one field for each header in the request\. Header names are converted to lowercase in the event object, and header names must be lowercase when they’re added by your function code\. When CloudFront Functions converts the event object back into an HTTP request, the first letter of each word in header names is capitalized\. Words are separated by a hyphen \(`-`\)\. For example, if your function code adds a header named `example-header-name`, CloudFront converts this to `Example-Header-Name` in the HTTP request\.

**`cookies`**  
An object that represents the cookies in the request \(`Cookie` headers\)\.  
The `cookies` object contains one field for each cookie in the request\.

For more information about the structure of query strings, headers, and cookies, see [Query string, header, and cookie structure](#functions-event-structure-query-header-cookie)\.

For an example `event` object, see [Example event](#functions-event-structure-example)\.

## Response object<a name="functions-event-structure-response"></a>

The `response` object contains a representation of a CloudFront\-to\-viewer HTTP response\. In the `event` object that’s passed to your function, the `response` object represents CloudFront’s actual response to a viewer request\.

If your function code returns a `response` object, it must use this same structure\.

The `response` object contains the following fields:

**`statusCode`**  
The HTTP status code of the response\. This value is an integer, not a string\.  
If the function is associated with a *viewer response* event type, your function code cannot change the `statusCode` that it received\. If the function is associated with a *viewer request* event type and [generates an HTTP response](writing-function-code.md#function-code-generate-response), your function code can set the `statusCode`\.

**`statusDescription`**  
The HTTP status description of the response\. If your function code generates a response, this field is optional\.

**`headers`**  
An object that represents the HTTP headers in the response\. If the response contains any `Set-Cookie` headers, those headers are not part of the `headers` object\. Cookies are represented separately in the `cookies` object\.  
The `headers` object contains one field for each header in the response\. Header names are converted to lowercase in the event object, and header names must be lowercase when they’re added by your function code\. When CloudFront Functions converts the event object back into an HTTP response, the first letter of each word in header names is capitalized\. Words are separated by a hyphen \(`-`\)\. For example, if your function code adds a header named `example-header-name`, CloudFront converts this to `Example-Header-Name` in the HTTP response\.

**`cookies`**  
An object that represents the cookies in the response \(`Set-Cookie` headers\)\.  
The `cookies` object contains one field for each cookie in the response\.

For more information about the structure of headers and cookies, see [Query string, header, and cookie structure](#functions-event-structure-query-header-cookie)\.

For an example `event` object, see [Example event](#functions-event-structure-example)\.

## Query string, header, and cookie structure<a name="functions-event-structure-query-header-cookie"></a>

Query strings, headers, and cookies in the `request` and `response` objects share the same structure\. Each query string, header, or cookie is a unique field within the parent `querystring`, `headers`, or `cookies` object\. The field name is the name of the query string, header, or cookie\. Each field contains a `value` property with the value of the query string, header, or cookie\.

For headers only, the header names are converted to lowercase in the event object, and header names must be lowercase when they’re added by your function code\. When CloudFront Functions converts the event object back into an HTTP request or response, the first letter of each word in header names is capitalized\. Words are separated by a hyphen \(`-`\)\. For example, if your function code adds a header named `example-header-name`, CloudFront converts this to `Example-Header-Name` in the HTTP request or response\.

For example, consider the following `Host` header in an HTTP request:

```
Host: video.example.com
```

This header is represented as follows in the `request` object:

```
"headers": {
    "host": {
        "value": "video.example.com"
    }
}
```

To access the `Host` header in your function code, use code like the following:

```
var request = event.request;
var host = request.headers.host.value;
```

To add or modify a header in your function code, use code like the following \(this code adds a header named `X-Custom-Header` with the value `example value`\):

```
var request = event.request;
request.headers['x-custom-header'] = {value: 'example value'};
```

### Duplicate query strings, headers, and cookies \(`multiValue` array\)<a name="functions-event-structure-multivalue"></a>

An HTTP request or response can contain more than one query string, header, or cookie with the same name\. In this case, the duplicate query strings, headers, or cookies are collapsed into one field in the `request` or `response` object, but this field contains an extra property named `multiValue`\. The `multiValue` property contains an array with the values of each of the duplicate query strings, headers, or cookies\.

For example, consider an HTTP request with the following `Accept` headers:

```
Accept: application/json
Accept: application/xml
Accept: text/html
```

These headers are represented as follows in the `request` object:

```
"headers": {
    "accept": {
        "value": "application/json",
        "multiValue": [
            {
                "value": "application/json"
            },
            {
                "value": "application/xml"
            },
            {
                "value": "text/html"
            }
        ]
    }
}
```

Note that the first header value \(in this case, `application/json`\) is repeated in both the `value` and `multiValue` properties\. This allows you to access *all* the values by looping through the `multiValue` array\.

If your function code modifies a query string, header, or cookie with a `multiValue` array, CloudFront Functions uses the following rules to apply the changes:

1. If the `multiValue` array exists and has any modification, then that modification is applied\. The first element in the `value` property is ignored\.

1. Otherwise, any modification to the `value` property is applied, and subsequent values \(if they exist\) remain unchanged\.

The `multiValue` property is used only when the HTTP request or response contains duplicate query strings, headers, or cookies with the same name, as shown in the preceding example\. However, if there are multiple values in a single query string, header, or cookie, the `multiValue` property is not used\.

For example, consider a request with one `Accept` header that contains three values, as in the following example:

```
Accept: application/json, application/xml, text/html
```

This header is represented as follows in the `request` object:

```
"headers": {
    "accept": {
        "value": "application/json, application/xml, text/html"
    }
}
```

### Cookie attributes<a name="functions-event-structure-cookie-attributes"></a>

In a `Set-Cookie` header in an HTTP response, the header contains the name–value pair for the cookie and optionally a set of attributes separated by semicolons\. For example:

```
Set-Cookie: cookie1=val1; Secure; Path=/; Domain=example.com; Expires=Wed, 05 Apr 2021 07:28:00 GMT"
```

In the `response` object, these attributes are represented in the `attributes` property of the cookie field\. For example, the preceding `Set-Cookie` header is represented as follows:

```
"cookie1": {
    "value": "val1",
    "attributes": "Secure; Path=/; Domain=example.com; Expires=Wed, 05 Apr 2021 07:28:00 GMT"
}
```

## Example event<a name="functions-event-structure-example"></a>

The following example shows a complete `event` object\.

**Note**  
The `event` object is the input to your function\. Your function returns only the `request` or `response` object, not the complete `event` object\.

```
{
  "version": "1.0",
  "context": {
    "distributionDomainName": "d111111abcdef8.cloudfront.net",
    "distributionId": "EDFDVBD6EXAMPLE",
    "eventType": "viewer-response",
    "requestId": "EXAMPLEntjQpEXAMPLE_SG5Z-EXAMPLEPmPfEXAMPLEu3EqEXAMPLE=="
  },
  "viewer": {
    "ip": "198.51.100.11"
  },
  "request": {
    "method": "GET",
    "uri": "/media/index.mpd",
    "querystring": {
      "ID": {
        "value": "42"
      },
      "Exp": {
        "value": "1619740800"
      },
      "TTL": {
        "value": "1440"
      },
      "NoValue": {
        "value": ""
      },
      "querymv": {
        "value": "val1",
        "multiValue": [
          {
            "value": "val1"
          },
          {
            "value": "val2,val3"
          }
        ]
      }
    },
    "headers": {
      "host": {
        "value": "video.example.com"
      },
      "user-agent": {
        "value": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:83.0) Gecko/20100101 Firefox/83.0"
      },
      "accept": {
        "value": "application/json",
        "multiValue": [
          {
            "value": "application/json"
          },
          {
            "value": "application/xml"
          },
          {
            "value": "text/html"
          }
        ]
      },
      "accept-language": {
        "value": "en-GB,en;q=0.5"
      },
      "accept-encoding": {
        "value": "gzip, deflate, br"
      },
      "origin": {
        "value": "https://website.example.com"
      },
      "referer": {
        "value": "https://website.example.com/videos/12345678?action=play"
      },
      "cloudfront-viewer-country": {
        "value": "GB"
      }
    },
    "cookies": {
      "Cookie1": {
        "value": "value1"
      },
      "Cookie2": {
        "value": "value2"
      },
      "cookie_consent": {
        "value": "true"
      },
      "cookiemv": {
        "value": "value3",
        "multiValue": [
          {
            "value": "value3"
          },
          {
            "value": "value4"
          }
        ]
      }
    }
  },
  "response": {
    "statusCode": 200,
    "statusDescription": "OK",
    "headers": {
      "date": {
        "value": "Mon, 04 Apr 2021 18:57:56 GMT"
      },
      "server": {
        "value": "gunicorn/19.9.0"
      },
      "access-control-allow-origin": {
        "value": "*"
      },
      "access-control-allow-credentials": {
        "value": "true"
      },
      "content-type": {
        "value": "application/json"
      },
      "content-length": {
        "value": "701"
      }
    },
    "cookies": {
      "ID": {
        "value": "id1234",
        "attributes": "Expires=Wed, 05 Apr 2021 07:28:00 GMT"
      },
      "Cookie1": {
        "value": "val1",
        "attributes": "Secure; Path=/; Domain=example.com; Expires=Wed, 05 Apr 2021 07:28:00 GMT",
        "multiValue": [
          {
            "value": "val1",
            "attributes": "Secure; Path=/; Domain=example.com; Expires=Wed, 05 Apr 2021 07:28:00 GMT"
          },
          {
            "value": "val2",
            "attributes": "Path=/cat; Domain=example.com; Expires=Wed, 10 Jan 2021 07:28:00 GMT"
          }
        ]
      }
    }
  }
}
```