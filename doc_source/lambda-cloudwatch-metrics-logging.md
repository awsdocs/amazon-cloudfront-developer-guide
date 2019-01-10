# CloudWatch Metrics and CloudWatch Logs for Lambda Functions<a name="lambda-cloudwatch-metrics-logging"></a>

You can use CloudWatch metrics to monitor, in real time, the CloudFront requests that trigger Lambda functions\. You can also use CloudWatch Logs to get aggregate data\. There's no additional charge for metrics or logs\.

**Topics**
+ [CloudWatch Metrics](#lambda-cloudwatch-metrics)
+ [CloudWatch Logs](#lambda-cloudwatch-logs)

## CloudWatch Metrics<a name="lambda-cloudwatch-metrics"></a>

When you create a trigger for a CloudFront event, Lambda begins to send metrics to CloudWatch automatically\. Metrics are available for all Lambda Regions\. The name of each metric is `/aws/lambda/us-east-1`\.*function\-name*, where *function\-name* is the name that you gave to the function when you created it\. CloudWatch sends metrics to the Region that's closest to the location where the function is executed\.

For more information about CloudWatch metrics, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

## CloudWatch Logs<a name="lambda-cloudwatch-logs"></a>

When you create a trigger, Lambda automatically starts to send data to CloudWatch Logs about the CloudFront requests that trigger Lambda functions\. You use CloudWatch Logs tools to access the logs\.

Lambda creates CloudWatch Logs log streams in the CloudWatch Logs regions closest to the locations where the function is executed\. The log group name is formatted as: `/aws/lambda/us-east-1`\.*function\-name*, where *function\-name* is the name that you gave to the function when you created it\. 

To help you find your log files, you can use the following Linux \(or OSX\) command\. Replace `function-name` with the name of your function\.

```
for i in `aws ec2 describe-regions | grep RegionName | awk '/[a-z]{2}-[a-z]+-[0-9]/g {gsub(/"/, "", $2); print $2 }'`; 
do aws logs describe-log-groups --log-group-name-prefix '/aws/lambda/us-east-1.' --region $i; done | grep arn: | grep function-name
```

**Note**  
Lambda@Edge throttles logs based on the request volume and the size of logs\.

To learn more about how to use CloudWatch Logs with Lambda functions, see the following:
+ For information about the permissions required to send data to CloudWatch Logs, see [Setting IAM Permissions and Roles for Lambda@Edge](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html#lambda-edge-permissions) in the *IAM User Guide*\.
+ For information about adding logging to a Lambda function, see [Logging \(Node\.js\)](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-logging.html) in the *AWS Lambda Developer Guide*\. 
+ For information about CloudWatch Logs limits, see [Limits](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) in the *Amazon CloudWatch Logs User Guide*\.