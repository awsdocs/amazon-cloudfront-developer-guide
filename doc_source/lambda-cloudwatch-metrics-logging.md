# CloudWatch Metrics and CloudWatch Logs for Lambda Functions<a name="lambda-cloudwatch-metrics-logging"></a>

You can use CloudWatch metrics to monitor, in real time, the CloudFront requests that trigger Lambda functions\. You can also use CloudWatch Logs to get aggregate data\. There's no additional charge for metrics or logs\.

**Topics**
+ [CloudWatch Metrics](#lambda-cloudwatch-metrics)
+ [CloudWatch Logs](#lambda-cloudwatch-logs)

## CloudWatch Metrics<a name="lambda-cloudwatch-metrics"></a>

When you create a trigger for a CloudFront event, Lambda begins to send metrics to CloudWatch automatically\. Metrics are available for all Lambda Regions\. The name of each metric is `/aws/lambda/us-east-1`\.*function\-name*, where *function\-name* is the name that you gave to the function when you created it\. CloudWatch sends metrics to the Region that's closest to the location where the function is executed\.

For more information about CloudWatch metrics, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

## CloudWatch Logs<a name="lambda-cloudwatch-logs"></a>

When you create a trigger, Lambda automatically starts to send data to CloudWatch Logs about the CloudFront requests that trigger Lambda functions\. You can access the log files by using the CloudFront console in the AWS Management Console or use CloudWatch Logs tools directly to access the logs\.

Lambda creates CloudWatch Logs log streams in the CloudWatch Logs Regions closest to the locations where the function is executed\. The log group name is formatted as: `/aws/lambda/us-east-1`\.*function\-name*, where *function\-name* is the name that you gave to the function when you created it\. 

**Note**  
Lambda@Edge throttles logs based on the request volume and the size of logs\.

You must review CloudWatch log files in the correct AWS Region to see the log files created when CloudFront executed your Lambda function\. To see the Regions where your Lambda@Edge function is receiving traffic, view graphs of metrics for the function on the CloudFront console on the AWS Management Console\. Metrics are displayed for each AWS Region\. On the same page, you can choose a Region and then view log files for that Region so that you can investigate issues\. 

To learn more about how to use CloudWatch Logs with Lambda functions, see the following:
+ For more information about viewing graphs in the **Monitoring** section of the CloudFront console, see [Monitoring CloudFront with Amazon CloudWatch](monitoring-using-cloudwatch.md)\.
+ For information about the permissions required to send data to CloudWatch Logs, see [Setting IAM Permissions and Roles for Lambda@Edge](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-permissions) in the *IAM User Guide*\.
+ For information about adding logging to a Lambda function, see [Function Logging in Node\.js](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-logging.html) or [Function Logging in Python](https://docs.aws.amazon.com/lambda/latest/dg/python-logging.html) in the *AWS Lambda Developer Guide*\. 
+ For information about CloudWatch Logs quotas \(formerly known as limits\), see [CloudWatch Logs quotas](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch Logs User Guide*\.