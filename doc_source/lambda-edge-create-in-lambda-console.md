# Creating a Lambda@Edge Function in the Lambda Console<a name="lambda-edge-create-in-lambda-console"></a>

To set up AWS Lambda to run Lambda functions that are based on CloudFront events, follow this procedure\.<a name="lambda-edge-create-function-procedure"></a>

**To create a Lambda@Edge function**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. If you already have one or more Lambda functions, choose **Create function**\.

   If you've don't have any functions, choose **Get Started Now**\.

1. In the Region list at the top of the page, choose **US East \(N\. Virginia\)**\.

1. Create a function using your own code or create a function starting with a CloudFront blueprint\.
   + To create a function using your own code, choose **Author from scratch**\. 
   + To display a list of blueprints for CloudFront, type **cloudfront** in the filter field, and then press **Enter**\.

     If you find a blueprint that you want to use, choose the name of the blueprint\.

1. In the **Basic information** section, specify the following values:  
**Name**  
Type a name for your function\.  
**Role**  
Choose **Create new role from template\(s\)**\.  
Choosing this value will get you started quickly\. Or you can choose **Choose an existing role** or **Create a custom role**\. If you choose one of these, follow the prompts to complete the information for this section\.  
**Role name**  
Type a name for the role\.  
**Policy templates**  
Choose **Basic Edge Lambda permissions**\.

1. If you chose **Author from scratch** in step 4, skip to step 7\.

   If you chose a blueprint in step 4, the **cloudfront** section lets you create one trigger, which associates this function with a cache in a CloudFront distribution and a CloudFront event\. We recommend that you choose **Remove** at this point, so there isn't a trigger for the function when it's created\. Then you can add triggers later\. 
**Important**  
Why add triggers later? Generally it's best to test and debug the function before you add triggers\. If you choose instead to add a trigger now, the function will start to run as soon as you create the function and it finishes replicating to AWS locations around the world, and the corresponding distribution is deployed\.

1. Choose **Create function**\.

   Lambda creates two versions of your function: $LATEST and Version 1\. You can edit only the $LATEST version, but the console initially displays Version 1\.

1. To edit the function, choose **Version 1** near the top of the page, under the ARN for the function\. Then, on the **Versions** tab, choose **$LATEST**\. \(If you left the function and then returned to it, the button label is **Qualifiers**\.\)

1. On the **Configuration** tab, choose the applicable **Code entry type**\. Then follow the prompts to edit or upload your code\.

1. For **Runtime**, choose the value based on your function's code\.

1. In the **Tags** section, add any applicable tags\.

1. Choose **Actions**, and then choose **Publish new version**\.

1. Type a description for the new version of the function\.

1. Choose **Publish**\.

1. Test and debug the function\. For more information about testing in the Lambda console, see the *Invoke the Lambda Function and Verify Results, Logs, and Metrics* section in [ Create a Lambda Function with the Console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html) in the *AWS Lambda Developer Guide*\.

1. When you're ready to have the function execute for CloudFront events, publish another version and edit the function to add triggers\. For more information, see [Adding Triggers for a Lambda@Edge Function](lambda-edge-add-triggers.md)\.