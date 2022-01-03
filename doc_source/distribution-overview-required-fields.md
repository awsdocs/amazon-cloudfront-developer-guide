# Required fields for creating and updating distributions<a name="distribution-overview-required-fields"></a>

When you update a distribution by using the [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) CloudFront API action, there are more required fields than when you create a distribution by using [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\. Review the following tables for a summary of the fields that are required for creating and for updating a distribution\.

**DistributionConfig**


| Members | Required in CreateDistribution API call | Required in UpdateDistribution API call | 
| --- | --- | --- | 
| CallerReference | Y | Y | 
| Aliases | \- | Y | 
| DefaultRootObject | \- | Y | 
| Origins | Y | Y | 
| DefaultCacheBehavior | Y | Y | 
| CacheBehaviors | \- | Y | 
| CustomErrorResponses | \- | Y | 
| Comment | Y | Y | 
| Logging | \- | Y | 
| PriceClass | \- | Y | 
| Enabled | Y | Y | 
| ViewerCertificate | \- | Y | 
| Restrictions | \- | Y | 
| WebACLId | \- | Y | 
| HttpVersion | \- | Y | 
| IsIPV6Enabled | \- | \- | 

**CacheBehavior**


| Members | Required in CreateDistribution API call | Required in UpdateDistribution API call | 
| --- | --- | --- | 
| PathPattern | Y | Y | 
| TargetOriginId | Y | Y | 
| ForwardedValues | Y | Y | 
| TrustedSigners | Y | Y | 
| ViewerProtocolPolicy | Y | Y | 
| MinTTL | Y | Y | 
| AllowedMethods | \- | Y | 
| SmoothStreaming | \- | Y | 
| DefaultTTL | \- | Y | 
| MaxTTL | Y | Y | 
| Compress | \- | Y | 
| LambdaFunctionAssociations | \- | Y | 
| FieldLevelEncryptionId | \- | Y | 