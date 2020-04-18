# CloudWatch Metrics and CloudWatch Logs for Lambda Functions<a name="lambda-cloudwatch-metrics-logging"></a>

You can use CloudWatch metrics to monitor, in real time, problems with your Lambda@Edge functions\. You can also use CloudWatch Logs to get the function logs\. There’s no additional charge for metrics or logs\.

**Topics**
+ [CloudWatch Metrics](#lambda-cloudwatch-metrics)
+ [CloudWatch Logs](#lambda-cloudwatch-logs)

## CloudWatch Metrics<a name="lambda-cloudwatch-metrics"></a>

When you create a trigger for a CloudFront event, Lambda begins to send metrics to CloudWatch automatically\. Metrics are available for all Lambda Regions, but to view metrics in the CloudWatch console or get the metric data from the CloudWatch API, you must use the US East \(N\. Virginia\) Region \(us\-east\-1\)\. The metric group name is formatted as: `AWS/CloudFront/distribution-ID`, where *distribution\-ID* is the ID of the CloudFront distribution that the Lambda@Edge function is associated with\.

For more information about CloudWatch metrics, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

## CloudWatch Logs<a name="lambda-cloudwatch-logs"></a>

Lambda automatically sends function logs to CloudWatch Logs\. You can access the log files using the CloudWatch console or the CloudWatch Logs API\.

Lambda creates CloudWatch Logs log streams in the AWS Regions closest to the location where the function is executed\. The log group name is formatted as: `/aws/lambda/us-east-1`\.*function\-name*, where *function\-name* is the name that you gave to the function when you created it\.

**Note**  
Lambda@Edge throttles logs based on the request volume and the size of logs\.

You must review CloudWatch log files in the correct AWS Region to see your Lambda@Edge function log files\. To see the Regions where your Lambda@Edge function is receiving traffic, view graphs of metrics for the function on the CloudFront console\. Metrics are displayed for each AWS Region\. On the same page, you can choose a Region and then view log files for that Region so that you can investigate issues\.

To learn more about how to use CloudWatch Logs with Lambda@Edge functions, see the following:
+ For more information about viewing graphs in the **Monitoring** section of the CloudFront console, see [Monitoring CloudFront with Amazon CloudWatch](monitoring-using-cloudwatch.md)\.
+ For information about the permissions required to send data to CloudWatch Logs, see [Setting IAM Permissions and Roles for Lambda@Edge](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-permissions) in the *IAM User Guide*\.
+ For information about adding logging to a Lambda function, see [Function Logging in Node\.js](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-logging.html) or [Function Logging in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-logging.html) in the *AWS Lambda Developer Guide*\. 
+ For information about CloudWatch Logs quotas \(formerly known as limits\), see [CloudWatch Logs quotas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch Logs User Guide*\.