# Lambda@Edge Example Functions<a name="lambda-examples"></a>

See the following sections for examples of using Lambda functions with CloudFront\.

Note that each Lambda@Edge function must contain the `callback` parameter to successfully process a request or return a response\. For more information, see [Writing and Creating a Lambda@Edge Function](lambda-edge-create-function.md)\.

**Topics**
+ [General Examples](#lambda-examples-general-examples)
+ [Generating Responses \- Examples](#lambda-examples-generated-response-examples)
+ [Working with Query Strings \- Examples](#lambda-examples-query-string-examples)
+ [Personalize Content by Country or Device Type Headers \- Examples](#lambda-examples-redirecting-examples)
+ [Content\-Based Routing \- Examples](#lambda-examples-content-based-routing-examples)
+ [Updating Error Statuses \- Examples](#lambda-examples-update-error-status-examples)

## General Examples<a name="lambda-examples-general-examples"></a>

### Example: A/B Testing<a name="lambda-examples-a-b-testing"></a>

You can use the following example if you want to test two different versions of your home page, but you don't want to create redirects or change the URL\. This example sets cookies when CloudFront receives a request, randomly assigns the user to version A or B, and then returns the corresponding version to the viewer\.

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

### Example: Overriding a Response Header<a name="lambda-examples-overriding-response-header"></a>

The following example shows how to change the value of a response header based on the value of another header:

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

## Generating Responses \- Examples<a name="lambda-examples-generated-response-examples"></a>

### Example: Serving Static Content \(Generated Response\)<a name="lambda-examples-static-web-server"></a>

The following example shows how to use a Lambda function to serve static website content, which reduces the load on the origin server and reduces overall latency\. 

**Note**  
You can generate HTTP responses for viewer request and origin request events\. For more information, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses.md)\. You can also replace the HTTP response in origin and viewer response events\. For more information, see [Updating HTTP Responses in Origin\-Response Triggers](lambda-updating-http-responses.md)\.

```
'use strict';

let content = `
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
            }],
            'content-encoding': [{
                key: 'Content-Encoding',
                value: 'UTF-8'
            }],
        },
        body: content,
    };
    callback(null, response);
};
```

### Example: Serving Static Website Content as Gzip Compressed Content \(Generated Response\)<a name="lambda-examples-body-encoding-base64"></a>

This function demonstrates how to use a Lambda function to serve static website content as gzip compressed content, which reduces the load on the origin server and reduces overall latency\.

You can generate HTTP responses for viewer request and origin request events\. For more information, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses.md)

```
'use strict';

const zlib = require('zlib');

let content = `
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
     * Generate HTTP OK response using 200 status code with a gzip compressed content HTML body.
     */
    
    const buffer = zlib.gzipSync(content); 
    const base64EncodedBody = buffer.toString('base64');
    
    var response = {
        headers: {
            'content-type': [{key:'Content-Type', value: 'text/html; charset=utf-8'}],
            'content-encoding' : [{key:'Content-Encoding', value: 'gzip'}]
         },
        body: base64EncodedBody,
        bodyEncoding: 'base64',
        status: '200',
        statusDescription: "OK"
     }
     
    callback(null, response);
};
```

### Example: Generating an HTTP Redirect \(Generated Response\)<a name="lambda-examples-http-redirect"></a>

The following example shows how to generate an HTTP redirect\.

**Note**  
You can generate HTTP responses for viewer request and origin request events\. For more information, see [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses.md)\.

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

## Working with Query Strings \- Examples<a name="lambda-examples-query-string-examples"></a>

### Example: Adding a Header Based on a Query String Parameter<a name="lambda-examples-header-based-on-query-string"></a>

The following example shows how to get the key\-value pair of a query string parameter and add a header based on those values\.

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

### Example: Normalizing Query String Parameters to Improve the Cache Hit Ratio<a name="lambda-examples-normalize-query-string-parameters"></a>

The following example shows how to improve your cache hit ratio by making the following changes to query strings before CloudFront forwards requests to your origin:
+ Alphabetize key\-value pairs by the name of the parameter
+ Change the case of key\-value pairs to lowercase

For more information, see [Configuring CloudFront to Cache Based on Query String Parameters](QueryStringParameters.md)\.

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

### Example: Redirecting Unauthenticated Users to a Sign\-In Page<a name="lambda-examples-redirect-to-signin-page"></a>

The following example shows how to redirect users to a sign\-in page if they haven't entered their credentials\.

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
    }

    /* URI encode the original request to be sent as redirect_url in query params */
    const encodedRedirectUrl = encodeURIComponent(`https://${headers.host[0].value}${request.uri}?${request.querystring}`);
    const response = {
        status: '302',
        statusDescription: 'Found',
        headers: {
            location: [{
                key: 'Location',
                value: `http://www.example.com/signin?redirect_url=${encodedRedirectUrl}`,
            }],
        },
    };
    callback(null, response);
};
```

## Personalize Content by Country or Device Type Headers \- Examples<a name="lambda-examples-redirecting-examples"></a>

### Example: Redirecting Viewer Requests to a Country\-Specific URL<a name="lambda-examples-redirect-based-on-country"></a>

The following example shows how to generate an HTTP redirect response with a country\-specific URL and return the response to the viewer\. This is useful when you want to provide country\-specific responses\. For example:
+ If you have country\-specific subdomains, such as us\.example\.com and tw\.example\.com, you can generate a redirect response when a viewer requests example\.com\.
+ If you're streaming video but you don't have rights to stream the content in a specific country, you can redirect users in that country to a page that explains why they can't view the video\. 

Note the following:
+ You must configure your distribution to cache based on the `CloudFront-Viewer-Country` header\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\.
+ CloudFront adds the `CloudFront-Viewer-Country` header after the viewer request event\. To use this example, you must create a trigger for the origin request event\.

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

### Example: Serving Different Versions of an Object Based on the Device<a name="lambda-examples-vary-on-device-type"></a>

The following example shows how to serve different versions of an object based on the type of device that the user is using, for example, a mobile device or a tablet\. Note the following:
+ You must configure your distribution to cache based on the `CloudFront-Is-*-Viewer` headers\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\.
+ CloudFront adds the `CloudFront-Is-*-Viewer` headers after the viewer request event\. To use this example, you must create a trigger for the origin request event\.

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
    console.log(`Request uri set to "${request.uri}"`);

    callback(null, request);
};
```

