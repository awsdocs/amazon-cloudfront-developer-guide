# HTTP 503 status code \(Lambda limit exceeded\)<a name="http-503-lambda-limit-execeeded-error"></a>

If you're using Lambda@Edge, an HTTP 503 status code can indicate that the Lambda service returned an error\. The error might be caused by one of the following:
+ The number of function executions exceeded one of the quotas \(formerly known as limits\) that Lambda sets to throttle executions in an AWS Region \(concurrent executions or invocation frequency\)\.
+ The function exceeded the Lambda function timeout quota\.

For more information about the AWS Lambda quotas, see [Lambda quotas](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html) in the *AWS Lambda Developer Guide*\. For more information about troubleshooting Lambda@Edge errors, see [Testing and debugging Lambda@Edge functions](lambda-edge-testing-debugging.md)\.