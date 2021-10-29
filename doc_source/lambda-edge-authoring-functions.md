# Writing functions for Lambda@Edge<a name="lambda-edge-authoring-functions"></a>

There are several resources to help you with writing Lambda@Edge functions:
+ To learn about the event structure to use with Lambda@Edge functions, see [Lambda@Edge event structure](lambda-event-structure.md)\.
+ To see examples of Lambda@Edge functions, such as functions for A/B testing and generating an HTTP redirect, see [Lambda@Edge example functions](lambda-examples.md)\.

The programming model for using Node\.js or Python with Lambda@Edge is the same as using Lambda in an AWS Region\. For more information, see [Building Lambda functions with Node\.js](https://docs.aws.amazon.com/lambda/latest/dg/lambda-nodejs.html) or [Building Lambda functions with Python](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html)\.

In your Lambda@Edge code, include the `callback` parameter and return the applicable object for request or response events:
+ **Request events** – Include the `cf.request` object in the response\.

  If you're generating a response, include the `cf.response` object in the response\. For more information, see [Generating HTTP responses in request triggers](lambda-generating-http-responses-in-requests.md)\. 
+ **Response events** – Include the `cf.response` object in the response\.