# Writing function code \(CloudFront Functions programming model\)<a name="writing-function-code"></a>

With CloudFront Functions in Amazon CloudFront, you can write lightweight functions in JavaScript for high\-scale, latency\-sensitive CDN customizations\. Your function code can manipulate the requests and responses that flow through CloudFront, perform basic authentication and authorization, generate HTTP responses at the edge, and more\.

The following topics can help you write function code for CloudFront Functions\.

**Topics**
+ [Choose the purpose of your function](#function-code-choose-purpose)
+ [Event structure](functions-event-structure.md)
+ [JavaScript runtime features](functions-javascript-runtime-features.md)
+ [Example code](functions-example-code.md)

## Choose the purpose of your function<a name="function-code-choose-purpose"></a>

Before you write your function code, determine the purpose of your function\. Most functions in CloudFront Functions have one of the following purposes\. For more information, see the topic that corresponds to your function’s purpose\.

Regardless of your function’s purpose, the `handler` is the entry point for any function\. It takes a single argument called `event`, which is passed to the function by CloudFront\. The `event` is a JSON object that contains a representation of the HTTP request \(and the response, if your function modifies the HTTP response\)\. For more information about the structure of the `event` object, see [CloudFront Functions event structure](functions-event-structure.md)\.

For more information about restrictions that apply to CloudFront Functions and Lambda@Edge, see [Restrictions on edge functions](edge-functions-restrictions.md)\.

**Topics**
+ [Modify the HTTP request in a viewer request event type](#function-code-modify-request)
+ [Generate an HTTP response in a viewer request event type](#function-code-generate-response)
+ [Modify the HTTP response in a viewer response event type](#function-code-modify-response)

### Modify the HTTP request in a viewer request event type<a name="function-code-modify-request"></a>

Your function can modify the HTTP request that CloudFront receives from the viewer \(client\), and return the modified request to CloudFront for continued processing\. For example, your function code might normalize the [cache key](understanding-the-cache-key.md) or modify request headers\.

When you create a function that modifies the HTTP request, make sure to choose the *viewer request* event type\. This means that the function runs each time that CloudFront receives a request from a viewer, before checking to see whether the requested object is in the CloudFront cache\.

The following pseudocode shows the structure of a function that modifies the HTTP request\.

```
function handler(event) {
    var request = event.request;

    // Modify the request object here.

    return request;
}
```

The function returns the modified `request` object to CloudFront\. CloudFront continues processing the returned request by checking the CloudFront cache for a cache hit, and sending the request to the origin if necessary\.

For more information about the structure of the `event` and `request` objects, see [Event structure](functions-event-structure.md)\.

### Generate an HTTP response in a viewer request event type<a name="function-code-generate-response"></a>

Your function can generate an HTTP response at the edge and return it directly to the viewer \(client\) without checking for a cached response or any further processing by CloudFront\. For example, your function code might redirect the request to a new URL, or check for authorization and return a `401` or `403` response to unauthorized requests\.

When you create a function that generates an HTTP response, make sure to choose the *viewer request* event type\. This means that the function runs each time CloudFront receives a request from a viewer, before CloudFront does any further processing of the request\.

The following pseudocode shows the structure of a function that generates an HTTP response\.

```
function handler(event) {
    var request = event.request;

    var response = ...; // Create the response object here,
                        // using the request properties if needed.

    return response;
}
```

The function returns a `response` object to CloudFront, which CloudFront immediately returns to the viewer without checking the CloudFront cache or sending a request to the origin\.

For more information about the structure of the `event`, `request`, and `response` objects, see [Event structure](functions-event-structure.md)\.

### Modify the HTTP response in a viewer response event type<a name="function-code-modify-response"></a>

Your function can modify the HTTP response before CloudFront sends it to the viewer \(client\), regardless of whether the response comes from the CloudFront cache or the origin\. For example, your function code might add or modify response headers, status codes, and body content\.

When you create a function that modifies the HTTP response, make sure to choose the *viewer response* event type\. This means that the function runs before CloudFront returns a response to the viewer, regardless of whether the response comes from the CloudFront cache or the origin\.

The following pseudocode shows the structure of a function that modifies the HTTP response\.

```
function handler(event) {
    var request = event.request;
    var response = event.response;

    // Modify the response object here,
    // using the request properties if needed.

    return response;
}
```

The function returns the modified `response` object to CloudFront, which CloudFront immediately returns to the viewer\.

For more information about the structure of the `event` and `response` objects, see [Event structure](functions-event-structure.md)\.

For more information about writing function code for CloudFront Functions, see [Event structure](functions-event-structure.md), [JavaScript runtime features](functions-javascript-runtime-features.md), and [Example code](functions-example-code.md)\.