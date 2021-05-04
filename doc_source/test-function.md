# Testing functions<a name="test-function"></a>

To test a function, you provide an *event object* that represents an HTTP request or response that your CloudFront distribution could receive in production\. CloudFront Functions does the following:

1. Runs the function, using the provided event object as input to the function\.

1. Returns the function’s result \(the modified event object\) along with any function logs or error messages and the function’s *compute utilization*\. The compute utilization is a number between 0 and 100 that indicates the amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a compute utilization of 35 means that the function completed in 35% of the maximum allowed time\.

Before you test a function, you must create the *event object* to test it with\. To create an event object, you have the following options:
+ Use the visual editor in the CloudFront console\. With the visual editor, you can create event objects with a graphical interface and use them to test your function\. After you create an event object, you can also copy a JSON representation of the event object to use for testing your function through other interfaces such as the AWS CLI or CloudFront API\. For more information, see [Testing functions \(console\)](#test-function-console)\.
+ Write the event object by hand in JSON format\. For more information about the structure of an event object, see [Event structure](functions-event-structure.md)\.
**Note**  
When you create an event object for testing a function, you can omit the `distributionDomainName`, `distributionId`, and `requestId` fields\. Also, make sure the names of headers, cookies, and query strings are lowercase\.

You can test a function in the CloudFront console or with the AWS CLI\.

## Testing functions \(console\)<a name="test-function-console"></a>

In the CloudFront console, you can create and save event objects with a graphical interface \(the visual editor\) and use them to test your function\. You can also copy a JSON representation of the event object to use for testing your function through other interfaces\.

**To create event objects and test a function \(console\)**

1. If you haven’t already done so, follow the steps to [create a function](create-function.md#create-function-console)\.

   To test a function, open the **Functions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home#/functions](https://console.aws.amazon.com/cloudfront/v2/home#/functions), and then choose the function that you want to test\.

1. On the function page, choose the **Test** tab\. Then do the following:

   1. Choose the **Event type**\. If your function modifies a request or generates a response based on the request, choose **Viewer Request**\. If it modifies a response, choose **Viewer Response**\.

   1. Choose the **Stage** of the function that you want to test, **Development** or **Live**\.  
![\[Test tab for a CloudFront function.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test.png)

1. Scroll down to use the console’s visual editor to create an event object that represents an HTTP request or response that you want to test\. When you choose **Viewer Response** for **Event type**, the visual editor adds fields so you can test a response\.

   For the request, you can choose the request method, and enter a URI path and a client IP address\. To add query strings or request headers, enter a key and a value, then choose **Add**\.

   For the response, you can add response headers and enter a response status code and status description\.

   To see a JSON representation of your event object, choose the **JSON** tab\. You can copy the JSON representation of the event object to use it for testing your function through other interfaces such as the AWS CLI or CloudFront API\.

   The following images show the visual editor and the JSON view for the same test event\.  
![\[Example test event for a CloudFront function, in the visual editor.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test-example-visual.png)  
![\[Example test event for a CloudFront function, in the JSON editor.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test-example-json.png)

1. To test your function with the event object that you created, choose **Test event**\. The console shows the output of the test, including function logs, query strings, and headers and cookies in the request \(and in the response, if applicable\)\. It also shows the *compute utilization*, which is a number between 0 and 100 that indicates the amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a compute utilization of 35 means that the function completed in 35% of the maximum allowed time\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test-result.png)

## Testing functions \(AWS CLI\)<a name="test-function-cli"></a>

After you create an event object, you can use it to test your function with the aws cloudfront test\-function command in the AWS CLI, as in the following example\. This example command uses an input file \(*`event-object.json`*\) to provide the event object to the command\. The following is an example of a simple event object for testing, in the *`event-object.json`* file\.

```
{
    "version": "1.0",
    "context": {
        "eventType": "viewer-request"
    },
    "viewer": {
        "ip": "198.51.100.11"
    },
    "request": {
        "method": "GET",
        "uri": "/example.png",
        "headers": {
            "host": {"value": "example.org"}
        }
    }
}
```

To use the following example command, do the following:
+ Replace *`ExampleFunction`* with the name of the function to test\.
+ Replace *`ETVABCEXAMPLE`* with the `ETag` value of the function whose code you’re testing\. To get this value, you can use the aws cloudfront describe\-function command\.
+ Replace *`event-object.json`* with the name of the file that contains the event object to test the function with\. Run the command from the same directory that contains this file\.
+ The following command tests the function in the `DEVELOPMENT` stage\. If you want to test the function in the `LIVE` stage instead, replace `DEVELOPMENT` with `LIVE`\.
+ Run the command on one line\. In the example, line breaks are provided to make the example more readable\.

```
aws cloudfront test-function \
    --name ExampleFunction \
    --if-match ETVABCEXAMPLE \
    --event-object fileb://event-object.json
    --stage DEVELOPMENT
```

When the command is successful, you see output like the following that shows the result of testing the function\.

**Note**  
The following output is formatted as a YAML string\. This feature is only available in version 2 of the AWS CLI\. For more information, see [Setting the AWS CLI output format]() in the *AWS Command Line Interface User Guide*\.

Note the following about the output:
+ `FunctionSummary` describes the function that was tested\.
+ `FunctionExecutionLogs` contains a list of log lines that the function wrote in `console.log()` statements \(if any\)\.
+ `ComputeUtilization` contains a number between 0 and 100 that indicates the amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a compute utilization of 35 means that the function completed in 35% of the maximum allowed time\.
+ If the function failed, `FunctionErrorMessage` contains the error message\.
+ `FunctionOutput` contains the event object that the function returned\. The following output shows that the function returned a response with HTTP status code `302 (Found)`, a `Location` header with the value `https://aws.amazon.com/cloudfront/`, and a `Cloudfront-Functions` header with the value `generated-by-CloudFront-Functions`\.

```
TestResult:
  ComputeUtilization: '21'
  FunctionErrorMessage: ''
  FunctionExecutionLogs: []
  FunctionOutput: '{"response":{"headers":{"cloudfront-functions":{"value":"generated-by-CloudFront-Functions"},"location":{"value":"https://aws.amazon.com/cloudfront/"}},"statusDescription":"Found","cookies":{},"statusCode":302}}'
  FunctionSummary:
    FunctionConfig:
      Comment: Example function
      Runtime: cloudfront-js-1.0
    FunctionMetadata:
      CreatedTime: '2021-04-18T20:38:56.915000+00:00'
      FunctionARN: arn:aws:cloudfront::111122223333:function/ExampleFunction
      LastModifiedTime: '2021-04-18T20:38:57.057000+00:00'
      Stage: DEVELOPMENT
    Name: ExampleFunction
    Status: UNPUBLISHED
```