# Adding Triggers for a Lambda@Edge Function<a name="lambda-edge-add-triggers"></a>

A Lambda@Edge trigger is one combination of CloudFront distribution, cache behavior, and event that causes a function to execute\. You can specify one or more CloudFront triggers that cause the function to run\. For example, you can create a trigger that causes the function to execute when CloudFront receives a request from a viewer for a specific cache behavior you set up for your distribution\.

**Tip**  
If you're not familiar with CloudFront cache behaviors, here's a brief overview\. When you create a CloudFront distribution, you specify settings that tell CloudFront how to respond when it receives different requests\. The default settings are called the default cache behavior for the distribution\. You can set up additional cache behaviors that define how CloudFront responds under specific circumstances, for example, when it receives a request for a specific file type\. For more information, see [Cache Behavior Settings](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesCacheBehavior)\.

At the time that you create a Lambda function, you can specify only one trigger\. But you can add more triggers to the same function later in one of two ways: by using the Lambda console or by editing the distribution in the CloudFront console\.
+ Using the Lambda console works well if you want to add more triggers to a function for the same CloudFront distribution\.
+ Using the CloudFront console can be better if you want to add triggers for multiple distributions because it's easier to find the distribution that you want to update\. You can also update other CloudFront settings at the same time\.

**Note**  
If you want to work with Lambda@Edge programmatically, there are several resources to help you\. For more information, see [Creating Lambda Functions and CloudFront Triggers Programmatically](lambda-edge-create-programmatically.md)\.

**Topics**
+ [CloudFront Events That Can Trigger a Lambda Function](lambda-cloudfront-trigger-events.md)
+ [How to Decide Which CloudFront Event to Use to Trigger a Lambda Function](lambda-how-to-choose-event.md)
+ [Adding Triggers by Using the Lambda Console](lambda-edge-add-triggers-lam-console.md)
+ [Adding Triggers by Using the CloudFront Console](lambda-edge-add-triggers-cf-console.md)