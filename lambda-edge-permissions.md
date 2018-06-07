# Setting IAM Permissions and Roles for Lambda@Edge<a name="lambda-edge-permissions"></a>

Specific IAM permissions and an IAM execution role are required so that you can configure Lambda@Edge\. Lambda@Edge also creates a service\-linked role to replicate Lambda functions to CloudFront Regions\.

**Topics**
+ [IAM Permissions Required to Associate Lambda Functions with CloudFront Distributions](#lambda-edge-permissions-required)
+ [Function Execution Role for Service Principals](#lambda-edge-permissions-function-execution)
+ [Service\-Linked Role for Lambda@Edge](#using-service-linked-roles)

## IAM Permissions Required to Associate Lambda Functions with CloudFront Distributions<a name="lambda-edge-permissions-required"></a>

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

## Function Execution Role for Service Principals<a name="lambda-edge-permissions-function-execution"></a>

You must create an IAM role that can be assumed by the service principals `lambda.amazonaws.com` and `edgelambda.amazonaws.com`\. This role is assumed by the service principals when they execute your function\. For more information, see [Creating the Roles and Attaching the Policies \(Console\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html#access_policies_job-functions_create-policies) in the topic "AWS Managed Policies for Job Functions" in the *IAM User Guide*\.

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

For information about the permissions that you need to grant to the execution role, see [Manage Permissions: Using an IAM Role \(Execution Role\)](http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\. Note the following:
+ By default, whenever a CloudFront event triggers a Lambda function, data is written to CloudWatch Logs\. If you want to use these logs, the execution role needs permission to write data to CloudWatch Logs\. You can use the predefined AWSLambdaBasicExecutionRole to grant permission to the execution role\.

  For more information about CloudWatch Logs, see [CloudWatch Metrics and CloudWatch Logs for Lambda Functions](lambda-cloudwatch-metrics-logging.md)\. 
+ If your Lambda function code accesses other AWS resources, such as reading an object from an S3 bucket, the execution role needs permission to perform that operation\. 

## Service\-Linked Role for Lambda@Edge<a name="using-service-linked-roles"></a>

Lambda@Edge uses an AWS Identity and Access Management \(IAM\)[ service\-linked role](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Lambda@Edge\. The service\-linked role is predefined by Lambda@Edge and includes all of the permissions that the service requires so it can call other AWS services on your behalf\. 

When you first add a Lambda@Edge trigger in CloudFront, a role named AWSServiceRoleForLambdaReplicator is automatically created to allow Lambda@Edge to replicate functions to AWS Regions\. This role is required for using Lambda@Edge functions\. The ARN for the AWSServiceRoleForLambdaReplicator role looks like this:

`arn:aws:iam::123456789012:role/aws-service-role/replicator.lambda.amazonaws.com/AWSServiceRoleForLambdaReplicator`

A service\-linked role makes setting up Lambda@Edge easier because you don’t have to manually add the necessary permissions\. Lambda@Edge defines the permissions of its service\-linked role, and only Lambda@Edge can assume the role\. The defined permissions include the trust policy and the permissions policy\. The permissions policy cannot be attached to any other IAM entity\.

You must remove any associated CloudFront or Lambda@Edge resources before you can delete the service\-linked role\. This protects your Lambda@Edge resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\.

### Service\-Linked Role Permissions for Lambda@Edge<a name="slr-permissions"></a>

Lambda@Edge uses the service\-linked role named **AWSServiceRoleForLambdaReplicator**\. This service\-linked role allows Lambda to replicate Lambda@Edge functions to AWS Regions\.

The AWSServiceRoleForLambdaReplicator service\-linked role trusts the following service to assume the role:
+ `replicator.lambda.amazonaws.com`

The role permissions policy allows Lambda@Edge to complete the following actions on the specified resources:
+ Action: `lambda:CreateFunction` on `arn:aws:lambda:*:*:function:*`
+ Action: `lambda:DeleteFunction` on `arn:aws:lambda:*:*:function:*`
+ Action: `lambda:DisableReplication` on `arn:aws:lambda:*:*:function:*`
+ Action: `iam:PassRole` on `all AWS resources`
+ Action: `cloudfront:ListDistributionsByLambdaFunction` on `all AWS resources`

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

### Creating the Service\-Linked Role for Lambda@Edge<a name="create-slr"></a>

You don't need to manually create the service\-linked role for Lambda@Edge\. When you first create a trigger, the service creates a role that allows Lambda to replicate Lambda@Edge functions to AWS Regions\.

If you delete the service\-linked role, the role will be created again when you add a new trigger for Lambda@Edge in a distribution\.

### Editing the Service\-Linked Role for Lambda@Edge<a name="edit-slr"></a>

Lambda@Edge does not allow you to edit the AWSServiceRoleForLambdaReplicator service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

### Deleting the Service\-Linked Role for Lambda@Edge<a name="delete-slr"></a>

If you no longer need to use Lambda@Edge, we recommend that you delete the service\-linked role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up the Lambda@Edge resources in your account before you can manually delete the role\.

**To delete Lambda@Edge resources used by the AWSServiceRoleForLambdaReplicator**

To remove the service\-linked role, you must remove all Lambda@Edge associations from your distributions\. To do this, update your distributions to remove all Lambda@Edge function triggers, or remove the distributions that use Lambda@Edge functions\. For more information, see [Deleting Lambda@Edge Functions and Replicas](lambda-edge-delete-replicas.md)\.

After you have removed all Lambda@Edge function associations from your distributions and CloudFront has removed the function replicas from AWS locations, then you can use the CloudFront console to delete the AWSServiceRoleForLambdaReplicator service\-linked role\.

**Note**  
If CloudFront hasn't finished updating, the service\-linked role deletion might fail\. If that happens, wait for a few minutes, and then try the steps again\.

**To manually delete the Lambda@Edge service\-linked role \(CloudFront console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. On the **CloudFront Distributions** page, click the avatar in the upper right\.  
![\[Screenshot that shows the avatar in the CloudFront.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/lambda-delete-slr-role-click-avatar.png)

1. Choose **Delete**\.  
![\[Screenshot that shows the dialog for deleting the Lambda service-linked role in the CloudFront.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/lambda-delete-slr-role.png)