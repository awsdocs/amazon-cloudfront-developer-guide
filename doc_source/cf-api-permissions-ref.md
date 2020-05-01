# CloudFront API Permissions: Actions, Resources, and Conditions Reference<a name="cf-api-permissions-ref"></a>

When you are setting up [Access Control](auth-and-access-control.md#access-control) and writing a permissions policy that you can attach to an IAM identity \(identity\-based policies\), you can use the following as a reference\. The each CloudFront API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\. You specify the actions in the policy's `Action` field, and you specify the resource value in the policy's `Resource` field\. 

You can use AWS\-wide condition keys in your CloudFront policies to express conditions\. For a complete list of AWS\-wide keys, see [Available Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 

**Topics**
+ [Required Permissions for Actions on Web Distributions](#required-permissions-web-distributions)
+ [Required Permissions for Actions on RTMP Distributions](#required-permissions-rtmp-distributions)
+ [Required Permissions for Actions on Invalidations](#required-permissions-invalidations)
+ [Required Permissions for Actions on Origin Access Identities](#required-permissions-origin-access-identities)
+ [Required Permissions for CloudFront Actions Related to Lambda@Edge](#required-permissions-lambda)
+ [Required Permissions for Actions on Tags](#required-permissions-tags)

## Required Permissions for Actions on Web Distributions<a name="required-permissions-web-distributions"></a><a name="web-distributions-table"></a>

[CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)  
**Required Permissions \(API Action\):**  
+ `cloudfront:CreateDistribution`
+ `acm:ListCertificates` \(CloudFront console only\)
+ Only if you configure CloudFront to save access logs:
  + `s3:GetBucketAcl`
  + `s3:PutBucketAcl`
  + The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:**  
+ CloudFront: \*
+ ACM: \*
+ Amazon S3: If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[CreateDistributionWithTags](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistributionWithTags.html)  
**Required Permissions \(API Action\):**  
+ `cloudfront:CreateDistribution`, `cloudfront:TagResource`
+ `acm:ListCertificates` \(CloudFront console only\)
+ Only if you configure CloudFront to save access logs:
  + `s3:GetBucketAcl`
  + `s3:PutBucketAcl`
  + The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:**  
+ CloudFront: \*
+ ACM: \*
+ Amazon S3: If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[GetDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetDistribution.html)  
**Required Permissions \(API Action\):** `cloudfront:GetDistribution`, `acm:ListCertificates` \(CloudFront console only\)  
**Resources:** `*`

[GetDistributionConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetDistributionConfig.html)  
**Required Permissions \(API Action\):** `cloudfront:GetDistributionConfig`, `acm:ListCertificates` \(CloudFront console only\)  
**Resources:** `*`

[ListDistributions](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListDistributions.html)  
**Required Permissions \(API Action\):** `cloudfront:ListDistributions`  
**Resources:** `*`

[UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)  
**Required Permissions \(API Action\):**  
+ `cloudfront:UpdateDistribution`
+ `acm:ListCertificates` \(CloudFront console only\)
+ Only if you configure CloudFront to save access logs:
  + `s3:GetBucketAcl`
  + `s3:PutBucketAcl`
  + The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:**  
+ CloudFront: \*
+ ACM: \*
+ Amazon S3: If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[DeleteDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteDistribution.html)  
**Required Permissions \(API Action\):** `cloudfront:DeleteDistribution`  
**Resources:** `*`

## Required Permissions for Actions on RTMP Distributions<a name="required-permissions-rtmp-distributions"></a><a name="rtmp-distributions-table"></a>

[CreateStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateStreamingDistribution.html)  
**Required Permissions \(API Action\):** `cloudfront:CreateStreamingDistribution`  
Only if you configure CloudFront to save access logs:  
+ `s3:GetBucketAcl`
+ `s3:PutBucketAcl`
+ The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:** `*`  
If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[CreateStreamingDistributionWithTags](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateStreamingDistributionWithTags.html)  
**Required Permissions \(API Action\):** `cloudfront:CreateStreamingDistribution`, `cloudfront:TagResource`  
Only if you configure CloudFront to save access logs:  
+ `s3:GetBucketAcl`
+ `s3:PutBucketAcl`
+ The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:** `*`  
If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[GetStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetStreamingDistribution.html)  
**Required Permissions \(API Action\):** `cloudfront:GetStreamingDistribution`  
**Resources:** `*`

[GetStreamingDistributionConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetStreamingDistributionConfig.html)  
**Required Permissions \(API Action\):** `cloudfront:GetStreamingDistributionConfig`  
**Resources:** `*`

[ListStreamingDistributions](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListStreamingDistributions.html)  
**Required Permissions \(API Action\):** `cloudfront:ListStreamingDistributions`  
**Resources:** `*`

[UpdateStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateStreamingDistribution.html)  
**Required Permissions \(API Action\):** `cloudfront:UpdateStreamingDistribution`  
Only if you configure CloudFront to save access logs:  
+ `s3:GetBucketAcl`
+ `s3:PutBucketAcl`
+ The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:** `*`  
If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[DeleteStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteStreamingDistribution.html)  
**Required Permissions \(API Action\):** `cloudfront:DeleteDistribution`  
**Resources:** `*`

## Required Permissions for Actions on Invalidations<a name="required-permissions-invalidations"></a><a name="invalidations-table"></a>

[CreateInvalidation](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateInvalidation.html)  
**Required Permissions \(API Action\):** `cloudfront:CreateInvalidation`  
**Resources:** `*`

[GetInvalidation](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetInvalidation.html)  
**Required Permissions \(API Action\):** `cloudfront:GetInvalidation`  
**Resources:** `*`

[ListInvalidations](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListInvalidations.html)  
**Required Permissions \(API Action\):** `cloudfront:ListInvalidations`  
**Resources:** `*`

## Required Permissions for Actions on Origin Access Identities<a name="required-permissions-origin-access-identities"></a><a name="origin-access-identities-table"></a>

[CreateCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateCloudFrontOriginAccessIdentity.html)  
**Required Permissions \(API Action\):** `cloudfront:CreateCloudFrontOriginAccessIdentity`  
**Resources:** `*`

[GetCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetCloudFrontOriginAccessIdentity.html)  
**Required Permissions \(API Action\):** `cloudfront:GetCloudFrontOriginAccessIdentity`  
**Resources:** `*`

[GetCloudFrontOriginAccessIdentityConfig](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetCloudFrontOriginAccessIdentityConfig.html)  
**Required Permissions \(API Action\):** `cloudfront:GetCloudFrontOriginAccessIdentityConfig`  
**Resources:** `*`

[ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html)  
**Required Permissions \(API Action\):** `cloudfront:ListCloudFrontOriginAccessIdentities`  
**Resources:** `*`

[UpdateCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateCloudFrontOriginAccessIdentity.html)  
**Required Permissions \(API Action\):** `cloudfront:UpdateCloudFrontOriginAccessIdentity`  
**Resources:** `*`

[DeleteCloudFrontOriginAccessIdentity](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteCloudFrontOriginAccessIdentity.html)  
**Required Permissions \(API Action\):** `cloudfront:DeleteCloudFrontOriginAccessIdentity`  
**Resources:** `*`

## Required Permissions for CloudFront Actions Related to Lambda@Edge<a name="required-permissions-lambda"></a>

To use Lambda@Edge, you need the following CloudFront permissions so you can create or update a distribution that includes triggers for Lambda functions\. For information about the Lambda permissions that you need, see [Setting IAM Permissions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge-permissions.html) in the "AWS Lambda@Edge" chapter in the *AWS Lambda Developer Guide*\.<a name="lambda-table"></a>

[CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)  
**Required Permissions \(API Action\):**  
+ `cloudfront:CreateDistribution`
+ `acm:ListCertificates` \(CloudFront console only\)
+ Only if you configure CloudFront to save access logs:
  + `s3:GetBucketAcl`
  + `s3:PutBucketAcl`
  + The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:**  
+ CloudFront: \*
+ ACM: \*
+ Amazon S3: If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[CreateDistributionWithTags](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistributionWithTags.html)  
**Required Permissions \(API Action\):**  
+ `cloudfront:CreateDistribution`, `cloudfront:TagResource`
+ `acm:ListCertificates` \(CloudFront console only\)
+ Only if you configure CloudFront to save access logs:
  + `s3:GetBucketAcl`
  + `s3:PutBucketAcl`
  + The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:**  
+ CloudFront: \*
+ ACM: \*
+ Amazon S3: If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

[UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)  
**Required Permissions \(API Action\):**  
+ `cloudfront:UpdateDistribution`
+ `acm:ListCertificates` \(CloudFront console only\)
+ Only if you configure CloudFront to save access logs:
  + `s3:GetBucketAcl`
  + `s3:PutBucketAcl`
  + The S3 ACL for the bucket must grant you `FULL_CONTROL`
**Resources:**  
+ CloudFront: \*
+ ACM: \*
+ Amazon S3: If you configure CloudFront to save access logs, you can optionally restrict access to a specified bucket\.

## Required Permissions for Actions on Tags<a name="required-permissions-tags"></a><a name="invalidations-tags"></a>

[TagResource](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_TagResource.html)  
**Required Permissions \(API Action\):** `cloudfront:TagResource`  
**Resources:** `*`

[UntagResource](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UntagResource.html)  
**Required Permissions \(API Action\):** `cloudfront:UntagResource`  
**Resources:** `*`

[ListTagsForResource](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListTagsForResource.html)  
**Required Permissions \(API Action\):** `cloudfront:ListTagsForResource`  
**Resources:** `*`