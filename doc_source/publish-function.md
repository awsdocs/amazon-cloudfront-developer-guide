# Publishing functions<a name="publish-function"></a>

Publishing a function copies it from the `DEVELOPMENT` stage to `LIVE`\.

**Important**  
When you publish a function, all cache behaviors that are associated with the function automatically start using the newly published copy, as soon as the distributions finish deploying\.

If no cache behaviors are associated with the function, publishing it enables you to associate it with a cache behavior\. You can only associate cache behaviors with functions that are in the `LIVE` stage\.

You can publish a function in the CloudFront console or with the AWS CLI\.

## Publishing functions \(console\)<a name="publish-function-console"></a>

To publish your function, you can use the CloudFront console\. The console also shows the CloudFront distributions that are associated with the function\.

**To publish a function \(console\)**

1. To publish a function, open the **Functions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home#/functions](https://console.aws.amazon.com/cloudfront/v2/home#/functions), and then choose the function that you want to publish\.

1. On the function page, choose the **Publish** tab\. Then choose the **Publish** button \(or, if your function is already attached to one or more cache behaviors, the **Publish and update** button\)\.

1. \(Optional\) To see the distributions that are associated with the function, choose **Associated CloudFront distributions** to expand that section\.

When successful, you see a banner at the top of the page that says ***Function name* published successfully**\. You can also choose the **Build** tab and then choose **Live** to see the live version of the function code\.

## Publishing functions \(AWS CLI\)<a name="publish-function-cli"></a>

To publish a function, use the aws cloudfront publish\-function command in the AWS CLI, as in the following example\. To use this example, do the following:
+ Replace *`ExampleFunction`* with the name of the function that you’re publishing\.
+ Replace *`ETVXYZEXAMPLE`* with the `ETag` value of the function that you’re publishing\. To get this value, you can use the aws cloudfront describe\-function command\.
+ Run the command on one line\. In the example, line breaks are provided to make the example more readable\.

```
aws cloudfront publish-function \
    --name ExampleFunction \
    --if-match ETVXYZEXAMPLE
```

When the command is successful, you see output like the following that describes the function that was just published\.

**Note**  
The following output is formatted as a YAML string\. This feature is only available in version 2 of the AWS CLI\. For more information, see [Setting the AWS CLI output format]() in the *AWS Command Line Interface User Guide*\.

```
FunctionSummary:
  FunctionConfig:
    Comment: Example function
    Runtime: cloudfront-js-1.0
  FunctionMetadata:
    CreatedTime: '2021-04-18T21:24:21.314000+00:00'
    FunctionARN: arn:aws:cloudfront::111122223333:function/ExampleFunction
    LastModifiedTime: '2021-04-19T23:41:15.389000+00:00'
    Stage: LIVE
  Name: ExampleFunction
  Status: UNASSOCIATED
```