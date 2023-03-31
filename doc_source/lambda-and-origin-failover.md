# Using Lambda@Edge functions with origin failover<a name="lambda-and-origin-failover"></a>

You can use Lambda@Edge functions with CloudFront distributions that you've set up with origin groups, for example, for origin failover that you configure to help ensure high availability\. To use a Lambda function with an origin group, specify the function in an origin request or origin response trigger for an origin group when you create the cache behavior\.

For more information, see the following:
+ **Creating origin groups:** [Creating an origin group](high_availability_origin_failover.md#concept_origin_groups.creating)
+ **How origin failover works with Lambda@Edge:** [Use origin failover with Lambda@Edge functions](high_availability_origin_failover.md#concept_origin_groups.lambda)