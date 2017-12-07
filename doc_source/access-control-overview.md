# Overview of Managing Access Permissions to Your CloudFront Resources<a name="access-control-overview"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\.

**Note**  
An *account administrator* \(or administrator user\) is a user that has administrator privileges\. For more information about administrators, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When you grant permissions, you decide who gets the permissions, the resources they get permissions for, and the actions that they get permission to perform\.


+ [ARNs for CloudFront Resources](#access-control-resources)
+ [Understanding Resource Ownership](#access-control-owner)
+ [Managing Access to Resources](#access-control-manage-access-intro)
+ [Specifying Policy Elements: Resources, Actions, Effects, and Principals](#access-control-specify-cf-actions)
+ [Specifying Conditions in a Policy](#specifying-conditions)

## ARNs for CloudFront Resources<a name="access-control-resources"></a>

All CloudFront resources—web and RTMP distributions, invalidations, and origin access identities—use the same format for Amazon Resource Names \(ARNs\):

`arn:aws:cloudfront::optional-account-id:*`

CloudFront provides API actions to work with each of these types of resources\. For more information, see the [Amazon CloudFront API Reference](http://docs.aws.amazon.com/cloudfront/latest/APIReference/)\. For a list of actions and the ARN that you specify to grant or deny permission to use each action, see [CloudFront API Permissions: Actions, Resources, and Conditions Reference](cf-api-permissions-ref.md)\.

## Understanding Resource Ownership<a name="access-control-owner"></a>

An AWS account owns the resources that are created in the account, regardless of who created the resources\. Specifically, the resource owner is the AWS account of the principal entity \(that is, the root account, an IAM user, or an IAM role\) that authenticates the resource creation request\. 

The following examples illustrate how this works:

+ If you use the root account credentials of your AWS account to create a web distribution, your AWS account is the owner of the distribution\.

+ If you create an IAM user in your AWS account and grant permissions to create a web distribution to that user, the user can create a web distribution\. The AWS account that created the user owns the distribution\.

+ If you create an IAM role in your AWS account with permissions to create a web distribution, anyone who can assume the role can create a web distribution\. Your AWS account, to which the role belongs, owns the distribution\.

## Managing Access to Resources<a name="access-control-manage-access-intro"></a>

A *permissions policy* specifies who has access to what\. This section explains the options for creating permissions policies for CloudFront\. For general information about IAM policy syntax and descriptions, see the [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as identity\-based policies \(IAM policies\), and policies attached to a resource are referred to as resource\-based policies\. CloudFront supports only identity\-based policies \(IAM policies\)\.


+ [Identity\-Based Policies \(IAM Policies\)](#access-control-manage-access-intro-iam-policies)
+ [Resource\-Based Policies](#access-control-manage-access-intro-resource-policies)

### Identity\-Based Policies \(IAM Policies\)<a name="access-control-manage-access-intro-iam-policies"></a>

You can attach policies to IAM identities\. For example, you can do the following:

+ **Attach a permissions policy to a user or a group in your account** – An account administrator can use a permissions policy that is associated with a particular user to grant permissions for that user to create a web distribution\. 

+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can grant permissions to perform CloudFront actions to a user that was created in another AWS account\. To do so, you attach a permissions policy to an IAM role, and then you allow the user in the other account to assume the role\. The following example explains how this works for two AWS accounts, account A and account B:

  1. Account A administrator creates an IAM role and attaches to the role a permissions policy that grants permissions to create or access resources that are owned by account A\.

  1. Account A administrator attaches a trust policy to the role\. The trust policy identifies account B as the principal that can assume the role\.

  1. Account B administrator can then delegate permissions to assume the role to users or groups in account B\. This allows users in account B to create or access resources in account A\.

  For more information about how to delegate permissions to users in another AWS account, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The following example policy allows a user to perform the `CreateDistribution` action to programmatically create a web distribution for your AWS account:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "cloudfront:CreateDistribution"
         ],
         "Resource":"*"
      }
   ]
}
```

For information about the permissions required to perform operations by using the CloudFront console, see [Permissions Required to Use the CloudFront Console](access-control-managing-permissions.md#console-required-permissions)\. For more information about attaching policies to identities for CloudFront, see [Using Identity\-Based Policies \(IAM Policies\) for CloudFront](access-control-managing-permissions.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\.

### Resource\-Based Policies<a name="access-control-manage-access-intro-resource-policies"></a>

Other services, such as Amazon S3, support attaching permissions policies to resources\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. CloudFront doesn't support attaching policies to resources\. 

## Specifying Policy Elements: Resources, Actions, Effects, and Principals<a name="access-control-specify-cf-actions"></a>

CloudFront includes API actions \(see [Amazon CloudFront API Reference](http://docs.aws.amazon.com/cloudfront/latest/APIReference/)\) that you can use on each CloudFront resource \(see [ARNs for CloudFront Resources](#access-control-resources)\)\. You can grant a user or a federated user permission to perform any or all of these actions\. 

The following are the basic policy elements:

+ **Resource** – You use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. For more information, see [ARNs for CloudFront Resources](#access-control-resources)\.

+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, depending on the specified `Effect`, the `cloudfront:CreateDistribution` permission allows or denies the user permissions to perform the CloudFront `CreateDistribution` action\.

+ **Effect** – You specify the effect, either allow or deny, when a user tries to perform the action on the specified resource\. If you don't explicitly grant access to an action, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.

+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. CloudFront doesn't support resource\-based policies\.

For more information about IAM policy syntax and descriptions, see the [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a showing all of the CloudFront API operations and the resources that they apply to, see [CloudFront API Permissions: Actions, Resources, and Conditions Reference](cf-api-permissions-ref.md)\.

## Specifying Conditions in a Policy<a name="specifying-conditions"></a>

When you grant permissions, you can use the IAM policy language to specify when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\. 

To express conditions, you use predefined condition keys\. There are no condition keys specific to CloudFront\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 