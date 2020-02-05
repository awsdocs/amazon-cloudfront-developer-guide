# Get Started Creating and Using Lambda@Edge Functions<a name="lambda-edge-how-it-works"></a>

You can use Lambda@Edge functions to do lots of useful things, but it can seem a little complicated when you're getting started\. This section explains, at a high level, how Lambda@Edge works with CloudFront and [provides a tutorial](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-how-it-works-tutorial.html) that steps through a simple example\.

**Tip**  
After you're familiar with how Lambda@Edge works and you've created a Lambda@Edge function, learn more about how you can use Lambda@Edge for your own custom solutions\. Learn more about [ creating and updating functions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-create-function.html), [ the event structure](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-event-structure.html), and [ adding CloudFront triggers](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-add-triggers.html)\. You can also find more ideas and get code samples in [Lambda@Edge Example Functions](lambda-examples.md)\.

Here's an overview of how to create and use Lambda functions with CloudFront:

1. In the AWS Lambda console, create a Lambda function in the US East \(N\. Virginia\) Region\. \(Or you can create the function programmatically, for example, by using one of the AWS SDKs\.\) 

1. Save and publish a numbered version of the function\.

   If you want to change the function, you must edit the $LATEST version of the function in the US East \(N\. Virginia\) Region\. Then, before you set it up to work with CloudFront, you publish a new numbered version\.

1. Choose the CloudFront distribution and cache behavior that the function applies to\. Then specify one or more CloudFront events \(*triggers*\) that cause the function to execute\. For example, you can create a trigger for the function to execute when CloudFront receives a request from a viewer\.

1. When you create a trigger, Lambda replicates the function to AWS locations around the world\.

![\[Conceptual graphic that shows how you create Lambda functions that integrate with CloudFront.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/lambda-creation-workflow-aws-location.png)

**Topics**
+ [Tutorial: Creating a Simple Lambda@Edge Function](lambda-edge-how-it-works-tutorial.md)