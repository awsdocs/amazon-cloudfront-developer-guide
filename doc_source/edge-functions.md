# Customizing at the edge with functions<a name="edge-functions"></a>

With Amazon CloudFront, you can write your own code to customize how your CloudFront distributions process HTTP requests and responses\. The code runs close to your viewers \(users\) to minimize latency, and you don’t have to manage servers or other infrastructure\. You can write code to manipulate the requests and responses that flow through CloudFront, perform basic authentication and authorization, generate HTTP responses at the edge, and more\.

The code that you write and attach to your CloudFront distribution is called an *edge function*\. CloudFront provides two ways to write and manage edge functions:
+ **CloudFront Functions** – With CloudFront Functions, you can write lightweight functions in JavaScript for high\-scale, latency\-sensitive CDN customizations\. The CloudFront Functions runtime environment offers submillisecond startup times, scales immediately to handle millions of requests per second, and is highly secure\. CloudFront Functions is a native feature of CloudFront, which means you can build, test, and deploy your code entirely within CloudFront\.
+ **Lambda@Edge** – Lambda@Edge is an extension of [AWS Lambda](https://aws.amazon.com/lambda/) that offers powerful and flexible computing for complex functions and full application logic closer to your viewers, and is highly secure\. Lambda@Edge functions run in a Node\.js or Python runtime environment\. You publish them to a single AWS Region, but when you associate the function with a CloudFront distribution, Lambda@Edge automatically replicates your code around the world\.

## Choosing between CloudFront Functions and Lambda@Edge<a name="edge-functions-choosing"></a>

CloudFront Functions and Lambda@Edge both provide a way to run code in response to CloudFront events\. However, there are important differences that distinguish them\. These differences can help you choose the one that’s right for your use case\. The following table lists some of the important differences between CloudFront Functions and Lambda@Edge\.


|  | CloudFront Functions | Lambda@Edge | 
| --- | --- | --- | 
| Programming languages | JavaScript \(ECMAScript 5\.1 compliant\) | Node\.js and Python | 
| Event sources |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions.html)  | 
| Scale | 10,000,000 requests per second or more | Up to 10,000 requests per second per Region | 
| Function duration | Submillisecond |  Up to 5 seconds \(viewer request and viewer response\) Up to 30 seconds \(origin request and origin response\)  | 
| Maximum memory | 2 MB | 128 – 3,008 MB | 
| Maximum size of the function code and included libraries | 10 KB |  1 MB \(viewer request and viewer response\) 50 MB \(origin request and origin response\)  | 
| Network access | No | Yes | 
| File system access | No | Yes | 
| Access to the request body | No | Yes | 
| Access to geolocation and device data | Yes |  No \(viewer request\) Yes \(origin request, origin response, and viewer response\)  | 
| Can build and test entirely within CloudFront | Yes | No | 
| Function logging and metrics | Yes | Yes | 
| Pricing | Free tier available; charged per request | No free tier; charged per request and function duration | 

**CloudFront Functions** is ideal for lightweight, short\-running functions for use cases like the following:
+ **Cache key normalization** – You can transform HTTP request attributes \(headers, query strings, cookies, and even the URL path\) to create an optimal [cache key](understanding-the-cache-key.md), which can improve your cache hit ratio\.
+ **Header manipulation** – You can insert, modify, or delete HTTP headers in the request or response\. For example, you can add a `True-Client-IP` header to every request\.
+ **URL redirects or rewrites** – You can redirect viewers to other pages based on information in the request, or rewrite all requests from one path to another\.
+ **Request authorization** – You can validate hashed authorization tokens, such as JSON web tokens \(JWT\), by inspecting authorization headers or other request metadata\.

To get started with CloudFront Functions, see [Customizing at the edge with CloudFront Functions](cloudfront-functions.md)\.

**Lambda@Edge** is a good fit for the following scenarios:
+ Functions that take several milliseconds or more to complete\.
+ Functions that require adjustable CPU or memory\.
+ Functions that depend on third\-party libraries \(including the AWS SDK, for integration with other AWS services\)\.
+ Functions that require network access to use external services for processing\.
+ Functions that require file system access or access to the body of HTTP requests\.

To get started with Lambda@Edge, see [Customizing at the edge with Lambda@Edge](lambda-at-the-edge.md)\.