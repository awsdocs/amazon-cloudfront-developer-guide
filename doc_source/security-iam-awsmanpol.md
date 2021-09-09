# AWS managed policies for Amazon CloudFront<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it’s easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your users with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can’t change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new permissions become available\. Services do not remove permissions from an AWS managed policy, so policy updates won’t break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.







## AWS managed policy: CloudFrontReadOnlyAccess<a name="security-iam-awsmanpol-cloudfront-read-only"></a>

You can attach the **CloudFrontReadOnlyAccess** policy to your IAM identities\. This policy allows read\-only permissions to CloudFront resources\. It also allows read\-only permissions to other AWS service resources that are related to CloudFront and that are visible in the CloudFront console\.

**Permissions details**

This policy includes the following permissions\.
+ `cloudfront:DescribeFunction` – Allows principals to get information about metadata about CloudFront functions\.
+ `cloudfront:Get*` – Allows principals to get detailed information and configurations for CloudFront resources\.
+ `cloudfront:List*` – Allows principals to get lists of CloudFront resources\.
+ `acm:ListCertificates` – Allows principals to get a list of ACM certificates\.
+ `iam:ListServerCertificates` – Allows principals to get a list of server certificates stored in IAM\.
+ `route53:List*` – Allows principals to get lists of Route 53 resources\.
+ `waf:ListWebACLs` – Allows principals to get a list of web ACLs in AWS WAF\.
+ `waf:GetWebACL` – Allows principals to get detailed information about web ACLs in AWS WAF\.
+ `wafv2:ListWebACLs` – Allows principals to get a list of web ACLs in AWS WAF\.
+ `wafv2:GetWebACL` – Allows principals to get detailed information about web ACLs in AWS WAF\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "acm:ListCertificates",
                "cloudfront:DescribeFunction",
                "cloudfront:Get*",
                "cloudfront:List*",
                "iam:ListServerCertificates",
                "route53:List*",
                "waf:ListWebACLs",
                "waf:GetWebACL",
                "wafv2:ListWebACLs",
                "wafv2:GetWebACL"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: CloudFrontFullAccess<a name="security-iam-awsmanpol-cloudfront-full-access"></a>

You can attach the **CloudFrontFullAccess** policy to your IAM identities\. This policy allows administrative permissions to CloudFront resources\. It also allows read\-only permissions to other AWS service resources that are related to CloudFront and that are visible in the CloudFront console\.

**Permissions details**

This policy includes the following permissions\.
+ `cloudfront:*` – Allows principals to perform all actions on all CloudFront resources\.
+ `s3:ListAllMyBuckets` – Allows principals to get a list of all Amazon S3 buckets\.
+ `acm:ListCertificates` – Allows principals to get a list of ACM certificates\.
+ `iam:ListServerCertificates` – Allows principals to get a list of server certificates stored in IAM\.
+ `waf:ListWebACLs` – Allows principals to get a list of web ACLs in AWS WAF\.
+ `waf:GetWebACL` – Allows principals to get detailed information about web ACLs in AWS WAF\.
+ `wafv2:ListWebACLs` – Allows principals to get a list of web ACLs in AWS WAF\.
+ `wafv2:GetWebACL` – Allows principals to get detailed information about web ACLs in AWS WAF\.
+ `kinesis:ListStreams` – Allows principals to get a list of Amazon Kinesis streams\.
+ `kinesis:DescribeStream` – Allows principals to get detailed information about a Kinesis stream\.
+ `iam:ListRoles` – Allows principals to get a list of roles in IAM\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "s3:ListAllMyBuckets"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Action": [
                "acm:ListCertificates",
                "cloudfront:*",
                "iam:ListServerCertificates",
                "waf:ListWebACLs",
                "waf:GetWebACL",
                "wafv2:ListWebACLs",
                "wafv2:GetWebACL",
                "kinesis:ListStreams"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": [
                "kinesis:DescribeStream"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:kinesis:*:*:*"
        },
        {
            "Action": [
                "iam:ListRoles"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:iam::*:*"
        }
    ]
}
```

## AWS managed policy: AWSCloudFrontLogger<a name="security-iam-awsmanpol-cloudfront-logger"></a>

You can’t attach the **AWSCloudFrontLogger** policy to your IAM identities\. This policy is attached to a service\-linked role that allows CloudFront to perform actions on your behalf\. For more information, see [Service\-linked roles for Lambda@Edge](lambda-edge-permissions.md#using-service-linked-roles)\.

This policy allows CloudFront to push log files to Amazon CloudWatch\. For details about the permissions included in this policy, see [Service\-linked role permissions for CloudFront logger](lambda-edge-permissions.md#slr-permissions-cloudfront-logger)\.

## AWS managed policy: AWSLambdaReplicator<a name="security-iam-awsmanpol-lambda-replicator"></a>

You can’t attach the **AWSLambdaReplicator** policy to your IAM identities\. This policy is attached to a service\-linked role that allows CloudFront to perform actions on your behalf\. For more information, see [Service\-linked roles for Lambda@Edge](lambda-edge-permissions.md#using-service-linked-roles)\.

This policy allows CloudFront to create, delete, and disable functions in AWS Lambda to replicate Lambda@Edge functions to AWS Regions\. For details about the permissions included in this policy, see [Service\-linked role permissions for Lambda replicator](lambda-edge-permissions.md#slr-permissions-lambda-replicator)\.

## CloudFront updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for CloudFront since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the CloudFront [Document history](WhatsNew.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [CloudFrontReadOnlyAccess](#security-iam-awsmanpol-cloudfront-read-only) – Update to an existing policy  |  CloudFront added a new permission to describe CloudFront functions\. This permission allows the user, group, or role to read information and metadata about a CloudFront function, but not the function’s code\.  | September 8, 2021 | 
|  CloudFront started tracking changes  |  CloudFront started tracking changes for its AWS managed policies\.  | September 8, 2021 | 