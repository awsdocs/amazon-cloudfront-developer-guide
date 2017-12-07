# CloudWatch Metrics and CloudWatch Logs for Lambda Functions<a name="lambda-cloudwatch-metrics-logging"></a>

You can use CloudWatch metrics to monitor, in real time, the CloudFront requests that trigger Lambda functions\. You can also use CloudWatch Logs to get aggregate data\. There's no additional charge for metrics or logs\.


+ [CloudWatch Metrics](#lambda-cloudwatch-metrics)
+ [CloudWatch Logs](#lambda-cloudwatch-logs)

## CloudWatch Metrics<a name="lambda-cloudwatch-metrics"></a>

When you create a trigger for a CloudFront event, Lambda automatically begins to send metrics to CloudWatch\. Metrics are available for all Lambda regions\. The name of each metric is `/aws/lambda/us-east-1`\.*function\-name* where *function\-name* is the name that you gave to the function when you created it\. CloudWatch sends metrics to the region that's closest to the location where the function is executed\.

For more information about CloudWatch metrics, see the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

## CloudWatch Logs<a name="lambda-cloudwatch-logs"></a>

When you create a trigger, Lambda automatically starts to send data to CloudWatch Logs about the CloudFront requests that trigger Lambda functions\. You use CloudWatch Logs tools to access the logs\.

Lambda creates CloudWatch Logs log streams in the CloudWatch Logs regions closest to the locations where the function is executed\. The format of the name for each log stream is `/aws/lambda/us-east-1`\.*function\-name* where *function\-name* is the name that you gave to the function when you created it\. 

For information about the permissions required to send data to CloudWatch Logs, see [Setting IAM Permissions and Roles for Lambda@Edge](http://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-permissions) in the *IAM User Guide*\.

For information about adding logging to a Lambda function, see [Logging \(Node\.js\)](http://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-logging.html) in the *AWS Lambda Developer Guide*\.

For information about CloudWatch Logs limits, see [Limits](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch Logs User Guide*\.

Lambda@Edge throttles logs based on the request volume and the size of logs\.