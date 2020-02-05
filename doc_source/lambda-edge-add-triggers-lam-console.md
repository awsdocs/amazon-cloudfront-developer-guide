# Adding Triggers by Using the Lambda Console<a name="lambda-edge-add-triggers-lam-console"></a><a name="lambda-edge-add-triggers-procedure"></a>

**To add triggers to a Lambda@Edge function \(AWS Lambda console\)**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. In the Region list at the top of the page, choose **US East \(N\. Virginia\)**\.

1. On the **Functions** page, choose the name of the function that you want to add triggers for\.

1. Choose **Qualifiers**, and then choose the **Versions** tab\.

1. Choose the version that you want to add triggers to\.
**Important**  
You can't create triggers for the $LATEST version, you must create them for a numbered version\.

   After you choose a version, the name of the button changes to **Version: $LATEST** or **Version:** *version number*\.

1. Choose the **Triggers** tab\.

1. Choose **Add triggers**\.

1. In the **Add trigger** dialog box, choose the dotted box, and then choose **CloudFront**\.
**Note**  
If you've already created one or more triggers, CloudFront is the default service\.

1. Specify the following values to indicate when you want the Lambda function to execute\.  
**Distribution ID**  
Choose the ID of the distribution that you want to add the trigger to\.  
**Cache behavior**  
Choose the cache behavior that specifies the objects that you want to execute the function on\.  
If you specify `*` for the cache behavior, the Lambda function deploys to the default cache behavior\.  
**CloudFront event**  
Choose the CloudFront event that causes the function to execute\.  
**Include body**  
Select this check box if you want to access the request body in your function\.   
**Enable trigger and replicate**  
Select this check box so that AWS Lambda replicates the function to Regions globally\.

1. Choose **Submit**\.

   The function starts to process requests for the specified CloudFront events when the updated CloudFront distribution is deployed\. To determine whether a distribution is deployed, choose **Distributions** in the navigation pane\. When a distribution is deployed, the value of the **Status** column for the distribution changes from **In Progress** to **Deployed**\.