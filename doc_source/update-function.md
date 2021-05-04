# Updating functions<a name="update-function"></a>

In the course of [testing a function](test-function.md), you might want to update the function code\. When you update a function’s code, it only affects the copy of the function that’s in the `DEVELOPMENT` stage\. The function code in the `LIVE` stage doesn’t change\. To update the code in the `LIVE` stage, you [publish the function](publish-function.md), which copies it from the `DEVELOPMENT` stage to `LIVE`\.

You can update a function’s code in the CloudFront console or with the AWS CLI\.

## Updating functions \(console\)<a name="update-function-console"></a>

To update your function code, you can use the visual code editor in the CloudFront console\.

**To update function code \(console\)**

1. To update an existing function, open the **Functions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home#/functions](https://console.aws.amazon.com/cloudfront/v2/home#/functions), and then choose the function that you want to update\.

1. On the function page, choose the **Build** tab\. Then make sure that the **Development** version of the function is chosen\. You cannot update a function’s live version\.

1. Use the console’s code editor to update your function code\.

   As you update your code, the code editor might display errors or warnings about the JavaScript syntax\.

1. When you’re finished updating your function code, choose **Save**\.

When successful, you see a banner at the top of the page that says ***Function name* saved successfully**\.

## Updating functions \(AWS CLI\)<a name="update-function-cli"></a>

After you update your function code locally, you can use the AWS CLI to update it in CloudFront Functions with the aws cloudfront update\-function command, as in the following example\. This example command uses input files to provide the function configuration and the updated function code to the command\. To use this example, do the following:
+ Replace *`ExampleFunction`* with the name of the function whose code you’re updating\.
+ Replace *`Example function`* with a comment to describe the function\.
+ Replace *`function.js`* with the name of the file that contains the updated function code\. Run the command from the same directory that contains this file\.
+ Replace *`ETVABCEXAMPLE`* with the `ETag` value of the function whose code you’re updating\. To get this value, you can use the aws cloudfront describe\-function command\.
+ Run the command on one line\. In the example, line breaks are provided to make the example more readable\.

```
aws cloudfront update-function \
    --name ExampleFunction \
    --function-config Comment="Example function",Runtime="cloudfront-js-1.0" \
    --function-code fileb://function.js \
    --if-match ETVABCEXAMPLE
```

When the command is successful, you see output like the following that describes the function that was just updated\.

**Note**  
The following output is formatted as a YAML string\. This feature is only available in version 2 of the AWS CLI\. For more information, see [Setting the AWS CLI output format]() in the *AWS Command Line Interface User Guide*\.

```
ETag: ETVXYZEXAMPLE
FunctionSummary:
  FunctionConfig:
    Comment: Example function
    Runtime: cloudfront-js-1.0
  FunctionMetadata:
    CreatedTime: '2021-04-18T20:38:56.915000+00:00'
    FunctionARN: arn:aws:cloudfront::111122223333:function/ExampleFunction
    LastModifiedTime: '2021-04-19T23:41:15.389000+00:00'
    Stage: DEVELOPMENT
  Name: ExampleFunction
  Status: UNPUBLISHED
```