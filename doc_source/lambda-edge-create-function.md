# Writing and Creating a Lambda@Edge Function<a name="lambda-edge-create-function"></a>

To use Lambda@Edge, you write the code for your Lambda function, then set up AWS Lambda to run the function based on specific CloudFront events \(triggers\)\. To set up Lambda to run your function, you use the create function option in Lambda\.

You can use the AWS console to work with Lambda functions and CloudFront triggers, or you can work with Lambda@Edge programmatically by using APIs\.
+ If you use the console, be aware that you can use only the AWS Lambda console to create Lambda functions\. You can't use the Amazon CloudFront console to create a function\.
+ If you want to work with Lambda@Edge programmatically, there are several resources to help you\. For more information, see [Creating Lambda Functions and CloudFront Triggers Programmatically](lambda-edge-create-programmatically.md)\.

**Note**  
You can use either the AWS Lambda console or CloudFront console to add triggers for Lambda@Edge functions\.

**Topics**
+ [Writing Functions for Lambda@Edge](lambda-edge-authoring-functions.md)
+ [Creating a Lambda@Edge Function in the Lambda Console](lambda-edge-create-in-lambda-console.md)
+ [Editing a Lambda Function for Lambda@Edge](lambda-edge-edit-function.md)
+ [Creating Lambda Functions and CloudFront Triggers Programmatically](lambda-edge-create-programmatically.md)