# Customizing at the edge with CloudFront Functions<a name="cloudfront-functions"></a>

With CloudFront Functions in Amazon CloudFront, you can write lightweight functions in JavaScript for high\-scale, latency\-sensitive CDN customizations\. Your functions can manipulate the requests and responses that flow through CloudFront, perform basic authentication and authorization, generate HTTP responses at the edge, and more\. The CloudFront Functions runtime environment offers submillisecond startup times, scales immediately to handle millions of requests per second, and is highly secure\. CloudFront Functions is a native feature of CloudFront, which means you can build, test, and deploy your code entirely within CloudFront\.

CloudFront Functions is ideal for lightweight, short\-running functions for use cases like the following:
+ **Cache key normalization** – You can transform HTTP request attributes \(headers, query strings, cookies, even the URL path\) to create an optimal [cache key](understanding-the-cache-key.md), which can improve your cache hit ratio\.
+ **Header manipulation** – You can insert, modify, or delete HTTP headers in the request or response\. For example, you can add a `True-Client-IP` header to every request\.
+ **Status code modification and body generation** – You can evaluate headers and respond back to viewers with customized content\.
+ **URL redirects or rewrites** – You can redirect viewers to other pages based on information in the request, or rewrite all requests from one path to another\.
+ **Request authorization** – You can validate hashed authorization tokens, such as JSON web tokens \(JWT\), by inspecting authorization headers or other request metadata\.

When you associate a CloudFront function with a CloudFront distribution, CloudFront intercepts requests and responses at CloudFront edge locations and passes them to your function\. You can invoke CloudFront functions when the following events occur:
+ When CloudFront receives a request from a viewer \(viewer request\)
+ Before CloudFront returns the response to the viewer \(viewer response\)

For a step\-by\-step guide to creating a CloudFront function, see [Tutorial: Creating a simple function with CloudFront Functions](functions-tutorial.md)\.

To get started writing function code and to read example code that you can use with CloudFront Functions, see [Writing function code \(programming model\)](writing-function-code.md) and [Example code](functions-example-code.md)\.