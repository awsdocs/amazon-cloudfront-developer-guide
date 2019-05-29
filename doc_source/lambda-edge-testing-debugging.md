# Testing and Debugging Lambda@Edge Functions<a name="lambda-edge-testing-debugging"></a>

This topic includes sections that describe strategies for testing and debugging Lambda@Edge functions\. It's important to test your Lambda@Edge function code standalone, to make sure that it completes the intended task, and to do integration testing, to make sure that the function works correctly with CloudFront\. 

During integration testing or after your function has been deployed, you might need to debug CloudFront errors, such as HTTP 5xx errors\. Errors can be an invalid response returned from the Lambda function, execution errors when the function is triggered, or errors due to execution throttling by the Lambda service\. Sections in this topic share strategies for determining which type of failure is the issue, and then steps you can take to correct the problem\.

**Note**  
When you review CloudWatch log files or metrics when you're troubleshooting errors, be aware that they are displayed or stored in the Region closest to the location where the function executed\. So, if you have a website or web application with users in the United Kingdom, and you have a Lambda function associated with your distribution, for example, you must change the Region to view the CloudWatch metrics or log files for the London AWS Region\. For more information, see *Determining the Lambda@Edge Region* later in this topic\.

**Topics**
+ [Testing your Lambda@Edge Functions](#lambda-edge-testing-debugging-test-function)
+ [What Causes Lambda Function Errors in CloudFront](#lambda-edge-testing-debugging-function-errors)
+ [How to Determine the Type of Failure](#lambda-edge-testing-debugging-failure-type)
+ [Troubleshooting Invalid Lambda Function Responses \(Validation Errors\)](#lambda-edge-testing-debugging-troubleshooting-invalid-responses)
+ [Troubleshooting Lambda Function Execution Errors](#lambda-edge-testing-debugging-execution-errors)
+ [Determining the Lambda@Edge Region](#lambda-edge-testing-debugging-determine-region)
+ [Determining if Your Account Pushes Logs to CloudWatch](#lambda-edge-testing-debugging-cloudwatch-logs-enabled)

## Testing your Lambda@Edge Functions<a name="lambda-edge-testing-debugging-test-function"></a>

There are two steps to testing your Lambda function: standalone testing and integration testing\.

**Test standalone functionality**  
Before you add your Lambda function to CloudFront, make sure to test the functionality first by using the testing capabilities in the Lambda console or by using other methods\. For more information about testing in the Lambda console, see the *Invoke the Lambda Function and Verify Results, Logs, and Metrics* section in [ Create a Lambda Function with the Console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html) in the *AWS Lambda Developer Guide*\.

**Test your function's operation in CloudFront**  
It's important to complete integration testing, where your function is associated with a distribution and runs based on a CloudFront event\. Make sure that the function is triggered for the right event, and returns a response that is valid and correct for CloudFront\. For example, make sure that the event structure correct, that only valid headers are included, and so on\.  
As you iterate on integration testing with your function in the Lambda console, refer to the steps in the Lambda@Edge tutorial as you modify your code or change the CloudFront trigger that calls your function\. For example, make sure that you're working in a numbered version of your function, as described in this step of the tutorial: [ Step 4: Add a CloudFront Trigger to Run the Function](lambda-edge-how-it-works-tutorial.md#lambda-edge-how-it-works-tutorial-add-trigger)\.   
As you make changes and deploy them, be aware that it will take several minutes for your updated function and CloudFront triggers to replicate across all Regions\. This typically takes a few minutes but can take up to 15 minutes\.  
You can check to see if replication is finished by going to the CloudFront console and viewing your distribution:  
+ Go to the CloudFront console at https://console\.aws\.amazon\.com/cloudfront/\.
Check for the distribution status to change from **In Progress** back to **Deployed**, which means that your function has been replicated\. Then follow the steps in the next section to verify that the function works\.  
Be aware that testing in the console only validates logic, and does not apply service limits that are specific to Lambda@Edge\.

## What Causes Lambda Function Errors in CloudFront<a name="lambda-edge-testing-debugging-function-errors"></a>

After you’ve verified that your function logic works correctly, you might still see HTTP 5xx errors when your function runs in CloudFront\.

When you're using CloudFront, HTTP 5xx errors can be returned for a variety of reasons\. The following topic includes several troubleshooting steps you can take to help avoid HTTP 5xx errors in general: [Troubleshooting Error Responses from Your Origin](troubleshooting-response-errors.md)\.

If your CloudFront configuration includes a Lambda@Edge function, there are also specific steps you can take to track down errors with Lambda@Edge\. There are several reasons why a Lambda function might cause an HTTP 5xx error, and the troubleshooting steps you take depend on the type of error\. Errors can include the following

**A Lambda function execution error\.**  
An execution error results when CloudFront doesn’t get a response from Lambda because there are unhandled exceptions in the function or there’s an error in the code\. For example, if the code includes callback\(Error\)\. For more information, see [Lambda Function Errors](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-mode-exceptions.html) in the AWS Lambda Developer Guide\.

**An invalid Lambda function response is returned to CloudFront\.**  
After the function runs, CloudFront receives a response from Lambda\. An error is returned if the object structure of the response doesn't conform to the [Lambda@Edge Event Structure](lambda-event-structure.md), or the response contains invalid headers or other invalid fields\.

**The execution in CloudFront is throttled due to Lambda service limits\.**  
The Lambda service throttles executions in each Region, and returns an error if you reach the limit\. For more information, including how to request an increase in the limit, see [Limits on Lambda@Edge](cloudfront-limits.md#limits-lambda-at-edge)\. 

## How to Determine the Type of Failure<a name="lambda-edge-testing-debugging-failure-type"></a>

Identifying why CloudFront is returning an error can help you decide where to focus as you debug and resolve the problem\. CloudFront response headers and access logs include information that can help narrow down the issue\.

**CloudFront response headers**  
You can check CloudFront X\-Cache response headers to learn the type of failure\. Depending on the failure type, the X\-Cache header shows the following:  
+ `X-Cache: LambdaValidationError from CloudFront`
+ `X-Cache: LambdaExecutionError from CloudFront`
+ `X-Cache: LambdaLimitExceeded from CloudFront`

**CloudFront access logs**  
Alternatively, you can look for errors in the CloudFront access logs\. The `x-edge-result-type` field shows the following:  
+ `LambdaValidationError`
+ `LambdaExecutionError`
+ `LambdaLimitExceeded`

**Note**  
Note that there's a brief delay before the CloudFront access log files are updated\. CloudFront usually delivers log files to the Amazon S3 bucket that you've specified for logs within one hour of the events that appear in the log\. However, there are occasionally delays in posting log file entries, for up to 24 hours\. For more information, see [Timing of Log File Delivery](AccessLogs.md#access-logs-timing)\.

If you see a limit exceeded error, your function has reached a limit that the Lambda service imposes on executions in a Region\. For more information, including how to request an increase in the limit, see [Limits on Lambda@Edge](cloudfront-limits.md#limits-lambda-at-edge)\.

If you see an validation error or execution error, read the following sections for troubleshooting recommendations\.

## Troubleshooting Invalid Lambda Function Responses \(Validation Errors\)<a name="lambda-edge-testing-debugging-troubleshooting-invalid-responses"></a>

If you identify that your problem is a Lambda validation error, it means that your Lambda function is returning an invalid response to CloudFront\. Follow the guidance in this section to take steps to review your function and make sure that your response conforms to CloudFront requirements\. 

CloudFront validates the response from a Lambda function in two ways:
+ **The Lambda response must conform to the required object structure\.** Examples of bad object structure include the following: unparsable json, missing required fields, and an invalid object in the response\. For more information, see the [Lambda@Edge Event Structure](lambda-event-structure.md)\.
+ **The response must include only valid object values\.** An error will occur if the response includes a valid object but has values that are not supported\. Examples include the following: adding or updating blacklisted or read\-only headers \(see [Headers](lambda-requirements-limits.md#lambda-header-restrictions) in the *Requirements and Restrictions on Lambda Functions* topic\), exceeding body size limitations \(see *Limits on the Size of the Generated Response* in the Lambda Response [Errors](lambda-generating-http-responses-in-requests.md#lambda-generating-http-responses-errors) topic\), and invalid characters or values \(see the [Lambda@Edge Event Structure](lambda-event-structure.md)\)\.

When Lambda returns an invalid response to CloudFront, error messages are written to log files which CloudFront pushes to CloudWatch in the Region of where the Lambda function executed\. It's the default behavior to send the log files to CloudWatch when there's an invalid response\. However, if you associated a Lambda function with CloudFront before the functionality was released, it might not be enabled for your function\. For more information, see *Determine if Your Account Pushes Logs to CloudWatch* later in the topic\.

CloudFront pushes log files to the Region corresponding to where your function executed, in the log group that’s associated with your distribution\. Log groups have the following format: `/aws/cloudfront/LambdaEdge/DistributionId`, where *DistributionId* is your distribution’s ID\. To determine the Region where you can find the CloudWatch log files, see *Determining the Lambda@Edge Region* later in this topic\.

If the error is reproducible, you can create a new request that results in the error and then find the request id in a failed CloudFront response \(`X-Amz-Cf-Id` header\) to locate a single failure in log files\. The log file entry includes information that can help you identify why the error is being returned, and also lists the corresponding Lambda request id so you can analyze the root cause in the context of a single request\.

If an error is intermittent, you can use CloudFront access logs to find the request id for a request that has failed, and then search CloudWatch logs for the corresponding error messages\. For more information, see the previous section, *Determining the Type of Failure*\.

## Troubleshooting Lambda Function Execution Errors<a name="lambda-edge-testing-debugging-execution-errors"></a>

If the problem is a Lambda execution error, it can be helpful to create logging statements for Lambda functions, to write messages to CloudWatch log files that monitor the execution of your function in CloudFront and determine if it's working as expected\. Then you can search for those statements in the CloudWatch log files to verify that your function is working\.

**Note**  
Even if you haven't changed your Lambda@Edge function, updates to the Lambda function execution environment might affect it and could return an execution error\. For information about testing and migrating to a later version, see [Upcoming updates to the AWS Lambda and AWS Lambda@Edge execution environment\.](https://aws.amazon.com/blogs/compute/upcoming-updates-to-the-aws-lambda-execution-environment/)

## Determining the Lambda@Edge Region<a name="lambda-edge-testing-debugging-determine-region"></a>

You must review CloudWatch log files in the correct Region to see the log files created when CloudFront executed your Lambda function\. You can run the following AWS CLI script to determine a list of Regions where your Lambda@Edge function might be receiving traffic\. Those Regions are where you should look for the CloudWatch log files to help you debug errors\. Before you run the script, please review the following instructions to install and use AWS CLI: [AWS Command Line Interface](https://aws.amazon.com/cli/)\.

```
FUNCTION_NAME=function_name_without_qualifiers
for region in $(aws --output text  ec2 describe-regions | cut -f 3) 
do
    for loggroup in $(aws --output text  logs describe-log-groups --log-group-name "/aws/lambda/us-east-1.$FUNCTION_NAME" --region $region --query 'logGroups[].logGroupName')
    do
        echo $region $loggroup
    done
done
```

The response from this script shows you the Regions where your replicated Lambda@Edge function is, where it has executed at least once\.

## Determining if Your Account Pushes Logs to CloudWatch<a name="lambda-edge-testing-debugging-cloudwatch-logs-enabled"></a>

By default, CloudFront enables logging invalid Lambda function responses, and pushes the log files to CloudWatch by using one of the [Service\-Linked Roles for Lambda@Edge](lambda-edge-permissions.md#using-service-linked-roles)\. If you have Lambda@Edge functions that you added to CloudFront before the invalid Lambda function response log feature was released, logging is enabled when you next update your Lambda@Edge configuration, for example, by adding a CloudFront trigger\.

You can verify that pushing the log files to CloudWatch is enabled for your account by doing the following:
+ **Check to see if the logs appear in CloudWatch\.** Make sure that you look in the Region where the Lambda@Edge function executed\. For more information, see [ Determining the Lambda@Edge Region](#lambda-edge-testing-debugging-determine-region)\.
+ **Determine if the related service\-linked role exists in your account in IAM\.** To do this, open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/), and then choose **Roles** to view the list of service\-linked roles for your account\. Look for the following role: `AWSServiceRoleForCloudFrontLogger`\.