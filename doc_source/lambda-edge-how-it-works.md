# Overview of Creating and Using Lambda@Edge Functions<a name="lambda-edge-how-it-works"></a>

Here's an overview of how to create and use Lambda functions with CloudFront:

1. You use Node\.js 6\.10 to write the code for your Lambda function\.

1. In the AWS Lambda console, you create a Lambda function in the US East \(N\. Virginia\) Region\. \(You can also create the function programmatically, for example, by using one of the AWS SDKs\.\) When you create the function, you specify the following:
   + The CloudFront distribution that you want the function to apply to
   + A cache behavior in the distribution

1. You publish a numbered version of the function\.

   If you want to edit the function, you must edit it in the US East \(N\. Virginia\) Region\. You then publish a new numbered version\.

1. You specify one or more CloudFront events, known as *triggers*, that cause the function to execute\. For example, you can create a trigger that causes the function to execute when CloudFront receives a request from a viewer\.

   When you create a trigger, Lambda replicates the function to AWS locations around the world\.

![\[Conceptual graphic that shows how you create Lambda functions that integrate with CloudFront.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/lambda-creation-workflow.png)