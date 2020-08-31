# Using Identity\-Based Policies \(IAM Policies\) for CloudFront<a name="access-control-managing-permissions"></a>

This topic provides examples of identity\-based policies that demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on CloudFront resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options to manage access to your CloudFront resources\. For more information, see [Overview of Managing Access Permissions to Your CloudFront Resources](access-control-overview.md)\. 

**Topics**
+ [Permissions Required to Use the CloudFront Console](#console-required-permissions)
+ [AWS Managed \(Predefined\) Policies for CloudFront](#access-policy-examples-aws-managed)
+ [Customer Managed Policy Examples](#access-policy-examples-for-sdk-cli)

The following shows a permissions policy\. The `Sid`, or statement ID, is optional\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "AllowAllCloudFrontPermissions",
         "Effect": "Allow",
         "Action": ["cloudfront:*"],
         "Resource": "*"
      }
   ]
}
```

The policy grants permissions to perform all CloudFront operations, which is sufficient to access CloudFront programmatically\. If you're using the console to access CloudFront, see [Permissions Required to Use the CloudFront Console](#console-required-permissions)\. 

For a list of actions and the ARN that you specify to grant or deny permission to use each action, see [CloudFront API Permissions: Actions, Resources, and Conditions Reference](cf-api-permissions-ref.md)\.

## Permissions Required to Use the CloudFront Console<a name="console-required-permissions"></a>

To grant full access to the CloudFront console, you grant the permissions in the following permissions policy: 

```
{
   "Version": "2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "acm:ListCertificates", 
            "cloudfront:*", 
            "cloudwatch:DescribeAlarms",
            "cloudwatch:PutMetricAlarm",
            "cloudwatch:GetMetricStatistics",
            "elasticloadbalancing:DescribeLoadBalancers",
            "iam:ListServerCertificates",
            "sns:ListSubscriptionsByTopic",
            "sns:ListTopics",
            "waf:GetWebACL",
            "waf:ListWebACLs"
         ],
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":[
            "s3:ListAllMyBuckets",
            "s3:PutBucketPolicy"
         ],
         "Resource":"arn:aws:s3:::*"
      }
   ]
}
```

Here's why the permissions are required:

**`acm:ListCertificates`**  
When you're creating and updating web distributions by using the CloudFront console and you want to configure CloudFront to require HTTPS between the viewer and CloudFront or between CloudFront and the origin, lets you view a list of ACM certificates\.  
This permission isn't required if you aren't using the CloudFront console\.

**`cloudfront:*`**  
Lets you perform all CloudFront actions\.

**`cloudwatch:DescribeAlarms` and `cloudwatch:PutMetricAlarm`**  
Let you create and view CloudWatch alarms in the CloudFront console\. See also `sns:ListSubscriptionsByTopic` and `sns:ListTopics`\.  
These permissions aren't required if you aren't using the CloudFront console\.

**`cloudwatch:GetMetricStatistics`**  
Lets CloudFront render CloudWatch metrics in the CloudFront console\.  
This permission isn't required if you aren't using the CloudFront console\.

**`elasticloadbalancing:DescribeLoadBalancers`**  
When creating and updating web distributions, lets you view a list of Elastic Load Balancing load balancers in the list of available origins\.  
This permission isn't required if you aren't using the CloudFront console\.

**`iam:ListServerCertificates`**  
When you're creating and updating web distributions by using the CloudFront console and you want to configure CloudFront to require HTTPS between the viewer and CloudFront or between CloudFront and the origin, lets you view a list of certificates in the IAM certificate store\.  
This permission isn't required if you aren't using the CloudFront console\.

**`s3:ListAllMyBuckets`**  
When you're creating and updating web and RTMP distributions, lets you perform the following operations:  
+ View a list of S3 buckets in the list of available origins
+ View a list of S3 buckets that you can save access logs in
This permission isn't required if you aren't using the CloudFront console\.

**`S3:PutBucketPolicy`**  
When you're creating or updating distributions that restrict access to S3 buckets, lets a user update the bucket policy to grant access to the CloudFront origin access identity\. For more information, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\.  
This permission isn't required if you aren't using the CloudFront console\.

**`sns:ListSubscriptionsByTopic` and `sns:ListTopics`**  
When you create CloudWatch alarms in the CloudFront console, lets you choose an SNS topic for notifications\.  
These permissions aren't required if you aren't using the CloudFront console\.

**`waf:GetWebACL` and `waf:ListWebACLs`**  
Lets you view a list of AWS WAF web ACLs in the CloudFront console\.  
These permissions aren't required if you aren't using the CloudFront console\.

## AWS Managed \(Predefined\) Policies for CloudFront<a name="access-policy-examples-aws-managed"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant necessary permissions for common use cases so that you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\. For CloudFront, IAM provides two managed policies: 
+ **CloudFrontFullAccess** – Grants full access to CloudFront resources\.
**Important**  
If you want CloudFront to create and save access logs, you need to grant additional permissions\. For more information, see [Permissions Required to Configure Standard Logging and to Access Your Log Files](AccessLogs.md#AccessLogsBucketAndFileOwnership)\.
+ **CloudFrontReadOnlyAccess** – Grants read\-only access to CloudFront resources\.

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\. You can also create your own custom IAM policies to allow permissions for CloudFront API operations\. You can attach these custom policies to the IAM users or groups that require those permissions\.

## Customer Managed Policy Examples<a name="access-policy-examples-for-sdk-cli"></a>

You can create your own custom IAM policies to allow permissions for CloudFront API actions\. You can attach these custom policies to the IAM users or groups that require the specified permissions\. These policies work when you are using the CloudFront API, the AWS SDKs, or the AWS CLI\. The following examples show permissions for a few common use cases\. For the policy that grants a user full access to CloudFront, see [Permissions Required to Use the CloudFront Console](#console-required-permissions)\.

**Topics**
+ [Example 1: Allow Read Access to All Web Distributions](#access-policy-example-allow-read-hosted-zones)
+ [Example 2: Allow Creation, Updating, and Deletion of Web Distributions](#access-policy-example-allow-create-delete-hosted-zones)
+ [Example 3: Allow Creation and Listing of Invalidations](#access-policy-example-allow-change-resource-record-sets)

### Example 1: Allow Read Access to All Web Distributions<a name="access-policy-example-allow-read-hosted-zones"></a>

The following permissions policy grants the user permissions to view all web distributions in the CloudFront console:

```
{
   "Version": "2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "acm:ListCertificates", 
            "cloudfront:GetDistribution",
            "cloudfront:GetDistributionConfig",
            "cloudfront:ListDistributions",
            "cloudfront:ListCloudFrontOriginAccessIdentities",
            "elasticloadbalancing:DescribeLoadBalancers",
            "iam:ListServerCertificates",
            "sns:ListSubscriptionsByTopic",
            "sns:ListTopics",
            "waf:GetWebACL",
            "waf:ListWebACLs"
         ],
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":[
            "s3:ListAllMyBuckets"
         ],
         "Resource":"arn:aws:s3:::*"
      }
   ]
}
```

### Example 2: Allow Creation, Updating, and Deletion of Web Distributions<a name="access-policy-example-allow-create-delete-hosted-zones"></a>

The following permissions policy allows users to create, update, and delete web distributions by using the CloudFront console: 

```
{
   "Version": "2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "acm:ListCertificates", 
            "cloudfront:CreateDistribution",
            "cloudfront:DeleteDistribution",
            "cloudfront:GetDistribution",
            "cloudfront:GetDistributionConfig",
            "cloudfront:ListDistributions",
            "cloudfront:UpdateDistribution",
            "cloudfront:ListCloudFrontOriginAccessIdentities",
            "elasticloadbalancing:DescribeLoadBalancers",
            "iam:ListServerCertificates",
            "sns:ListSubscriptionsByTopic",
            "sns:ListTopics",
            "waf:GetWebACL",
            "waf:ListWebACLs"
         ],
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":[
            "s3:ListAllMyBuckets",
            "s3:PutBucketPolicy"
         ],
         "Resource":"arn:aws:s3:::*"
      }
   ]
}
```

The `cloudfront:ListCloudFrontOriginAccessIdentities` permission allows users to automatically grant to an existing origin access identity the permission to access objects in an Amazon S3 bucket\. If you also want users to be able to create origin access identities, you also need to allow the `cloudfront:CreateCloudFrontOriginAccessIdentity` permission\.

### Example 3: Allow Creation and Listing of Invalidations<a name="access-policy-example-allow-change-resource-record-sets"></a>

The following permissions policy allows users to create and list invalidations\. It includes read access to CloudFront distributions because you create and view invalidations by first displaying settings for a distribution:

```
{
   "Version": "2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "acm:ListCertificates", 
            "cloudfront:GetDistribution",
            "cloudfront:GetStreamingDistribution",
            "cloudfront:GetDistributionConfig",
            "cloudfront:ListDistributions",
            "cloudfront:ListCloudFrontOriginAccessIdentities",
            "cloudfront:CreateInvalidation",
            "cloudfront:GetInvalidation",
            "cloudfront:ListInvalidations",
            "elasticloadbalancing:DescribeLoadBalancers",
            "iam:ListServerCertificates",
            "sns:ListSubscriptionsByTopic",
            "sns:ListTopics",
            "waf:GetWebACL",
            "waf:ListWebACLs"
         ],
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":[
            "s3:ListAllMyBuckets"
         ],
         "Resource":"arn:aws:s3:::*"
      }
   ]
}
```