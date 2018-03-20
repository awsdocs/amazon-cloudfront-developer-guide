# Testing and Debugging Lambda@Edge Functions<a name="lambda-edge-testing-debugging"></a>

You can test Lambda@Edge functions on the Lambda console by using test events modeled on the CloudFront events\. However, testing in the console only validates logic, and does not apply service limits that are specific to Lambda@Edge\. 

To monitor the execution of your function in CloudFront and help determine if it's working as expected, you can create logging statements for Lambda functions running on Lambda@Edge that will write to CloudWatch Logs\. 

When you check for the log files, be aware that log files are stored in the Region closest to the location where the function is executed\. So if you visit a website from, for example, London, you must change the Region to view the CloudWatch Logs for the London Region\.

For more information, see [CloudWatch Metrics and CloudWatch Logs for Lambda Functions](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-cloudwatch-metrics-logging.html)\.

For more information, see the following topics:
+ [Lambda@Edge Event Structure](lambda-event-structure.md)
+ [Requirements and Restrictions on Lambda Functions](lambda-requirements-limits.md)