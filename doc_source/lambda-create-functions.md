# Creating Lambda Functions and Adding Triggers<a name="lambda-create-functions"></a>

To use Lambda@Edge, you create Lambda functions and add triggers\. If you use the AWS console, note the following:

+ You can use only the AWS Lambda console to create Lambda functions\.

+ You can use either the AWS Lambda console or the CloudFront console to add triggers\.

You can also perform all operations programmatically\. For more information, see the following documentation:

+ [API Reference](http://docs.aws.amazon.com/lambda/latest/dg/API_Reference.html) in the *AWS Lambda Developer Guide*

+ [Amazon CloudFront API Reference](http://docs.aws.amazon.com/cloudfront/latest/APIReference/)

+ **AWS CLI**

  + [Lambda create\-function command](http://docs.aws.amazon.com/cli/latest/reference/lambda/create-function.html)

  + [CloudFront create\-distribution command](http://docs.aws.amazon.com/cli/latest/reference/cloudfront/create-distribution.html)

  + [CloudFront create\-distribution\-with\-tags command](http://docs.aws.amazon.com/cli/latest/reference/cloudfront/create-distribution-with-tags.html)

  + [CloudFront update\-distribution command](http://docs.aws.amazon.com/cli/latest/reference/cloudfront/update-distribution.html)

+ [AWS SDKs](http://docs.aws.amazon.com/) \(See the **SDKs & Toolkits** section\.\)

+ [AWS Tools for Windows PowerShell Reference](http://docs.aws.amazon.com/powershell/latest/reference/)

## Creating Lambda Functions \(Console\)<a name="lambda-create-functions-console"></a>

For information about creating Lambda functions in the AWS Lambda console, see [AWS Lambda@Edge](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html) in the *AWS Lambda Developer Guide*\.

## Adding Triggers for CloudFront Events to a Lambda Function<a name="lambda-create-functions-add-triggers"></a>

You can add triggers for CloudFront events with either the AWS Lambda console or the CloudFront console\.


+ [Adding Triggers for CloudFront Events \(AWS Lambda Console\)](#lambda-create-functions-add-triggers-lambda-console)
+ [Adding Triggers for CloudFront Events \(CloudFront Console\)](#lambda-create-functions-add-triggers-cloudfront-console)

### Adding Triggers for CloudFront Events \(AWS Lambda Console\)<a name="lambda-create-functions-add-triggers-lambda-console"></a>

For information about adding triggers for CloudFront events in the Lambda console, see [AWS Lambda@Edge](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html) in the *AWS Lambda Developer Guide*\.

### Adding Triggers for CloudFront Events \(CloudFront Console\)<a name="lambda-create-functions-add-triggers-cloudfront-console"></a>

To add triggers for CloudFront events in the CloudFront console, perform the following procedure\.

**To add triggers for CloudFront events to a Lambda function \(CloudFront console\)**

1. Get the ARN of the Lambda function that you want to add triggers for:

   1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

   1. In the list of regions at the top of the page, choose **US East \(N\. Virginia\)**\.

   1. In the list of functions, choose name of the function that you want to add triggers to\.

   1. Choose **Qualifiers**, choose the **Versions** tab, and choose the numbered version that you want to add triggers to\.
**Important**  
You can add triggers only to a numbered version, not to **$LATEST**\.

   1. Copy the ARN that appears at the top of the page, for example:

      `arn:aws:lambda:us-east-1:123456789012:function:TestFunction:2`

      The number at the end \(**2** in this example\) is the version number of the function\.

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the list of distributions, choose the ID of the distribution that you want to add triggers to\.

1. Choose the **Behaviors** tab\.

1. Select the check box for the cache behavior that you want to add triggers to, and then choose **Edit**\.

1. At **Lambda Function Associations**, in the **Event Type** list, choose when you want the function to execute: for viewer requests, viewer responses, origin requests, or origin responses\. 

   For more information, see [How You Decide Which CloudFront Event to Use to Trigger a Lambda Function](lambda-how-to-choose-event.md)\.

1. Paste the ARN of the Lambda function that you want to execute when the chosen event occurs\. This is the value that you copied in step 1\.

1. To execute the same function for more event types, choose **\+** and repeat steps 6 and 7\.

1. Choose **Yes, Edit**\.

1. To add triggers to more cache behaviors for this distribution, repeat steps 5 through 9\.

   The function starts to process requests for the specified CloudFront events when the updated CloudFront distribution is deployed\. To determine whether a distribution is deployed, choose **Distributions** in the navigation pane\. When a distribution is deployed, the value of the **Status** column for the distribution changes from **In Progress** to **Deployed**\.