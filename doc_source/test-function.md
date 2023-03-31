# Testing functions<a name="test-function"></a>

You can test a [CloudFront Function](cloudfront-functions.md) to make sure that it works as intended before you deploy the function to the live stage \(production\)\. To test a function, you provide an *event object* that represents an HTTP request or response that your CloudFront distribution could receive in production\. CloudFront Functions does the following:

1. Runs the function, using the provided event object as input\.

1. Returns the function's result \(the modified event object\) along with any function logs or error messages and the function's *compute utilization*\. For more information about compute utilization, see [Understanding compute utilization](#compute-utilization)\.

Before you test a function, you must create the event object to test it with\. To create an event object, you have the following options:

**Use the visual editor in the CloudFront console**  
With the visual editor in the CloudFront console, you can create event objects by using a graphical interface, and then use them to test your function\. You can save up to 10 different event objects to test your function with different input\.  
After you create \(and optionally save\) an event object, you can also copy a JSON representation of the event object to use for testing your function through other interfaces such as the AWS CLI or CloudFront API\.

**Write the event object by hand in JSON format**  
You can use a text editor to create an event object by hand in JSON format\. For more information about the structure of an event object, see [Event structure](functions-event-structure.md)\.  
When you create an event object for testing a function, you can omit the `distributionDomainName`, `distributionId`, and `requestId` fields\. Also, make sure the names of headers, cookies, and query strings are lowercase\.

You can test a function in the CloudFront console or with the AWS CLI\.

------
#### [ Console ]

In the CloudFront console, you can create and save event objects with a graphical interface \(the visual editor\) and then use them to test your function\. You can also copy a JSON representation of the event object to use for testing your function through other interfaces\.

**To create event objects and test a function \(console\)**

1. If you haven't already done so, follow the steps to [create a function](create-function.md)\.

   To test a function, open the **Functions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home#/functions](https://console.aws.amazon.com/cloudfront/v3/home#/functions), and then choose the function that you want to test\.

1. On the function page, choose the **Test** tab\. Then do the following:

   1. Choose the **Event type**\. If your function modifies an HTTP request or generates a response based on the request, choose **Viewer request**\. If it modifies an HTTP response, choose **Viewer response**\.

   1. Choose the **Stage** of the function that you want to test, **Development** or **Live**\.

   1. Use the web form \(the visual editor\) to create an event object that represents an HTTP request or response that you want to test\. When you choose **Viewer response** for **Event type**, the visual editor adds additional fields for an HTTP response\.

      For the request, you can choose the request method and enter a URL path and a client IP address\. You can also add request headers, cookies, or query strings\.

      For the response, you can enter a response status code and status description, and add response headers and cookies\.

      To see a JSON representation of your event object, choose **Edit JSON**\. You can copy the JSON representation of the event object to use it for testing your function through other interfaces such as the AWS CLI or CloudFront API\.  
![\[Test tab for a CloudFront function showing a new test event.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test.png)

1. \(Optional\) To save the test event, choose **Save**\. You can save up to 10 test events per function\.

   To use a test event that you saved previously, choose the event from the **Select test event** menu\.

   If you make changes to a saved test event and want to discard them, choose **Undo changes**\. To save your changes, choose **Update**\.  
![\[Test tab for a CloudFront function showing a saved test event.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test-saved.png)

1. To test your function with the event object, choose **Test function**\.

   The console shows the output of the function, including function logs\. It also shows the compute utilization\. For more information, see [Understanding compute utilization](#compute-utilization)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/functions-test-result.png)

------
#### [ CLI ]

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
+ Replace *`ETVABCEXAMPLE`* with the `ETag` value of the function whose code you're testing\. To get this value, you can use the aws cloudfront describe\-function command\.
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

Note the following about the output:
+ `FunctionSummary` describes the function that was tested\.
+ `FunctionExecutionLogs` contains a list of log lines that the function wrote in `console.log()` statements \(if any\)\.
+ `ComputeUtilization` contains a number between 0 and 100 that indicates the amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a compute utilization of 35 means that the function completed in 35% of the maximum allowed time\. For more information, see [Understanding compute utilization](#compute-utilization)\.
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

------

## Understanding compute utilization<a name="compute-utilization"></a>

**Compute utilization** is the amount of time that the function took to run as a percentage of the maximum allowed time\. For example, a value of 35 means that the function completed in 35% of the maximum allowed time\.

If a function continuously exceeds the maximum allowed time, CloudFront throttles the function\. The following list explains the likelihood of a function getting throttled based on the compute utilization value\.

**Compute utilization value:**
+ **1 – 50** – The function is comfortably below the maximum allowed time and should run without throttling\.
+ **51 – 70** – The function is nearing the maximum allowed time\. Consider optimizing the function code\.
+ **71 – 100** – The function is very close to or exceeds the maximum allowed time\. CloudFront is likely to throttle this function if you associate it with a distribution\.