## Content\-Based Routing \- Examples<a name="lambda-examples-content-based-routing-examples"></a>

### Example: Using an Origin\-Request Trigger to Change From a Custom Origin to an Amazon S3 Origin<a name="lambda-examples-content-based-S3-origin-based-on-query"></a>

This function demonstrates how an origin\-request trigger can be used to change from a custom origin to an Amazon S3 origin from which the content is fetched, based on request properties\.

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

### Example: Using an Origin\-Request Trigger to Change the Amazon S3 Origin Region<a name="lambda-examples-content-based-S3-origin-request-trigger"></a>

This function demonstrates how an origin\-request trigger can be used to change the Amazon S3 origin from which the content is fetched, based on request properties\.

In this example, we use the value of the CloudFront\-Viewer\-Country header to update the S3 bucket domain name to a bucket in a region that is closer to the viewer\. This can be useful in several ways:
+ It reduces latencies when the region specified is nearer to the viewer's country\.
+ It provides data sovereignty by making sure that data is served from an origin that's in the same country that the request came from\.

In order to use this example, you must do the following:
+ You must configure your distribution to cache based on the CloudFront\-Viewer\-Country header\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\. 
+ CloudFront adds the CloudFront\-Viewer\-Country header after the viewer request event\. To use this example, you must create a trigger for this function in the origin request event\.

```
'use strict';

exports.handler = (event, context, callback) => {
    const request = event.Records[0].cf.request;

    /**
     * This blueprint demonstrates how an origin-request trigger can be used to
     * change the origin from which the content is fetched, based on request properties.
     * In this example, we use the value of the CloudFront-Viewer-Country header
     * to update the S3 bucket domain name to a bucket in a region that is closer to
     * the viewer.
     * 
     * This can be useful in several ways:
     *      1) Reduces latencies when the region specified is nearer to the viewer’s
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
        'IN': 'ap-south-1',
        'CN': 'cn-north-1'
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
             * the region so it can be used for the SIGV4 signature. Otherwise, the
             * region is not required.
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

### Example: Using an Origin\-Request Trigger to Change From an Amazon S3 Origin to a Custom Origin<a name="lambda-examples-content-based-custom-origin-request-trigger"></a>

This function demonstrates how an origin\-request trigger can be used to change the custom origin from which the content is fetched, based on request properties\.

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

### Example: Using an Origin\-Request Trigger to Gradually Transfer Traffic From One Amazon S3 Bucket to Another<a name="lambda-examples-content-based-gradual-traffic-transfer"></a>

This function demonstrates how you can gradually transfer traffic from one Amazon S3 bucket to another, in a controlled way\.

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

### Example: Using an Origin\-Request Trigger to Change the Origin Domain Name Based on the Country Header<a name="lambda-examples-content-based-geo-header"></a>

This function demonstrates how you can change the origin domain name based on the CloudFront\-Viewer\-Country header, so content is served from an origin closer to the viewer's country\.

Implementing this functionality for your distribution can have advantages such as the following:
+ Reducing latencies when the region specified is nearer to the viewer's country\.
+ Providing data sovereignty by making sure that data is served from an origin that's in the same country that the request came from\.

Note that to enable this functionality you must configure your distribution to cache based on the CloudFront\-Viewer\-Country header\. For more information, see [Cache Based on Selected Request Headers](distribution-web-values-specify.md#DownloadDistValuesForwardHeaders)\.

```
'use strict';

exports.handler = (event, context, callback) => {
     const request = event.Records[0].cf.request;
     
  if (request.headers['cloudfront-viewer-country']) {
         const countryCode = request.headers['cloudfront-viewer-country'][0].value;
         if (countryCode === 'UK' || countryCode === 'DE' || countryCode === 'IE' ) {
             const domainName = 'eu.example.com';
             request.origin.custom.domainName = domainName;
             request.headers['host'] = [{key: 'host', value: domainName}];
         } 
     }
     
    callback(null, request);
};
```

## Updating Error Statuses \- Examples<a name="lambda-examples-update-error-status-examples"></a>

### Example: Using an Origin\-Response Trigger to Update the Error Status Code to 200\-OK<a name="lambda-examples-custom-error-static-body"></a>

This function demonstrates how you can update the response status to 200 and generate static body content to return to the viewer in the following scenario:
+ The function is triggered in an origin response
+ The response status from the origin server is an error status code \(4xx or 5xx\)

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

### Example: Using an Origin\-Response Trigger to Update the Error Status Code to 302\-Found<a name="lambda-examples-custom-error-new-site"></a>

This function demonstrates how you can update the HTTP status code to 302 to redirect to another path \(cache behavior\) that has a different origin configured\. Note the following:
+ The function is triggered in an origin response
+ The response status from the origin server is an error status code \(4xx or 5xx\)

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