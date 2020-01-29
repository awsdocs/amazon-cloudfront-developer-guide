# CloudFront Events That Can Trigger a Lambda Function<a name="lambda-cloudfront-trigger-events"></a>

For each cache behavior in a CloudFront distribution, you can add up to four triggers \(associations\) that cause a Lambda function to execute when specific CloudFront events occur\. CloudFront triggers can be based on one of four CloudFront events, as shown in the following diagram\.

![\[Conceptual graphic that shows CloudFront trigger events for Lambda functions that integrate with CloudFront.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/cloudfront-events-that-trigger-lambda-functions.png)

The CloudFront events that can be used to trigger Lambda@Edge functions are the following:

**Viewer Request**  
The function executes when CloudFront receives a request from a viewer, before it checks to see whether the requested object is in the CloudFront cache\.

**Origin Request**  
The function executes *only* when CloudFront forwards a request to your origin\. When the requested object is in the CloudFront cache, the function doesn’t execute\.

**Origin Response**  
The function executes after CloudFront receives a response from the origin and before it caches the object in the response\. Note that the function executes even if an error is returned from the origin\.  
The function doesn’t execute in the following cases:  
+ When the requested file is in the CloudFront cache and is not expired\.
+ When the response is generated from a function that was triggered by an origin request event\.

**Viewer Response**  
The function executes before returning the requested file to the viewer\. Note that the function executes regardless of whether the file is already in the CloudFront cache\.  
The function doesn’t execute in the following cases:  
+ When the origin returns an HTTP status code of 400 or higher\.
+ When a custom error page is returned\.
+ When the response is generated from a function that was triggered by a viewer request event\.
+ When CloudFront automatically redirects an HTTP request to HTTPS \(when the value of [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy) is **Redirect HTTP to HTTPS**\)\.

When you add multiple triggers to the same cache behavior, you can use them to run the same function or run different functions for each trigger\. You can also associate the same function with more than one distribution\.

**Note**  
When a CloudFront event triggers the execution of a Lambda function, the function must finish before CloudFront can continue\. For example, if a Lambda function is triggered by a CloudFront viewer request event, CloudFront won’t return a response to the viewer or forward the request to the origin until the Lambda function finishes running\. This means that each request that triggers a Lambda function increases latency for the request, so you’ll want the function to execute as fast as possible\.