# Required fields for creating and updating distributions<a name="distribution-overview-required-fields"></a>

When you update a distribution by using the [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) CloudFront API action, there are more required fields than when you create a distribution by using [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\. To update a distribution, complete the following steps:

1. Use [GetDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetDistribution.html) to get the current configuration of the distribution that you want to update\.

1. Modify the fields in the distribution configuration that you want to update\. Also, rename the `ETag` field to `IfMatch`, but don’t change the field’s value\.

1. Use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) to update the distribution, providing the entire distribution configuration, including the fields that you modified and those that you didn’t\.

The following tables summarizes the fields that are required for creating and for updating a distribution\.

**DistributionConfig**


| Members | Required in CreateDistribution API call | Required in UpdateDistribution API call | 
| --- | --- | --- | 
| CallerReference | Yes |  Yes  | 
| Aliases | \- | Yes \(this field is required, but a quantity of 0 with no items is valid\) | 
| DefaultRootObject | \- |  Yes \(this field is required, but an empty string is a valid value\)  | 
| Origins | Yes | Yes | 
|  OriginGroups  |  \-  |  \-  | 
| DefaultCacheBehavior | Yes | Yes | 
| CacheBehaviors | \- |  Yes \(this field is required, but a quantity of 0 with no items is valid\)  | 
| CustomErrorResponses | \- |  Yes \(this field is required, but a quantity of 0 with no items is valid\)  | 
| Comment | Yes \(this field is required, but an empty string is a valid value\) |  Yes \(this field is required, but an empty string is a valid value\)  | 
| Logging | \- | Yes | 
| PriceClass | \- | Yes | 
| Enabled | Yes | Yes | 
| ViewerCertificate | \- | Yes | 
| Restrictions | \- |  Yes \(this field is required, but a RestrictionsType of none and a quantity of 0 with no items is valid\)  | 
| WebACLId | \- |  Yes \(this field is required, but an empty string is a valid value\)  | 
| HttpVersion | \- | Yes | 
| IsIPV6Enabled | \- | \- | 

**CacheBehavior** \(including DefaultCacheBehavior\)


| Members | Required in CreateDistribution API call | Required in UpdateDistribution API call | 
| --- | --- | --- | 
| PathPattern \(this field does not apply to DefaultCacheBehavior\) | Yes | Yes | 
| TargetOriginId | Yes | Yes | 
| TrustedSigners | \- | \- | 
|  TrustedKeyGroups  |  \-  |  \-  | 
| ViewerProtocolPolicy | Yes | Yes | 
| AllowedMethods | \- | Yes | 
| SmoothStreaming | \- | Yes | 
| Compress | \- | Yes | 
| LambdaFunctionAssociations | \- |  Yes \(this field is required, but a quantity of 0 with no items is valid\)  | 
|  FunctionAssociations  |  \-  |  \-  | 
| FieldLevelEncryptionId | \- |  Yes \(this field is required, but an empty string is a valid value\) | 
|  RealtimeLogConfigArn  |  \-  |  \-  | 
|  CachePolicyId  |  Yes \(CachePolicyId is not required when you use the following deprecated fields, which is not recommended: ForwardedValues, MinTTL, DefaultTTL, and MaxTTL\)  |  Yes \(CachePolicyId is not required when you use the following deprecated fields, which is not recommended: ForwardedValues, MinTTL, DefaultTTL, and MaxTTL\)  | 
|  OriginRequestPolicyId  |  \-  |  \-  | 
|  ResponseHeadersPolicyId  |  \-  |  \-  | 