# Setting IAM Permissions and Roles for Lambda@Edge<a name="lambda-edge-permissions"></a>

To configure Lambda@Edge, you must set up specific IAM permissions and an IAM execution role\. Lambda@Edge also creates service\-linked roles to replicate Lambda functions to CloudFront Regions and to enable CloudWatch to use CloudFront log files\.

**Topics**
+ [IAM Permissions Required to Associate Lambda Functions with CloudFront Distributions](#lambda-edge-permissions-required)
+ [Function Execution Role for Service Principals](#lambda-edge-permissions-function-execution)
+ [Service\-linked roles for Lambda@Edge](#using-service-linked-roles)

## IAM Permissions Required to Associate Lambda Functions with CloudFront Distributions<a name="lambda-edge-permissions-required"></a>

In addition to the IAM permissions that you need to use AWS Lambda, the IAM user needs the following IAM permissions to associate Lambda functions with CloudFront distributions:
+ `lambda:GetFunction`

  Allows the user to get configuration information for the Lambda function and a presigned URL to download a \.zip file that contains the function\.

  For the resource, specify the ARN of the function version that you want to execute when a CloudFront event occurs, as shown in the following example:

  `arn:aws:lambda:us-east-1:123456789012:function:TestFunction:2`
+ `lambda:EnableReplication*`

  Adds a permission to the resource policy that gives the Lambda replication service permission to get function code and configuration\.
**Important**  
The asterisk \(`*`\) at the end of the permission is required: `lambda:EnableReplication*`

  For the resource, specify the ARN of the function \(without the version number\) that you want to execute when a CloudFront event occurs, as shown in the following example:

  `arn:aws:lambda:us-east-1:123456789012:function:TestFunction`
+ `iam:CreateServiceLinkedRole`

  Allows the user to create a service linked role that is used by Lambda@Edge to replicate Lambda functions in CloudFront\. After this role has been created by the first distribution you use with Lambda@Edge, you don’t need to add permission to other distributions that you use with Lambda@Edge\.
+ `cloudfront:UpdateDistribution` or `cloudfront:CreateDistribution`

  Use `cloudfront:UpdateDistribution` to update a distribution or `cloudfront:CreateDistribution` to create a distribution\.

For more information, see the following documentation:
+ [Identity and Access Management \(IAM\) in CloudFront](auth-and-access-control.md) in this guide\.
+ [Authentication and Access Control for AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/auth-and-access-control.html) in the *AWS Lambda Developer Guide*

## Function Execution Role for Service Principals<a name="lambda-edge-permissions-function-execution"></a>

You must create an IAM role that can be assumed by the service principals `lambda.amazonaws.com` and `edgelambda.amazonaws.com`\. This role is assumed by the service principals when they execute your function\. For more information, see [Creating roles and attaching policies \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions_create-policies.html) in the *IAM User Guide*\.

You add this role under the **Trust Relationship** tab in IAM \(do not add it under the **Permissions** tab\)\.

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

For information about the permissions that you need to grant to the execution role, see [Manage Permissions: Using an IAM Role \(Execution Role\)](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\. Note the following:
+ By default, whenever a CloudFront event triggers a Lambda function, data is written to CloudWatch Logs\. If you want to use these logs, the execution role needs permission to write data to CloudWatch Logs\. You can use the predefined AWSLambdaBasicExecutionRole to grant permission to the execution role\.

  For more information about CloudWatch Logs, see [CloudWatch Metrics and CloudWatch Logs for Lambda Functions](lambda-cloudwatch-metrics-logging.md)\.
+ If your Lambda function code accesses other AWS resources, such as reading an object from an S3 bucket, the execution role needs permission to perform that operation\. 

## Service\-linked roles for Lambda@Edge<a name="using-service-linked-roles"></a>

Lambda@Edge uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to a service\. Service\-linked roles are predefined by the service and include all of the permissions that the service requires to call other AWS services on your behalf\.

Lambda@Edge uses the following IAM service\-linked role:
+ **AWSServiceRoleForLambdaReplicator** – Lambda@Edge uses this role to allow Lambda@Edge to replicate functions to AWS Regions\.
+ **AWSServiceRoleForCloudFrontLogger** – CloudFront uses this role to push log files into your CloudWatch account, to help you to debug Lambda@Edge validation errors\.

When you first add a Lambda@Edge trigger in CloudFront, a role named AWSServiceRoleForLambdaReplicator is automatically created to allow Lambda@Edge to replicate functions to AWS Regions\. This role is required for using Lambda@Edge functions\. The ARN for the AWSServiceRoleForLambdaReplicator role looks like this:

`arn:aws:iam::123456789012:role/aws-service-role/replicator.lambda.amazonaws.com/AWSServiceRoleForLambdaReplicator`

The second role, named AWSServiceRoleForCloudFrontLogger, is created automatically when you add Lambda@Edge function association to allow CloudFront to push Lambda@Edge error log files to CloudWatch\. The ARN for the AWSServiceRoleForCloudFrontLogger role looks like this:

`arn:aws:iam::account_number:role/aws-service-role/logger.cloudfront.amazonaws.com/AWSServiceRoleForCloudFrontLogger`

A service\-linked role makes setting up and using Lambda@Edge easier because you don’t have to manually add the necessary permissions\. Lambda@Edge defines the permissions of its service\-linked roles, and only Lambda@Edge can assume the roles\. The defined permissions include the trust policy and the permissions policy\. The permissions policy cannot be attached to any other IAM entity\.

You must remove any associated CloudFront or Lambda@Edge resources before you can delete a service\-linked role\. This helps protect your Lambda@Edge resources by making sure that you don't remove a service\-linked role that is still required to access active resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-linked roles** column\.

### Service\-linked role permissions for Lambda@Edge<a name="slr-permissions"></a>

Lambda@Edge uses two service\-linked roles, named **AWSServiceRoleForLambdaReplicator** and **AWSServiceRoleForCloudFrontLogger**\. The following sections describe the permissions for each of these roles\.

#### Service\-linked role permissions for Lambda replicator<a name="slr-permissions-lambda-replicator"></a>

This service\-linked role allows Lambda to replicate Lambda@Edge functions to AWS Regions\.

The AWSServiceRoleForLambdaReplicator service\-linked role trusts the following service to assume the role: `replicator.lambda.amazonaws.com`

The role permissions policy allows Lambda@Edge to complete the following actions on the specified resources:
+ Action: `lambda:CreateFunction` on `arn:aws:lambda:*:*:function:*`
+ Action: `lambda:DeleteFunction` on `arn:aws:lambda:*:*:function:*`
+ Action: `lambda:DisableReplication` on `arn:aws:lambda:*:*:function:*`
+ Action: `iam:PassRole` on `all AWS resources`
+ Action: `cloudfront:ListDistributionsByLambdaFunction` on `all AWS resources`

#### Service\-linked role permissions for CloudFront logger<a name="slr-permissions-cloudfront-logger"></a>

This service\-linked role allows CloudFront to push log files into your CloudWatch account, to help you to debug Lambda@Edge validation errors\.

The AWSServiceRoleForCloudFrontLogger service\-linked role trusts the following service to assume the role: `logger.cloudfront.amazonaws.com`

The role permissions policy allows Lambda@Edge to complete the following actions on the specified resources:
+ Action: `logs:CreateLogGroup` on `arn:aws:logs:*:*:log-group:/aws/cloudfront/*`
+ Action: `logs:CreateLogStream` on `arn:aws:logs:*:*:log-group:/aws/cloudfront/*`
+ Action: `logs:PutLogEvents` on `arn:aws:logs:*:*:log-group:/aws/cloudfront/*`

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to delete the Lambda@Edge service\-linked roles\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

### Creating service\-linked roles for Lambda@Edge<a name="create-slr"></a>

You don’t typically manually create the service\-linked roles for Lambda@Edge\. The service creates the roles for you automatically in the following scenarios:
+ When you first create a trigger, the service creates a role, AWSServiceRoleForLambdaReplicator, if the role doesn’t already exist, that allows Lambda to replicate Lambda@Edge functions to AWS Regions\.

  If you delete the service\-linked role, the role will be created again when you add a new trigger for Lambda@Edge in a distribution\.
+ When you update or create a CloudFront distribution that has a Lambda@Edge association, the service creates a role, AWSServiceRoleForCloudFrontLogger, if the role doesn’t already exist, that allows CloudFront to push your log files to CloudWatch\.

  If you delete the service\-linked role, the role will be created again when you update or create a CloudFront distribution that has a Lambda@Edge association\.

If you must manually create these service\-linked roles, run the following commands using the AWS CLI:

**To create the AWSServiceRoleForLambdaReplicator role**  
`aws iam create-service-linked-role --aws-service-name replicator.lambda.amazonaws.com`

**To create the AWSServiceRoleForCloudFrontLogger role**  
`aws iam create-service-linked-role --aws-service-name logger.cloudfront.amazonaws.com`

### Editing Lambda@Edge service\-linked roles<a name="edit-slr"></a>

Lambda@Edge does not allow you to edit the AWSServiceRoleForLambdaReplicator or AWSServiceRoleForCloudFrontLogger service\-linked roles\. After the service has created a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of a role by using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

### Supported AWS Regions for CloudFront service\-linked roles<a name="slr-regions"></a>

CloudFront supports using service\-linked roles for Lambda@Edge in the following AWS Regions:
+ US East \(N\. Virginia\) – `us-east-1`
+ US East \(Ohio\) – `us-east-2`
+ US West \(N\. California\) – `us-west-1`
+ US West \(Oregon\) – `us-west-2`
+ Asia Pacific \(Mumbai\) – `ap-south-1`
+ Asia Pacific \(Seoul\) – `ap-northeast-2`
+ Asia Pacific \(Singapore\) – `ap-southeast-1`
+ Asia Pacific \(Sydney\) – `ap-southeast-2`
+ Asia Pacific \(Tokyo\) – `ap-northeast-1`
+ Europe \(Frankfurt\) – `eu-central-1`
+ Europe \(Ireland\) – `eu-west-1`
+ Europe \(London\) – `eu-west-2`
+ South America \(São Paulo\) – `sa-east-1`