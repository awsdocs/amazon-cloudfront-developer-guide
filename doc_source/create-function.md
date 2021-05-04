# Creating functions<a name="create-function"></a>

Before you create a function, you must write the function code\. For more information about writing a function, see [Writing function code \(programming model\)](writing-function-code.md)\. For example code that can help you get started, see [Example code](functions-example-code.md)\.

When you create a new function in CloudFront Functions, the function is in the `DEVELOPMENT` stage\. In this stage, you can [test the function](test-function.md), and [update it](update-function.md) if necessary\. When you’re ready to use your function with a CloudFront distribution, you [publish the function](publish-function.md), which copies it from the `DEVELOPMENT` stage to `LIVE`\. When it’s in the `LIVE` stage, you can [associate the function with a distribution’s cache behavior](associate-function.md)\.

You can create a function in the CloudFront console or with the AWS Command Line Interface \(AWS CLI\)\.

## Creating functions \(console\)<a name="create-function-console"></a>

When you create a function in the console, you can start with the default function, copy a function from [example code on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions), or create your own function code from the beginning\.

**To create a function \(console\)**

1. Sign in to the AWS Management Console and open the **Functions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home#/functions](https://console.aws.amazon.com/cloudfront/v2/home#/functions)\.

1. Choose **Create function**\.

1. Enter a function name, and then choose **Continue**\. The function name must be unique within the AWS account\.

1. On the function page, do the following:

   1. \(Optional\) Enter a description for the function\.

   1. Modify the function code\. The console provides a default function that can help you get started\. Or you can copy from [example code on GitHub](https://github.com/aws-samples/amazon-cloudfront-functions)\. For more information about writing function code, see the following:
      + [Writing function code \(programming model\)](writing-function-code.md)
      + [CloudFront Functions event structure](functions-event-structure.md)

   1. Choose **Save** to save the function\.

When successful, you see a banner at the top of the page that says ***Function name* saved successfully**\.

After you save your function, [you can test it](test-function.md)\.

## Creating functions \(AWS CLI\)<a name="create-function-cli"></a>

After you write the function code, you can create the function with the AWS CLI using the aws cloudfront create\-function command, as in the following example\. The following example command uses an input file to provide the function code to the create\-function command\. To use this example, do the following:
+ Replace *`ExampleFunction`* with a name for the function\.
+ Replace *`Example function`* with a comment to describe the function\.
+ Replace *`function.js`* with the name of the file that contains your function code\. Run the command from the directory that contains this file\.
+ Run the command on one line\. In the example, line breaks are provided to make the example more readable\.

```
aws cloudfront create-function \
    --name ExampleFunction \
    --function-config Comment="Example function",Runtime="cloudfront-js-1.0" \
    --function-code fileb://function.js
```

When the command is successful, you see output like the following that describes the function that was just created\.

**Note**  
The following output is formatted as a YAML string\. This feature is only available in version 2 of the AWS CLI\. For more information, see [Setting the AWS CLI output format]() in the *AWS Command Line Interface User Guide*\.

```
ETag: ETVABCEXAMPLE
FunctionSummary:
  FunctionConfig:
    Comment: Example function
    Runtime: cloudfront-js-1.0
  FunctionMetadata:
    CreatedTime: '2021-04-18T20:38:56.915000+00:00'
    FunctionARN: arn:aws:cloudfront::111122223333:function/ExampleFunction
    LastModifiedTime: '2021-04-18T20:38:56.915000+00:00'
    Stage: DEVELOPMENT
  Name: ExampleFunction
  Status: UNPUBLISHED
Location: https://cloudfront.amazonaws.com/2020-05-31/function/arn:aws:cloudfront::111122223333:function/ExampleFunction
```