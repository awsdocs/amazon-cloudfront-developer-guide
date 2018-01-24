# CloudFront Events That Can Trigger a Lambda Function<a name="lambda-cloudfront-trigger-events"></a>

For each cache behavior in a CloudFront distribution, you can add up to four triggers \(associations\) that cause a Lambda function to execute when one or more of the following CloudFront events occur\. When you add multiple triggers to the same cache behavior, you can use the same function or different functions for the triggers\. You can also use the same function for more than one distribution\.

**Note**  
When a CloudFront event triggers the execution of a Lambda function, the function must complete before CloudFront continues\. For example, if you associate a Lambda function with a CloudFront viewer request event, CloudFront won't return a response to the viewer or forward the request to the origin until the Lambda function completes\. Each request that triggers a Lambda function increases latency for the request, so you'll want the function to execute as fast as possible\.

**CloudFront Viewer Request**  
The function executes when CloudFront receives a request from a viewer and before it checks to see whether the requested object is in the edge cache\.

**CloudFront Origin Request**  
The function executes only when CloudFront forwards a request to your origin\. When the requested object is in the edge cache, the function doesn't execute\.

**CloudFront Origin Response**  
The function executes after CloudFront receives a response from the origin and before it caches the object in the response\. The function will execute even if an error is returned from the origin\.  
The function doesn't execute in the following cases:  

+ When the requested object is in the edge cache

+ When the response is generated from a function that was triggered by an origin request event

**CloudFront Viewer Response**  
The function executes before returning the requested object to the viewer\. The function executes regardless of whether the object is already in the edge cache\.  
The function doesn't execute in the following cases:  

+ When the origin returns an HTTP status code of 400 or higher

+ When a custom error page is returned

+ When the response is generated from a function that was triggered by a viewer request event

+ When CloudFront automatically redirects an HTTP request to HTTPS \(when the value of [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy) is **Redirect HTTP to HTTPS**\)