# Troubleshooting Lambda@Edge<a name="troubleshooting-lambda"></a>

If your Lambda@Edge function is returning one of the following errors, you're still using the preview version of Lambda@Edge:

+ "The function has an invalid runtime \.\.\."

+ "The function code size is too big \.\.\."

+ "Invalid or unsupported region in the function \.\.\."

To confirm that you're still using the preview version, check the following:

+ Your existing functions are coded using the Edge Node\.js 4\.3 runtime, which is no longer supported\.

+ Your existing triggers reference the function only by name, which is no longer supported\. Triggers must reference a numbered version of the function, for example:

  arn:aws:lambda:us\-east\-1:123456789012:function:http\-redirect:**1**

You can continue to use the preview version of Lambda@Edge for a distribution until you delete all the triggers that are associated with the distribution\. To add new triggers to the distribution, you must migrate from the preview version to the general availability version; you can't revert to the preview version\.

**To migrate from the preview version to the general availability version of Lambda@Edge for a distribution**

1. Use the CloudFront console to delete all the existing triggers for the distribution:

   1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

   1. Choose the ID for the distribution that you want to update\.

   1. Choose **Behaviors**\.

   1. Select the check box for the cache behavior that you want to delete triggers for, and choose **Edit**\.

   1. For **Lambda Function Associations**, choose the **X** next to each function association \(trigger\)\.

   1. After you remove all triggers, choose **Yes, Edit**\.

   1. To delete triggers for other cache behaviors for this distribution, repeat steps 2d through 2f\.

      After you delete all triggers for a distribution and save your changes, the triggers that you add to the distribution provide the general availability functionality of Lambda@Edge instead of the preview functionality\.

1. Use the Lambda console to create new versions of your functions\. Note the following:

   + Create functions in the US East \(N\. Virginia\) Region\.

   + Create functions with the nodejs6\.10 runtime property\.

   + Update the event structure for headers\. For more information, see [Event Structure](lambda-event-structure.md)\.

   + For general information, see [Using CloudFront with Lambda@Edge](lambda-at-the-edge.md)\.

   + For information about how to use the Lambda console to create functions, see [Creating a Lambda@Edge Function and Creating a Trigger for a CloudFront Event](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-create-function) in the *AWS Lambda Developer Guide*\.

1. Use the Lambda console to add CloudFront triggers to the new functions\. The Lambda console allows you to test the triggers as you create them; this option isn't available if you create triggers in the CloudFront console\. For more information, see [Adding Triggers for a Lambda@Edge Function \(AWS Lambda Console\)](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-add-triggers) in the *AWS Lambda Developer Guide*\.