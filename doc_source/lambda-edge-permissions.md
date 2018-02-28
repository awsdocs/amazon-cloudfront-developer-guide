# Setting IAM Permissions and Roles for Lambda@Edge<a name="lambda-edge-permissions"></a>

To configure Lambda@Edge, you need IAM permissions and an IAM execution role:

**IAM Permissions Required to Associate Lambda Functions with CloudFront Distributions**  
In addition to the IAM permissions that you need to use AWS Lambda, the IAM user needs the following IAM permissions to associate Lambda functions with CloudFront distributions:  

+ `lambda:GetFunction`

  For the resource, specify the ARN of the function version that you want to execute when a CloudFront event occurs, as shown in the following example:

  `arn:aws:lambda:us-east-1:123456789012:function:TestFunction:2`

+ `lambda:EnableReplication*`

  For the resource, specify the ARN of the function version that you want to execute when a CloudFront event occurs, as shown in the following example:

  `arn:aws:lambda:us-east-1:123456789012:function:TestFunction:2`

+ `iam:CreateServiceLinkedRole`

  Used to create a service linked role used by Lambda@Edge to replicate Lambda functions in CloudFront\. After this role has been created by the first distribution you use with Lambda@Edge, you do not need to add permission to other distributions you use with Lambda@Edge\. 

+ `cloudfront:UpdateDistribution` or `cloudfront:CreateDistribution`

  Choose `cloudfront:UpdateDistribution` to update a distribution or `cloudfront:CreateDistribution` to create a distribution\.
For more information, see the following documentation:  

+ [Authentication and Access Control for CloudFront](auth-and-access-control.md) in this guide\.

+ [Authentication and Access Control for AWS Lambda](http://docs.aws.amazon.com/lambda/latest/dg/auth-and-access-control.html) in the *AWS Lambda Developer Guide*

**Execution Role**  
You must create an IAM role that can be assumed by the service principals `lambda.amazonaws.com` and `edgelambda.amazonaws.com`\. This role is assumed by the service principals when they execute your function\. For more information, see [Creating the Roles and Attaching the Policies \(Console\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#access_policies_job-functions_create-policies) in the topic "AWS Managed Policies for Job Functions" in the *IAM User Guide*\.  
Here's an example role trust policy:  

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": {
            "Service": [
               "lambda.amazonaws.com",
               "edgelambda.amazonaws.com"
            ]
         },
         "Action": "sts:AssumeRole"
      }
   ]
}
```
For information about the permissions that you need to grant to the execution role, see [Manage Permissions: Using an IAM Role \(Execution Role\)](http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\. Note the following:  

+ By default, whenever a CloudFront event triggers a Lambda function, data is written to CloudWatch Logs\. If you want to use these logs, the execution role needs permission to write data to CloudWatch Logs\. You can use the predefined AWSLambdaBasicExecutionRole to grant permission to the execution role\.

  For more information about CloudWatch Logs, see [CloudWatch Metrics and CloudWatch Logs for Lambda Functions](lambda-cloudwatch-metrics-logging.md)\. 

+ If your Lambda function code accesses other AWS resources, such as reading an object from an S3 bucket, the execution role needs permission to perform that operation\. 

**AWSServiceRoleForLambdaReplicator Role**  
When you first create a trigger, a role named AWSServiceRoleForLambdaReplicator is automatically created to allow Lambda to replicate Lambda@Edge functions to AWS Regions\. This role is required to use Lambda@Edge\. The ARN for the AWSServiceRoleForLambdaReplicator role looks like this:  
`arn:aws:iam::123456789012:role/aws-service-role/replicator.lambda.amazonaws.com/AWSServiceRoleForLambdaReplicator`