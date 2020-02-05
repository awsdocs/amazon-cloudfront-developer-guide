# Adding Triggers by Using the CloudFront Console<a name="lambda-edge-add-triggers-cf-console"></a><a name="lambda-create-functions-add-triggers-cloudfront-console-procedure"></a>

**To add triggers for CloudFront events to a Lambda function \(CloudFront console\)**

1. Get the ARN of the Lambda function that you want to add triggers for:

   1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

   1. In the list of Regions at the top of the page, choose **US East \(N\. Virginia\)**\.

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

   For more information, see [How to Decide Which CloudFront Event to Use to Trigger a Lambda Function](lambda-how-to-choose-event.md)\.

1. Paste the ARN of the Lambda function that you want to execute when the chosen event occurs\. This is the value that you copied in step 1\.

1. Select **Include Body** if you want to access the request body in your function\.

   If you just want to replace the request body, you don't need to select this option\.

1. To execute the same function for more event types, choose **\+** and repeat steps 6 and 7\.

1. Choose **Yes, Edit**\.

1. To add triggers to more cache behaviors for this distribution, repeat steps 5 through 9\.

   The function starts to process requests for the specified CloudFront events when the updated CloudFront distribution is deployed\. To determine whether a distribution is deployed, choose **Distributions** in the navigation pane\. When a distribution is deployed, the value of the **Status** column for the distribution changes from **In Progress** to **Deployed**\.