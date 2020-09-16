# Lambda@Edge Example Functions<a name="lambda-examples"></a>

See the following sections for examples of using Lambda functions with CloudFront\.

**Note**  
For Node\.js functions, each function must call the `callback` parameter to successfully process a request or return a response\. For more information, see [Writing and Creating a Lambda@Edge Function](lambda-edge-create-function.md)\.

**Topics**
+ [General Examples](#lambda-examples-general-examples)
+ [Generating Responses \- Examples](#lambda-examples-generated-response-examples)
+ [Working with Query Strings \- Examples](#lambda-examples-query-string-examples)
+ [Personalize Content by Country or Device Type Headers \- Examples](#lambda-examples-redirecting-examples)
+ [Content\-Based Dynamic Origin Selection \- Examples](#lambda-examples-content-based-routing-examples)
+ [Updating Error Statuses \- Examples](#lambda-examples-update-error-status-examples)
+ [Accessing the Request Body \- Examples](#lambda-examples-access-request-body-examples)

## General Examples<a name="lambda-examples-general-examples"></a>

The examples in this section illustrate some common ways to use Lambda@Edge in CloudFront\.

**Topics**
+ [Example: A/B Testing](#lambda-examples-a-b-testing)
+ [Example: Overriding a Response Header](#lambda-examples-overriding-response-header)

### Example: A/B Testing<a name="lambda-examples-a-b-testing"></a>

You can use the following example to test two different versions of an image without creating redirects or changing the URL\. This example reads the cookies in the viewer request and modifies the request URL accordingly\. If the viewer doesn’t send a cookie with one of the expected values, the example randomly assigns the viewer to one of the URLs\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    const headers = request.headers;

    if (request.uri !== '/experiment-pixel.jpg') {
        // do not process if this is not an A-B test request
        callback(null, request);
        return;
    }

    const cookieExperimentA = 'X-Experiment-Name=A';
    const cookieExperimentB = 'X-Experiment-Name=B';
    const pathExperimentA = '/experiment-group/control-pixel.jpg';
    const pathExperimentB = '/experiment-group/treatment-pixel.jpg';

    /*
     * Lambda at the Edge headers are array objects.
     *
     * Client may send multiple Cookie headers, i.e.:
     * > GET /viewerRes/test HTTP/1.1
     * > User-Agent: curl/7.18.1 (x86_64-unknown-linux-gnu) libcurl/7.18.1 OpenSSL/1.0.1u zlib/1.2.3
     * > Cookie: First=1; Second=2
     * > Cookie: ClientCode=abc
     * > Host: example.com
     *
     * You can access the first Cookie header at headers["cookie"][0].value
     * and the second at headers["cookie"][1].value.
     *
     * Header values are not parsed. In the example above,
     * headers["cookie"][0].value is equal to "First=1; Second=2"
     */
    let experimentUri;
    if (headers.cookie) {
        for (let i = 0; i < headers.cookie.length; i++) {
            if (headers.cookie[i].value.indexOf(cookieExperimentA) >= 0) {
                console.log('Experiment A cookie found');
                experimentUri = pathExperimentA;
                break;
            } else if (headers.cookie[i].value.indexOf(cookieExperimentB) >= 0) {
                console.log('Experiment B cookie found');
                experimentUri = pathExperimentB;
                break;
            }
        }
    }

    if (!experimentUri) {
        console.log('Experiment cookie has not been found. Throwing dice...');
        if (Math.random() < 0.75) {
            experimentUri = pathExperimentA;
        } else {
            experimentUri = pathExperimentB;
        }
    }

    request.uri = experimentUri;
    console.log(`Request uri set to "${request.uri}"`);
    callback(null, request);
};
```

------
#### [ Python ]

```
 import json
 import random
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
     headers = request['headers']
 
     if request['uri'] != '/experiment-pixel.jpg':
         # Not an A/B Test
         return request
 
     cookieExperimentA, cookieExperimentB = 'X-Experiment-Name=A', 'X-Experiment-Name=B'
     pathExperimentA, pathExperimentB = '/experiment-group/control-pixel.jpg', '/experiment-group/treatment-pixel.jpg'
 
     '''
     Lambda at the Edge headers are array objects.
     
     Client may send multiple cookie headers. For example:
     > GET /viewerRes/test HTTP/1.1
     > User-Agent: curl/7.18.1 (x86_64-unknown-linux-gnu) libcurl/7.18.1 OpenSSL/1.0.1u zlib/1.2.3
     > Cookie: First=1; Second=2
     > Cookie: ClientCode=abc
     > Host: example.com
     
     You can access the first Cookie header at headers["cookie"][0].value
     and the second at headers["cookie"][1].value.
     
     Header values are not parsed. In the example above,
     headers["cookie"][0].value is equal to "First=1; Second=2"
     '''
 
     experimentUri = ""
 
     for cookie in headers.get('cookie', []):
         if cookieExperimentA in cookie['value']:
             print("Experiment A cookie found")
             experimentUri = pathExperimentA
             break
         elif cookieExperimentB in cookie['value']:
             print("Experiment B cookie found")
             experimentUri = pathExperimentB
             break
 
     if not experimentUri:
         print("Experiment cookie has not been found. Throwing dice...")
         if random.random() < 0.75:
             experimentUri = pathExperimentA
         else:
             experimentUri = pathExperimentB
 
     request['uri'] = experimentUri
     print(f"Request uri set to {experimentUri}")
     return request
```

------

### Example: Overriding a Response Header<a name="lambda-examples-overriding-response-header"></a>

The following example shows how to change the value of a response header based on the value of another header\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
   const response = event.Records[0].cf.response;
   const headers = response.headers;

   const headerNameSrc = 'X-Amz-Meta-Last-Modified';
   const headerNameDst = 'Last-Modified';

   if (headers[headerNameSrc.toLowerCase()]) {
      headers[headerNameDst.toLowerCase()] = [
         headers[headerNameSrc.toLowerCase()][0],
      ];
      console.log(`Response header "${headerNameDst}" was set to ` +
               `"${headers[headerNameDst.toLowerCase()][0].value}"`);
   }

   callback(null, response);
};
```

------
#### [ Python ]

```
 import json
 
 def lambda_handler(event, context):
     response = event["Records"][0]["cf"]["response"]
     headers = response["headers"]
 
     headerNameSrc = "X-Amz-Meta-Last-Modified"
     headerNameDst = "Last-Modified"
 
     if headers.get(headerNameSrc.lower(), None):
         headers[headerNameDst.lower()] = [headers[headerNameSrc.lower()][0]]
         print(f"Response header {headerNameDst.lower()} was set to {headers[headerNameSrc.lower()][0]}")
 
     return response
```

------

## Generating Responses \- Examples<a name="lambda-examples-generated-response-examples"></a>

The examples in this section show how you can use Lambda@Edge to generate responses\.

**Topics**
+ [Example: Serving Static Content \(Generated Response\)](#lambda-examples-static-web-server)
+ [Example: Generating an HTTP Redirect \(Generated Response\)](#lambda-examples-http-redirect)

### Example: Serving Static Content \(Generated Response\)<a name="lambda-examples-static-web-server"></a>

The following example shows how to use a Lambda function to serve static website content, which reduces the load on the origin server and reduces overall latency\. 

**Note**  
You can generate HTTP responses for viewer request and origin request events\. For more information, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses-in-requests.md)\. You can also replace the HTTP response in origin and viewer response events\. For more information, see [Updating HTTP Responses in Origin\-Response Triggers](lambda-updating-http-responses.md)\.

------
#### [ Node\.js ]

```
'use strict';

const content = `
<\!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Simple Lambda@Edge Static Content Response</title>
  </head>
  <body>
    <p>Hello from Lambda@Edge!</p>
  </body>
</html>
`;

exports.handler = (event, context, callback) => {
    /*
     * Generate HTTP OK response using 200 status code with HTML body.
     */
    const response = {
        status: '200',
        statusDescription: 'OK',
        headers: {
            'cache-control': [{
                key: 'Cache-Control',
                value: 'max-age=100'
            }],
            'content-type': [{
                key: 'Content-Type',
                value: 'text/html'
            }]
        },
        body: content,
    };
    callback(null, response);
};
```

------
#### [ Python ]

```
 import json
 
 CONTENT = """
 <\!DOCTYPE html>
 <html lang="en">
   <head>
     <meta charset="utf-8">
     <title>Simple Lambda@Edge Static Content Response</title>
   </head>
   <body>
     <p>Hello from Lambda@Edge!</p>
   </body>
 </html>
 """
 
 def lambda_handler(event, context):
     # Generate HTTP OK response using 200 status code with HTML body.
     response = {
         'status': '200',
         'statusDescription': 'OK',
         'headers': {
             'cache-control': [
                 {
                     'key': 'Cache-Control',
                     'value': 'max-age=100'
                 }
             ],
             "content-type": [
                 {
                     'key': 'Content-Type',
                     'value': 'text/html'
                 }
             ]
         },
         'body': CONTENT
     }
     return response
```

------

### Example: Generating an HTTP Redirect \(Generated Response\)<a name="lambda-examples-http-redirect"></a>

The following example shows how to generate an HTTP redirect\.

**Note**  
You can generate HTTP responses for viewer request and origin request events\. For more information, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses-in-requests.md)\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
    /*
     * Generate HTTP redirect response with 302 status code and Location header.
     */
    const response = {
        status: '302',
        statusDescription: 'Found',
        headers: {
            location: [{
                key: 'Location',
                value: 'http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html',
            }],
        },
    };
    callback(null, response);
};
```

------
#### [ Python ]

```
 def lambda_handler(event, context):
 
     # Generate HTTP redirect response with 302 status code and Location header.
     
     response = {
         'status': '302',
         'statusDescription': 'Found',
         'headers': {
             'location': [{
                 'key': 'Location',
                 'value': 'http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html'
             }]
         }
     }
     
     return response
```

------

## Working with Query Strings \- Examples<a name="lambda-examples-query-string-examples"></a>

The examples in this section include ways that you can use Lambda@Edge with query strings\.

**Topics**
+ [Example: Adding a Header Based on a Query String Parameter](#lambda-examples-header-based-on-query-string)
+ [Example: Normalizing Query String Parameters to Improve the Cache Hit Ratio](#lambda-examples-normalize-query-string-parameters)
+ [Example: Redirecting Unauthenticated Users to a Sign\-In Page](#lambda-examples-redirect-to-signin-page)

### Example: Adding a Header Based on a Query String Parameter<a name="lambda-examples-header-based-on-query-string"></a>

The following example shows how to get the key\-value pair of a query string parameter, and then add a header based on those values\.

------
#### [ Node\.js ]

```
'use strict';

const querystring = require('querystring');
exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    
    /* When a request contains a query string key-value pair but the origin server
     * expects the value in a header, you can use this Lambda function to
     * convert the key-value pair to a header. Here's what the function does:
     * 1. Parses the query string and gets the key-value pair.
     * 2. Adds a header to the request using the key-value pair that the function got in step 1.
     */

    /* Parse request querystring to get javascript object */
    const params = querystring.parse(request.querystring);

    /* Move auth param from querystring to headers */
    const headerName = 'Auth-Header';
    request.headers[headerName.toLowerCase()] = [{ key: headerName, value: params.auth }];
    delete params.auth;

    /* Update request querystring */
    request.querystring = querystring.stringify(params);

    callback(null, request);
}

;
```

------
#### [ Python ]

```
 from urllib.parse import parse_qs, urlencode
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
 
     '''
     When a request contains a query string key-value pair but the origin server
     expects the value in a header, you can use this Lambda function to
     convert the key-value pair to a header. Here's what the function does:
         1. Parses the query string and gets the key-value pair.
         2. Adds a header to the request using the key-value pair that the function got in step 1.
     '''
 
     # Parse request querystring to get dictionary/json
     params = {k : v[0] for k, v in parse_qs(request['querystring']).items()}
 
     # Move auth param from querystring to headers
     headerName = 'Auth-Header'
     request['headers'][headerName.lower()] = [{'key': headerName, 'value': params['auth']}]
     del params['auth']
 
     # Update request querystring
     request['querystring'] = urlencode(params)
     
     return request
```

------

### Example: Normalizing Query String Parameters to Improve the Cache Hit Ratio<a name="lambda-examples-normalize-query-string-parameters"></a>

The following example shows how to improve your cache hit ratio by making the following changes to query strings before CloudFront forwards requests to your origin:
+ Alphabetize key\-value pairs by the name of the parameter\.
+ Change the case of key\-value pairs to lowercase\.

For more information, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\.

------
#### [ Node\.js ]

```
'use strict';

const querystring = require('querystring');

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    /* When you configure a distribution to forward query strings to the origin and
     * to cache based on a whitelist of query string parameters, we recommend
     * the following to improve the cache-hit ratio:
     * - Always list parameters in the same order.
     * - Use the same case for parameter names and values.
     *
     * This function normalizes query strings so that parameter names and values
     * are lowercase and parameter names are in alphabetical order.
     *
     * For more information, see:
     * http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/QueryStringParameters.html
     */

    console.log('Query String: ', request.querystring);

    /* Parse request query string to get javascript object */
    const params = querystring.parse(request.querystring.toLowerCase());
    const sortedParams = {};

    /* Sort param keys */
    Object.keys(params).sort().forEach(key => {
        sortedParams[key] = params[key];
    });

    /* Update request querystring with normalized  */
    request.querystring = querystring.stringify(sortedParams);

    callback(null, request);
};
```

------
#### [ Python ]

```
 from urllib.parse import parse_qs, urlencode
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
     '''
     When you configure a distribution to forward query strings to the origin and
     to cache based on a whitelist of query string parameters, we recommend
     the following to improve the cache-hit ratio:
     Always list parameters in the same order.
     - Use the same case for parameter names and values.
     
     This function normalizes query strings so that parameter names and values
     are lowercase and parameter names are in alphabetical order.
     
     For more information, see:
     http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/QueryStringParameters.html
     '''
     print("Query string: ", request["querystring"])
 
     # Parse request query string to get js object
     params = {k : v[0] for k, v in parse_qs(request['querystring'].lower()).items()}
 
     # Sort param keys
     sortedParams = sorted(params.items(), key=lambda x: x[0])
 
     # Update request querystring with normalized
     request['querystring'] = urlencode(sortedParams)
     
     return request
```

------

### Example: Redirecting Unauthenticated Users to a Sign\-In Page<a name="lambda-examples-redirect-to-signin-page"></a>

The following example shows how to redirect users to a sign\-in page if they haven't entered their credentials\.

------
#### [ Node\.js ]

```
'use strict';

function parseCookies(headers) {
    const parsedCookie = {};
    if (headers.cookie) {
        headers.cookie[0].value.split(';').forEach((cookie) => {
            if (cookie) {
                const parts = cookie.split('=');
                parsedCookie[parts[0].trim()] = parts[1].trim();
            }
        });
    }
    return parsedCookie;
}

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    const headers = request.headers;

    /* Check for session-id in request cookie in viewer-request event,
     * if session-id is absent, redirect the user to sign in page with original
     * request sent as redirect_url in query params.
     */

    /* Check for session-id in cookie, if present then proceed with request */
    const parsedCookies = parseCookies(headers);
    if (parsedCookies && parsedCookies['session-id']) {
        callback(null, request);
        return;
    }

    /* URI encode the original request to be sent as redirect_url in query params */
    const encodedRedirectUrl = encodeURIComponent(`https://${headers.host[0].value}${request.uri}?${request.querystring}`);
    const response = {
        status: '302',
        statusDescription: 'Found',
        headers: {
            location: [{
                key: 'Location',
                value: `https://www.example.com/signin?redirect_url=${encodedRedirectUrl}`,
            }],
        },
    };
    callback(null, response);
};
```

------
#### [ Python ]

```
import urllib

def parseCookies(headers):
    parsedCookie = {}
    if headers.get('cookie'):
        for cookie in headers['cookie'][0]['value'].split(';'):
            if cookie:
                parts = cookie.split('=')
                parsedCookie[parts[0].strip()] = parts[1].strip()
    return parsedCookie

def lambda_handler(event, context):
    request = event['Records'][0]['cf']['request']
    headers = request['headers']

    '''
    Check for session-id in request cookie in viewer-request event,
    if session-id is absent, redirect the user to sign in page with original
    request sent as redirect_url in query params.
    '''

    # Check for session-id in cookie, if present, then proceed with request
    parsedCookies = parseCookies(headers)

    if parsedCookies and parsedCookies['session-id']:
        return request

    # URI encode the original request to be sent as redirect_url in query params
    redirectUrl = "https://%s%s?%s" % (headers['host'][0]['value'], request['uri'], request['querystring'])
    encodedRedirectUrl = urllib.parse.quote_plus(redirectUrl.encode('utf-8'))
    
    response = {
        'status': '302',
        'statusDescription': 'Found',
        'headers': {
            'location': [{
                'key': 'Location',
                'value': 'https://www.example.com/signin?redirect_url=%s' % encodedRedirectUrl
            }]
        }
    }
    return response
```

------

## Personalize Content by Country or Device Type Headers \- Examples<a name="lambda-examples-redirecting-examples"></a>

The examples in this section illustrate how you can use Lambda@Edge to customize behavior based on location or the type of device used by the viewer\.

**Topics**
+ [Example: Redirecting Viewer Requests to a Country\-Specific URL](#lambda-examples-redirect-based-on-country)
+ [Example: Serving Different Versions of an Object Based on the Device](#lambda-examples-vary-on-device-type)

### Example: Redirecting Viewer Requests to a Country\-Specific URL<a name="lambda-examples-redirect-based-on-country"></a>

The following example shows how to generate an HTTP redirect response with a country\-specific URL and return the response to the viewer\. This is useful when you want to provide country\-specific responses\. For example:
+ If you have country\-specific subdomains, such as us\.example\.com and tw\.example\.com, you can generate a redirect response when a viewer requests example\.com\.
+ If you're streaming video but you don't have rights to stream the content in a specific country, you can redirect users in that country to a page that explains why they can't view the video\. 

Note the following:
+ You must configure your distribution to cache based on the `CloudFront-Viewer-Country` header\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\.
+ CloudFront adds the `CloudFront-Viewer-Country` header after the viewer request event\. To use this example, you must create a trigger for the origin request event\.

------
#### [ Node\.js ]

```
'use strict';

/* This is an origin request function */
exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    const headers = request.headers;

    /*
     * Based on the value of the CloudFront-Viewer-Country header, generate an
     * HTTP status code 302 (Redirect) response, and return a country-specific
     * URL in the Location header.
     * NOTE: 1. You must configure your distribution to cache based on the
     *          CloudFront-Viewer-Country header. For more information, see
     *          http://docs.aws.amazon.com/console/cloudfront/cache-on-selected-headers
     *       2. CloudFront adds the CloudFront-Viewer-Country header after the viewer
     *          request event. To use this example, you must create a trigger for the
     *          origin request event.
     */

    let url = 'https://example.com/';
    if (headers['cloudfront-viewer-country']) {
        const countryCode = headers['cloudfront-viewer-country'][0].value;
        if (countryCode === 'TW') {
            url = 'https://tw.example.com/';
        } else if (countryCode === 'US') {
            url = 'https://us.example.com/';
        }
    }

    const response = {
        status: '302',
        statusDescription: 'Found',
        headers: {
            location: [{
                key: 'Location',
                value: url,
            }],
        },
    };
    callback(null, response);
};
```

------
#### [ Python ]

```
 # This is an origin request function
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
     headers = request['headers']
 
     '''
      Based on the value of the CloudFront-Viewer-Country header, generate an
      HTTP status code 302 (Redirect) response, and return a country-specific
      URL in the Location header.
      NOTE: 1. You must configure your distribution to cache based on the
               CloudFront-Viewer-Country header. For more information, see
               http://docs.aws.amazon.com/console/cloudfront/cache-on-selected-headers
            2. CloudFront adds the CloudFront-Viewer-Country header after the viewer
               request event. To use this example, you must create a trigger for the
               origin request event.
     '''
 
     url = 'https://example.com/'
     viewerCountry = headers.get('cloudfront-viewer-country')
     if viewerCountry:
         countryCode = viewerCountry[0]['value']
         if countryCode == 'TW':
             url = 'https://tw.example.com/'
         elif countryCode == 'US':
             url = 'https://us.example.com/'
 
     response = {
         'status': '302',
         'statusDescription': 'Found',
         'headers': {
             'location': [{
                 'key': 'Location',
                 'value': url
             }]
         }
     }
 
     return response
```

------

### Example: Serving Different Versions of an Object Based on the Device<a name="lambda-examples-vary-on-device-type"></a>

The following example shows how to serve different versions of an object based on the type of device that the user is using, for example, a mobile device or a tablet\. Note the following:
+ You must configure your distribution to cache based on the `CloudFront-Is-*-Viewer` headers\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\.
+ CloudFront adds the `CloudFront-Is-*-Viewer` headers after the viewer request event\. To use this example, you must create a trigger for the origin request event\.

------
#### [ Node\.js ]

```
'use strict';

/* This is an origin request function */
exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    const headers = request.headers;

    /*
     * Serve different versions of an object based on the device type.
     * NOTE: 1. You must configure your distribution to cache based on the
     *          CloudFront-Is-*-Viewer headers. For more information, see
     *          the following documentation:
     *          http://docs.aws.amazon.com/console/cloudfront/cache-on-selected-headers
     *          http://docs.aws.amazon.com/console/cloudfront/cache-on-device-type
     *       2. CloudFront adds the CloudFront-Is-*-Viewer headers after the viewer
     *          request event. To use this example, you must create a trigger for the
     *          origin request event.
     */

    const desktopPath = '/desktop';
    const mobilePath = '/mobile';
    const tabletPath = '/tablet';
    const smarttvPath = '/smarttv';

    if (headers['cloudfront-is-desktop-viewer']
        && headers['cloudfront-is-desktop-viewer'][0].value === 'true') {
        request.uri = desktopPath + request.uri;
    } else if (headers['cloudfront-is-mobile-viewer']
               && headers['cloudfront-is-mobile-viewer'][0].value === 'true') {
        request.uri = mobilePath + request.uri;
    } else if (headers['cloudfront-is-tablet-viewer']
               && headers['cloudfront-is-tablet-viewer'][0].value === 'true') {
        request.uri = tabletPath + request.uri;
    } else if (headers['cloudfront-is-smarttv-viewer']
               && headers['cloudfront-is-smarttv-viewer'][0].value === 'true') {
        request.uri = smarttvPath + request.uri;
    }
    console.log(`Request uri set to "${request.uri}"`);

    callback(null, request);
};
```

------
#### [ Python ]

```
# This is an origin request function
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
     headers = request['headers']
 
     '''
      Serve different versions of an object based on the device type.
      NOTE: 1. You must configure your distribution to cache based on the
               CloudFront-Is-*-Viewer headers. For more information, see
               the following documentation:
               http://docs.aws.amazon.com/console/cloudfront/cache-on-selected-headers
               http://docs.aws.amazon.com/console/cloudfront/cache-on-device-type
            2. CloudFront adds the CloudFront-Is-*-Viewer headers after the viewer
               request event. To use this example, you must create a trigger for the
               origin request event.
     '''
 
     desktopPath = '/desktop';
     mobilePath = '/mobile';
     tabletPath = '/tablet';
     smarttvPath = '/smarttv';
 
     if 'cloudfront-is-desktop-viewer' in headers and headers['cloudfront-is-desktop-viewer'][0]['value'] == 'true':
         request['uri'] = desktopPath + request['uri']
     elif 'cloudfront-is-mobile-viewer' in headers and headers['cloudfront-is-mobile-viewer'][0]['value'] == 'true':
         request['uri'] = mobilePath + request['uri']
     elif 'cloudfront-is-tablet-viewer' in headers and headers['cloudfront-is-tablet-viewer'][0]['value'] == 'true':
         request['uri'] = tabletPath + request['uri']
     elif 'cloudfront-is-smarttv-viewer' in headers and headers['cloudfront-is-smarttv-viewer'][0]['value'] == 'true':
         request['uri'] = smarttvPath + request['uri']
 
     print("Request uri set to %s" % request['uri'])
 
     return request
```

------

## Content\-Based Dynamic Origin Selection \- Examples<a name="lambda-examples-content-based-routing-examples"></a>

The examples in this section show how you can use Lambda@Edge to route to different origins based on information in the request\.

**Topics**
+ [Example: Using an Origin\-Request Trigger to Change From a Custom Origin to an Amazon S3 Origin](#lambda-examples-content-based-S3-origin-based-on-query)
+ [Example: Using an Origin\-Request Trigger to Change the Amazon S3 Origin Region](#lambda-examples-content-based-S3-origin-request-trigger)
+ [Example: Using an Origin\-Request Trigger to Change From an Amazon S3 Origin to a Custom Origin](#lambda-examples-content-based-custom-origin-request-trigger)
+ [Example: Using an Origin\-Request Trigger to Gradually Transfer Traffic From One Amazon S3 Bucket to Another](#lambda-examples-content-based-gradual-traffic-transfer)
+ [Example: Using an Origin\-Request Trigger to Change the Origin Domain Name Based on the Country Header](#lambda-examples-content-based-geo-header)

### Example: Using an Origin\-Request Trigger to Change From a Custom Origin to an Amazon S3 Origin<a name="lambda-examples-content-based-S3-origin-based-on-query"></a>

This function demonstrates how an origin\-request trigger can be used to change from a custom origin to an Amazon S3 origin from which the content is fetched, based on request properties\.

------
#### [ Node\.js ]

```
'use strict';

 const querystring = require('querystring');
 
 exports.handler = (event, context, callback) => {
     const request = event.Records[0].cf.request;
 
     /**
      * Reads query string to check if S3 origin should be used, and
      * if true, sets S3 origin properties.
      */
 
     const params = querystring.parse(request.querystring);
 
     if (params['useS3Origin']) {
         if (params['useS3Origin'] === 'true') {
             const s3DomainName = 'my-bucket.s3.amazonaws.com';
 
             /* Set S3 origin fields */
             request.origin = {
                 s3: {
                     domainName: s3DomainName,
                     region: '',
                     authMethod: 'none',
                     path: '',
                     customHeaders: {}
                 }
             };
             request.headers['host'] = [{ key: 'host', value: s3DomainName}];
         }
     }
     
    callback(null, request);
};
```

------
#### [ Python ]

```
 from urllib.parse import parse_qs
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
     '''
     Reads query string to check if S3 origin should be used, and
     if true, sets S3 origin properties
     '''
     params = {k: v[0] for k, v in parse_qs(request['querystring']).items()}
     if params.get('useS3Origin') == 'true':
         s3DomainName = 'my-bucket.s3.amazonaws.com'
 
         # Set S3 origin fields
         request['origin'] = {
             's3': {
                 'domainName': s3DomainName,
                 'region': '',
                 'authMethod': 'none',
                 'path': '',
                 'customHeaders': {}
             }
         }
         request['headers']['host'] = [{'key': 'host', 'value': s3DomainName}]
     return request
```

------

### Example: Using an Origin\-Request Trigger to Change the Amazon S3 Origin Region<a name="lambda-examples-content-based-S3-origin-request-trigger"></a>

This function demonstrates how an origin\-request trigger can be used to change the Amazon S3 origin from which the content is fetched, based on request properties\.

In this example, we use the value of the CloudFront\-Viewer\-Country header to update the S3 bucket domain name to a bucket in a Region that is closer to the viewer\. This can be useful in several ways:
+ It reduces latencies when the Region specified is nearer to the viewer's country\.
+ It provides data sovereignty by making sure that data is served from an origin that's in the same country that the request came from\.

To use this example, you must do the following:
+ Configure your distribution to cache based on the CloudFront\-Viewer\-Country header\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\. 
+ Create a trigger for this function in the origin request event\. CloudFront adds the CloudFront\-Viewer\-Country header after the viewer request event, so to use this example, you must make sure that the function executes for an origin request\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;

    /**
     * This blueprint demonstrates how an origin-request trigger can be used to
     * change the origin from which the content is fetched, based on request properties.
     * In this example, we use the value of the CloudFront-Viewer-Country header
     * to update the S3 bucket domain name to a bucket in a Region that is closer to
     * the viewer.
     * 
     * This can be useful in several ways:
     *      1) Reduces latencies when the Region specified is nearer to the viewer’s
     *         country.
     *      2) Provides data sovereignty by making sure that data is served from an
     *         origin that’s in the same country that the request came from.
     * 
     * NOTE: 1. You must configure your distribution to cache based on the
     *          CloudFront-Viewer-Country header. For more information, see
     *          http://docs.aws.amazon.com/console/cloudfront/cache-on-selected-headers
     *       2. CloudFront adds the CloudFront-Viewer-Country header after the viewer
     *          request event. To use this example, you must create a trigger for the
     *          origin request event.
     */

    const countryToRegion = {
        'DE': 'eu-central-1',
        'IE': 'eu-west-1',
        'GB': 'eu-west-2',
        'FR': 'eu-west-3',
        'JP': 'ap-northeast-1',
        'IN': 'ap-south-1'
    };

    if (request.headers['cloudfront-viewer-country']) {
        const countryCode = request.headers['cloudfront-viewer-country'][0].value;
        const region = countryToRegion[countryCode];
        
        /**
         * If the viewer's country is not in the list you specify, the request
         * goes to the default S3 bucket you've configured.
         */  
        if (region) {
            /**
             * If you’ve set up OAI, the bucket policy in the destination bucket
             * should allow the OAI GetObject operation, as configured by default
             * for an S3 origin with OAI. Another requirement with OAI is to provide
             * the Region so it can be used for the SIGV4 signature. Otherwise, the
             * Region is not required.
             */
            request.origin.s3.region = region;
            const domainName = `my-bucket-in-${region}.s3.amazonaws.com`;
            request.origin.s3.domainName = domainName;
            request.headers['host'] = [{ key: 'host', value: domainName }];
        }
    }

    callback(null, request);
};
```

------
#### [ Python ]

```
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
 
     '''
     This blueprint demonstrates how an origin-request trigger can be used to
     change the origin from which the content is fetched, based on request properties.
     In this example, we use the value of the CloudFront-Viewer-Country header
     to update the S3 bucket domain name to a bucket in a Region that is closer to
     the viewer.
      
     This can be useful in several ways:
         1) Reduces latencies when the Region specified is nearer to the viewer’s
              country.
         2) Provides data sovereignty by making sure that data is served from an
              origin that’s in the same country that the request came from.
      
     NOTE: 1. You must configure your distribution to cache based on the
              CloudFront-Viewer-Country header. For more information, see
              http://docs.aws.amazon.com/console/cloudfront/cache-on-selected-headers
           2. CloudFront adds the CloudFront-Viewer-Country header after the viewer
              request event. To use this example, you must create a trigger for the
              origin request event.
     '''
 
     countryToRegion = {
         'DE': 'eu-central-1',
         'IE': 'eu-west-1',
         'GB': 'eu-west-2',
         'FR': 'eu-west-3',
         'JP': 'ap-northeast-1',
         'IN': 'ap-south-1'
     }
 
     viewerCountry = request['headers'].get('cloudfront-viewer-country')
     if viewerCountry:
         countryCode = viewerCountry[0]['value']
         region = countryToRegion.get(countryCode)
 
         # If the viewer's country in not in the list you specify, the request
         # goes to the default S3 bucket you've configured
         if region:
             '''
             If you’ve set up OAI, the bucket policy in the destination bucket
             should allow the OAI GetObject operation, as configured by default
             for an S3 origin with OAI. Another requirement with OAI is to provide
             the Region so it can be used for the SIGV4 signature. Otherwise, the
             Region is not required.
             '''
             request['origin']['s3']['region'] = region
             domainName = 'my-bucket-in-%s.s3.amazonaws.com' % region
             request['origin']['s3']['domainName'] = domainName
             request['headers']['host'] = [{'key': 'host', 'value': domainName}]
 
     return request
```

------

### Example: Using an Origin\-Request Trigger to Change From an Amazon S3 Origin to a Custom Origin<a name="lambda-examples-content-based-custom-origin-request-trigger"></a>

This function demonstrates how an origin\-request trigger can be used to change the custom origin from which the content is fetched, based on request properties\.

------
#### [ Node\.js ]

```
'use strict';

const querystring = require('querystring');
 
 exports.handler = (event, context, callback) => {
     const request = event.Records[0].cf.request;
 
     /**
      * Reads query string to check if custom origin should be used, and
      * if true, sets custom origin properties.
      */
 
     const params = querystring.parse(request.querystring);
 
     if (params['useCustomOrigin']) {
         if (params['useCustomOrigin'] === 'true') {
 
             /* Set custom origin fields*/
             request.origin = {
                 custom: {
                     domainName: 'www.example.com',
                     port: 443,
                     protocol: 'https',
                     path: '',
                     sslProtocols: ['TLSv1', 'TLSv1.1'],
                     readTimeout: 5,
                     keepaliveTimeout: 5,
                     customHeaders: {}
                 }
             };
             request.headers['host'] = [{ key: 'host', value: 'www.example.com'}];
         }
     }
    callback(null, request);
};
```

------
#### [ Python ]

```
from urllib.parse import parse_qs
 
 def lambda_handler(event, context): 
     request = event['Records'][0]['cf']['request']
 
     # Reads query string to check if custom origin should be used, and
     # if true, sets custom origin properties
 
     params = {k: v[0] for k, v in parse_qs(request['querystring']).items()}
 
     if params.get('useCustomOrigin') == 'true':
             # Set custom origin fields
             request['origin'] = {
                 'custom': {
                     'domainName': 'www.example.com',
                     'port': 443,
                     'protocol': 'https',
                     'path': '',
                     'sslProtocols': ['TLSv1', 'TLSv1.1'],
                     'readTimeout': 5,
                     'keepaliveTimeout': 5,
                     'customHeaders': {}
                 }
             }
             request['headers']['host'] = [{'key': 'host', 'value': 'www.example.com'}]
             
     return request
```

------

### Example: Using an Origin\-Request Trigger to Gradually Transfer Traffic From One Amazon S3 Bucket to Another<a name="lambda-examples-content-based-gradual-traffic-transfer"></a>

This function demonstrates how you can gradually transfer traffic from one Amazon S3 bucket to another, in a controlled way\.

------
#### [ Node\.js ]

```
'use strict';

    function getRandomInt(min, max) {
        /* Random number is inclusive of min and max*/
        return Math.floor(Math.random() * (max - min + 1)) + min;
 }

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;
    const BLUE_TRAFFIC_PERCENTAGE = 80;

    /**
      * This Lambda function demonstrates how to gradually transfer traffic from
      * one S3 bucket to another in a controlled way.
      * We define a variable BLUE_TRAFFIC_PERCENTAGE which can take values from
      * 1 to 100. If the generated randomNumber less than or equal to BLUE_TRAFFIC_PERCENTAGE, traffic
      * is re-directed to blue-bucket. If not, the default bucket that we've configured
      * is used.
      */

    const randomNumber = getRandomInt(1, 100);

if (randomNumber <= BLUE_TRAFFIC_PERCENTAGE) {
         const domainName = 'blue-bucket.s3.amazonaws.com';
         request.origin.s3.domainName = domainName;
         request.headers['host'] = [{ key: 'host', value: domainName}];
     }
    callback(null, request);
};
```

------
#### [ Python ]

```
 import math
 import random
 
 def getRandomInt(min, max):
     # Random number is inclusive of min and max
     return math.floor(random.random() * (max - min + 1)) + min
 
 def lambda_handler(min, max):
     request = event['Records'][0]['cf']['request']
     BLUE_TRAFFIC_PERCENTAGE = 80
 
     '''
     This Lambda function demonstrates how to gradually transfer traffic from
     one S3 bucket to another in a controlled way.
     We define a variable BLUE_TRAFFIC_PERCENTAGE which can take values from
     1 to 100. If the generated randomNumber less than or equal to BLUE_TRAFFIC_PERCENTAGE, traffic
     is re-directed to blue-bucket. If not, the default bucket that we've configured
     is used.
     '''
 
     randomNumber = getRandomInt(1, 100)
 
     if randomNumber <= BLUE_TRAFFIC_PERCENTAGE:
         domainName = 'blue-bucket.s3.amazonaws.com'
         request['origin']['s3']['domainName'] = domainName
         request['heaaders']['host'] = [{'key': 'host', 'value': domainName}]
 
     return request
```

------

### Example: Using an Origin\-Request Trigger to Change the Origin Domain Name Based on the Country Header<a name="lambda-examples-content-based-geo-header"></a>

This function demonstrates how you can change the origin domain name based on the CloudFront\-Viewer\-Country header, so content is served from an origin closer to the viewer's country\.

Implementing this functionality for your distribution can have advantages such as the following:
+ Reducing latencies when the Region specified is nearer to the viewer's country
+ Providing data sovereignty by making sure that data is served from an origin that's in the same country that the request came from

Note that to enable this functionality you must configure your distribution to cache based on the CloudFront\-Viewer\-Country header\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
     const request = event.Records[0].cf.request;
     
  if (request.headers['cloudfront-viewer-country']) {
         const countryCode = request.headers['cloudfront-viewer-country'][0].value;
         if (countryCode === 'GB' || countryCode === 'DE' || countryCode === 'IE' ) {
             const domainName = 'eu.example.com';
             request.origin.custom.domainName = domainName;
             request.headers['host'] = [{key: 'host', value: domainName}];
         } 
     }
     
    callback(null, request);
};
```

------
#### [ Python ]

```
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
 
     viewerCountry = request['headers'].get('cloudfront-viewer-country')
     if viewerCountry:
         countryCode = viewerCountry[0]['value']
         if countryCode == 'GB' or countryCode == 'DE' or countryCode == 'IE':
             domainName = 'eu.example.com'
             request['origin']['custom']['domainName'] = domainName
             request['headers']['host'] = [{'key': 'host', 'value': domainName}]
     return request
```

------

## Updating Error Statuses \- Examples<a name="lambda-examples-update-error-status-examples"></a>

The examples in this section provide guidance for how you can use Lambda@Edge to change the error status that is returned to users\.

**Topics**
+ [Example: Using an Origin\-Response Trigger to Update the Error Status Code to 200\-OK](#lambda-examples-custom-error-static-body)
+ [Example: Using an Origin\-Response Trigger to Update the Error Status Code to 302\-Found](#lambda-examples-custom-error-new-site)

### Example: Using an Origin\-Response Trigger to Update the Error Status Code to 200\-OK<a name="lambda-examples-custom-error-static-body"></a>

This function demonstrates how you can update the response status to 200 and generate static body content to return to the viewer in the following scenario:
+ The function is triggered in an origin response\.
+ The response status from the origin server is an error status code \(4xx or 5xx\)\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
    const response = event.Records[0].cf.response;

    /**
     * This function updates the response status to 200 and generates static
     * body content to return to the viewer in the following scenario:
     * 1. The function is triggered in an origin response
     * 2. The response status from the origin server is an error status code (4xx or 5xx)
     */

    if (response.status >= 400 && response.status <= 599) {
        response.status = 200;
        response.statusDescription = 'OK';
        response.body = 'Body generation example';
    }

    callback(null, response);
};
```

------
#### [ Python ]

```
 def lambda_handler(event, context):
     response = event['Records'][0]['cf']['response']
 
     '''
     This function updates the response status to 200 and generates static
     body content to return to the viewer in the following scenario:
     1. The function is triggered in an origin response
     2. The response status from the origin server is an error status code (4xx or 5xx)
     '''
 
     if int(response['status']) >= 400 and int(response['status']) <= 599:
         response['status'] = 200
         response['statusDescription'] = 'OK'
         response['body'] = 'Body generation example'
     return response
```

------

### Example: Using an Origin\-Response Trigger to Update the Error Status Code to 302\-Found<a name="lambda-examples-custom-error-new-site"></a>

This function demonstrates how you can update the HTTP status code to 302 to redirect to another path \(cache behavior\) that has a different origin configured\. Note the following:
+ The function is triggered in an origin response\.
+ The response status from the origin server is an error status code \(4xx or 5xx\)\.

------
#### [ Node\.js ]

```
'use strict';

exports.handler = (event, context, callback) => {
    const response = event.Records[0].cf.response;
    const request = event.Records[0].cf.request;

    /**
     * This function updates the HTTP status code in the response to 302, to redirect to another
     * path (cache behavior) that has a different origin configured. Note the following:
     * 1. The function is triggered in an origin response
     * 2. The response status from the origin server is an error status code (4xx or 5xx)
     */

    if (response.status >= 400 && response.status <= 599) {
        const redirect_path = `/plan-b/path?${request.querystring}`;

        response.status = 302;
        response.statusDescription = 'Found';

        /* Drop the body, as it is not required for redirects */
        response.body = '';
        response.headers['location'] = [{ key: 'Location', value: redirect_path }];
    }

    callback(null, response);
};
```

------
#### [ Python ]

```
 def lambda_handler(event, context):
     response = event['Records'][0]['cf']['response']
     request = event['Records'][0]['cf']['request']
 
     '''
     This function updates the HTTP status code in the response to 302, to redirect to another
     path (cache behavior) that has a different origin configured. Note the following:
     1. The function is triggered in an origin response
     2. The response status from the origin server is an error status code (4xx or 5xx)
     '''
 
     if int(response['status']) >= 400 and int(response['status']) <= 599:
         redirect_path = '/plan-b/path?%s' % request['querystring']
 
         response['status'] = 302
         response['statusDescription'] = 'Found'
 
         # Drop the body as it is not required for redirects
         response['body'] = ''   
         response['headers']['location'] = [{'key': 'Location', 'value': redirect_path}] 
 
     return response
```

------

## Accessing the Request Body \- Examples<a name="lambda-examples-access-request-body-examples"></a>

The examples in this section illustrate how you can use Lambda@Edge to work with POST requests\.

**Note**  
To use these examples, you must enable the *include body* option in the distribution’s Lambda function association\. It is not enabled by default\.  
To enable this setting in the CloudFront console, select the check box for **Include Body** in the **Lambda Function Association**\.
To enable this setting in the CloudFront API or with AWS CloudFormation, set the `IncludeBody` field to `true` in `LambdaFunctionAssociation`\.

**Topics**
+ [Example: Using a Request Trigger to Read an HTML Form](#lambda-examples-access-request-body-examples-read)
+ [Example: Using a Request Trigger to Modify an HTML Form](#lambda-examples-access-request-body-examples-replace)

### Example: Using a Request Trigger to Read an HTML Form<a name="lambda-examples-access-request-body-examples-read"></a>

This function demonstrates how you can process the body of a POST request generated by an HTML form \(web form\), such as a "contact us" form\. For example, you might have an HTML form like the following:

```
<html>
  <form action="http://example.com" method="post">
    Param 1: <input type="text" name="name1"><br>
    Param 2: <input type="text" name="name2"><br>
    input type="submit" value="Submit">
  </form>
</html>
```

For the example function that follows, the function must be triggered in a CloudFront viewer request or origin request\.

------
#### [ Node\.js ]

```
'use strict';

const querystring = require('querystring');

/**
 * This function demonstrates how you can read the body of a POST request 
 * generated by an HTML form (web form). The function is triggered in a
 * CloudFront viewer request or origin request event type.
 */

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;

    if (request.method === 'POST') {
        /* HTTP body is always passed as base64-encoded string. Decode it. */
        const body = Buffer.from(request.body.data, 'base64').toString();
 
        /* HTML forms send the data in query string format. Parse it. */
        const params = querystring.parse(body);
 
        /* For demonstration purposes, we only log the form fields here.
         * You can put your custom logic here. For example, you can store the 
         * fields in a database, such as AWS DynamoDB, and generate a response
         * right from your Lambda@Edge function.
         */
        for (let param in params) {
            console.log(`For "${param}" user submitted "${params[param]}".\n`);
        }
    }
    return callback(null, request);
};
```

------
#### [ Python ]

```
 import base64
 from urllib.parse import parse_qs
 
 '''
 Say there is a POST request body generated by an HTML such as:
 
 <html>
   <form action="http://example.com" method="post">
     Param 1: <input type="text" name="name1"><br>
     Param 2: <input type="text" name="name2"><br>
     input type="submit" value="Submit">
   </form>
 </html>
 
 '''
 
 '''
 This function demonstrates how you can read the body of a POST request 
 generated by an HTML form (web form). The function is triggered in a
 CloudFront viewer request or origin request event type. 
 '''
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
 
     if request['method'] == 'POST':
         # HTTP body is always passed as base64-encoded string. Decode it
         body = base64.b64decode(request['body']['data'])
 
         # HTML forms send the data in query string format. Parse it
         params = {k: v[0] for k, v in parse_qs(body).items()}
 
         '''
         For demonstration purposes, we only log the form fields here.
         You can put your custom logic here. For example, you can store the 
         fields in a database, such as AWS DynamoDB, and generate a response
         right from your Lambda@Edge function.
         '''
         for key, value in params.items():
             print("For %s use submitted %s" % (key, value))
             
     return request
```

------

### Example: Using a Request Trigger to Modify an HTML Form<a name="lambda-examples-access-request-body-examples-replace"></a>

This function demonstrates how you can modify the body of a POST request generated by an HTML form \(web form\)\. The function is triggered in a CloudFront viewer request or origin request\.

------
#### [ Node\.js ]

```
'use strict';
				
const querystring = require('querystring');

exports.handler = (event, context, callback) => {
    var request = event.Records[0].cf.request;
    if (request.method === 'POST') {
        /* Request body is being replaced. To do this, update the following
        /* three fields:
         *    1) body.action to 'replace'
         *    2) body.encoding to the encoding of the new data.
         *
         *       Set to one of the following values:
         *
         *           text - denotes that the generated body is in text format.
         *               Lambda@Edge will propagate this as is.
         *           base64 - denotes that the generated body is base64 encoded.
         *               Lambda@Edge will base64 decode the data before sending
         *               it to the origin.
         *    3) body.data to the new body.
         */
        request.body.action = 'replace';
        request.body.encoding = 'text';
        request.body.data = getUpdatedBody(request);
    }
    callback(null, request);
};

function getUpdatedBody(request) {
    /* HTTP body is always passed as base64-encoded string. Decode it. */
    const body = Buffer.from(request.body.data, 'base64').toString();

    /* HTML forms send data in query string format. Parse it. */
    const params = querystring.parse(body);

    /* For demonstration purposes, we're adding one more param.
     *
     * You can put your custom logic here. For example, you can truncate long
     * bodies from malicious requests.
     */
    params['new-param-name'] = 'new-param-value';
    return querystring.stringify(params);
}
```

------
#### [ Python ]

```
 import base64
 from urllib.parse import parse_qs, urlencode
 
 def lambda_handler(event, context):
     request = event['Records'][0]['cf']['request']
     if request['method'] == 'POST':
         '''
         Request body is being replaced. To do this, update the following
         three fields:
             1) body.action to 'replace'
             2) body.encoding to the encoding of the new data.
         
                Set to one of the following values:
         
                    text - denotes that the generated body is in text format.
                        Lambda@Edge will propagate this as is.
                    base64 - denotes that the generated body is base64 encoded.
                        Lambda@Edge will base64 decode the data before sending
                        it to the origin.
             3) body.data to the new body.
         '''
         request['body']['action'] = 'replace'
         request['body']['encoding'] = 'text'
         request['body']['data'] = getUpdatedBody(request)
     return request
 
 def getUpdatedBody(request):
     # HTTP body is always passed as base64-encoded string. Decode it
     body = base64.b64decode(request['body']['data'])
 
     # HTML forms send data in query string format. Parse it
     params = {k: v[0] for k, v in parse_qs(body).items()}
 
     # For demonstration purposes, we're adding one more param
 
     # You can put your custom logic here. For example, you can truncate long
     # bodies from malicious requests
     params['new-param-name'] = 'new-param-value'
     return urlencode(params)
```

------