# Using CloudFront with Lambda@Edge<a name="lambda-at-the-edge"></a>

Lambda@Edge is an extension of AWS Lambda, a compute service that lets you execute functions that customize the content that is delivered through CloudFront\. You can author functions in one region and execute them in AWS Regions and edge locations globally without provisioning or managing servers\. Lambda@Edge scales automatically, from a few requests per day to thousands per second\. Processing requests at edge locations instead of on origin servers significantly reduces latency and improves the user experience\.

When you associate a CloudFront distribution with a Lambda@Edge function, CloudFront intercepts requests and responses at CloudFront edge locations\. Lambda@Edge functions execute in response to CloudFront events in the region or edge location that is closest to your customer\.

You can execute Lambda functions when the following CloudFront events occur: 

+ When CloudFront receives a request from a viewer \(viewer request\)

+ Before CloudFront forwards a request to the origin \(origin request\)

+ When CloudFront receives a response from the origin \(origin response\)

+ Before CloudFront returns the response to the viewer \(viewer response\)

There are many uses for Lambda@Edge processing\. Here are just a few possibilities:

+ You can write Lambda functions that inspect cookies and rewrite URLs so that users see different versions of a site for A/B testing\.

+ CloudFront can return different objects to viewers based on the `User-Agent` header, which includes information about the devices that users are using to view your content\. For example, CloudFront can return different images based on the screen size of devices\. Similarly, the function could consider the value of the `Referer` header and cause CloudFront to return the images that have the lowest available resolution to bots\. 

  You might also want to consider additional criteria\. For example, on a retail website that sells clothing, if you use cookies to indicate which color a user chose for a jacket, the function can change the request so that CloudFront returns the image of a jacket in the selected color\.

+ You can use a Lambda function to generate HTTP responses when CloudFront viewer request or origin request events occur\.

+ A Lambda function can inspect headers or authorization tokens, and insert the applicable header to control access to your content before CloudFront forwards a request to the origin\.

+ A Lambda function can add, drop, and modify headers, and can rewrite URL paths so that CloudFront returns different objects\.

+ A Lambda function can make network calls to external resources to confirm user credentials, or fetch additional content to customize your responses\.

For more information about setting up CloudFront with Lambda@Edge, see [Lambda@Edge](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html) in the *AWS Lambda Developer Guide*\. After you complete the setup steps, see the following topics to learn how to use CloudFront with Lambda@Edge\.


+ [CloudFront Events That Can Trigger a Lambda Function](lambda-cloudfront-trigger-events.md)
+ [How You Decide Which CloudFront Event to Use to Trigger a Lambda Function](lambda-how-to-choose-event.md)
+ [Requirements and Restrictions on Lambda Functions](lambda-requirements-limits.md)
+ [Creating Lambda Functions and Adding Triggers](lambda-create-functions.md)
+ [Event Structure](lambda-event-structure.md)
+ [Generating HTTP Responses in Request Triggers](lambda-generating-http-responses.md)
+ [Updating HTTP Responses in an Origin\-Response Trigger](lambda-updating-http-responses.md)
+ [CloudWatch Metrics and CloudWatch Logs for Lambda Functions](lambda-cloudwatch-metrics-logging.md)
+ [Example Functions](lambda-examples.md)