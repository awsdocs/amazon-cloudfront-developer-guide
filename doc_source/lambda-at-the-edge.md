# Customizing Content at the Edge with Lambda@Edge<a name="lambda-at-the-edge"></a>

Lambda@Edge is an extension of AWS Lambda, a compute service that lets you execute functions that customize the content that CloudFront delivers\. You can author Node\.js or Python functions in one Region, US\-East\-1 \(N\. Virginia\), and then execute them in AWS locations globally that are closer to the viewer, without provisioning or managing servers\. Lambda@Edge scales automatically, from a few requests per day to thousands per second\. Processing requests at AWS locations closer to the viewer instead of on origin servers significantly reduces latency and improves the user experience\.

When you associate a CloudFront distribution with a Lambda@Edge function, CloudFront intercepts requests and responses at CloudFront edge locations\. You can execute Lambda functions when the following CloudFront events occur:
+ When CloudFront receives a request from a viewer \(viewer request\)
+ Before CloudFront forwards a request to the origin \(origin request\)
+ When CloudFront receives a response from the origin \(origin response\)
+ Before CloudFront returns the response to the viewer \(viewer response\)

There are many uses for Lambda@Edge processing\. For example:
+ A Lambda function can inspect cookies and rewrite URLs so that users see different versions of a site for A/B testing\.
+ CloudFront can return different objects to viewers based on the device they're using by checking the `User-Agent` header, which includes information about the devices\. For example, CloudFront can return different images based on the screen size of their device\. Similarly, the function could consider the value of the `Referer` header and cause CloudFront to return the images to bots that have the lowest available resolution\. 
+ Or you could check cookies for other criteria\. For example, on a retail website that sells clothing, if you use cookies to indicate which color a user chose for a jacket, a Lambda function can change the request so that CloudFront returns the image of a jacket in the selected color\.
+ A Lambda function can generate HTTP responses when CloudFront viewer request or origin request events occur\.
+ A function can inspect headers or authorization tokens, and insert a header to control access to your content before CloudFront forwards the request to your origin\.
+ A Lambda function can also make network calls to external resources to confirm user credentials, or fetch additional content to customize a response\.

For sample code and additional examples, see [Lambda@Edge Example Functions](lambda-examples.md)\.

**Topics**
+ [Get Started Creating and Using Lambda@Edge Functions](lambda-edge-how-it-works.md)
+ [Setting IAM Permissions and Roles for Lambda@Edge](lambda-edge-permissions.md)
+ [Writing and Creating a Lambda@Edge Function](lambda-edge-create-function.md)
+ [Adding Triggers for a Lambda@Edge Function](lambda-edge-add-triggers.md)
+ [Testing and Debugging Lambda@Edge Functions](lambda-edge-testing-debugging.md)
+ [CloudWatch Metrics and CloudWatch Logs for Lambda Functions](lambda-cloudwatch-metrics-logging.md)
+ [Deleting Lambda@Edge Functions and Replicas](lambda-edge-delete-replicas.md)
+ [Lambda@Edge Event Structure](lambda-event-structure.md)
+ [Working with Requests and Responses](lambda-generating-http-responses.md)
+ [Lambda@Edge Example Functions](lambda-examples.md)
+ [Requirements and Restrictions on Lambda Functions](lambda-requirements-limits.md)