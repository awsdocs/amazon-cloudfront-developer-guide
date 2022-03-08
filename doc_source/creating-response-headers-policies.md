# Creating response headers policies<a name="creating-response-headers-policies"></a>

You can use a response headers policy to specify the HTTP headers that Amazon CloudFront adds to HTTP responses\. For more information about response headers policies and why to use them, see [Adding HTTP headers to CloudFront responses](adding-response-headers.md)\.

You can create a response headers policy in the CloudFront console, with AWS CloudFormation, with the AWS Command Line Interface \(AWS CLI\), or with the CloudFront API\. After you create a response headers policy, you attach it to one or more cache behaviors in a CloudFront distribution\.

Before you create a custom response headers policy, you should see if one of the [managed response headers policies](using-managed-response-headers-policies.md) fits your use case\. If so, you can attach the managed policy to your cache behavior without needing to create a custom policy\.

------
#### [ Console ]

**To create a response headers policy \(console\)**

1. Sign in to the AWS Management Console, then go to the **Response headers** tab on the **Policies** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders](https://console.aws.amazon.com/cloudfront/v3/home#/policies/responseHeaders)\.

1. Choose **Create response headers policy**\.

1. In the **Create response headers policy** form, do the following:

   1. In the **Details** panel, enter a **Name** for the response headers policy and \(optionally\) a **Description** that explains what the policy is for\.

   1. In the **Cross\-origin resource sharing \(CORS\)** panel, choose the **Configure CORS** toggle and configure any CORS headers that you want to add to the policy\. If you want the configured headers to override the headers that CloudFront receives from the origin, select the **Origin override** check box\.

      For more information about the CORS headers settings, see [CORS headers](understanding-response-headers-policies.md#understanding-response-headers-policies-cors)\.

   1. In the **Security headers** panel, choose the toggle and configure each of the security headers that you want to add to the policy\.

      For more information about the security headers settings, see [Security headers](understanding-response-headers-policies.md#understanding-response-headers-policies-security)\.

   1. In the **Custom headers** panel, add any custom headers that you want to include in the policy\.

      For more information about the custom headers settings, see [Custom headers](understanding-response-headers-policies.md#understanding-response-headers-policies-custom)\.

1. Choose **Create** to create the policy\.

After you create a response headers policy, you can attach it to a cache behavior in a CloudFront distribution\.

**To attach a response headers policy to an existing distribution \(console\)**

1. Open the **Distributions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home#/distributions](https://console.aws.amazon.com/cloudfront/v3/home#/distributions)\.

1. Choose the distribution to update, then choose the **Behaviors** tab\.

1. Select the cache behavior to update, then choose **Edit**\.

   Or, to create a new cache behavior, choose **Create behavior**\.

1. For **Response headers policy**, choose the policy to add to the cache behavior\.

1. Choose **Save changes** to update the cache behavior\. \(If you’re creating a new cache behavior, choose **Create behavior**\)\.

**To attach a response headers policy to a new distribution \(console\)**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose **Create distribution**\.

1. For **Response headers policy**, choose the policy to add to the cache behavior\.

1. Choose the other settings for your distribution\. For more information, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\.

1. Choose **Create distribution** to create the distribution\.

------
#### [ AWS CloudFormation ]

To create a response headers policy with AWS CloudFormation, use the `AWS::CloudFront::ResponseHeadersPolicy` resource type\. The following example shows the AWS CloudFormation template syntax, in YAML format, for creating a response headers policy\.

```
Type: AWS::CloudFront::ResponseHeadersPolicy
Properties: 
  ResponseHeadersPolicyConfig: 
    Name: EXAMPLE-Response-Headers-Policy
    Comment: Example response headers policy for the documentation
    CorsConfig: 
      AccessControlAllowCredentials: false
      AccessControlAllowHeaders: 
        Items: 
          - '*'
      AccessControlAllowMethods: 
        Items: 
          - GET
          - OPTIONS
      AccessControlAllowOrigins: 
        Items: 
          - https://example.com
          - https://docs.example.com
      AccessControlExposeHeaders: 
        Items: 
          - '*'
      AccessControlMaxAgeSec: 600
      OriginOverride: false
    CustomHeadersConfig: 
      Items: 
        - Header: Example-Custom-Header-1
          Value: value-1
          Override: true
        - Header: Example-Custom-Header-2
          Value: value-2
          Override: true
    SecurityHeadersConfig: 
      ContentSecurityPolicy: 
        ContentSecurityPolicy: default-src 'none'; img-src 'self'; script-src 'self'; style-src 'self'; object-src 'none'; frame-ancestors 'none'
        Override: false
      ContentTypeOptions: # You don't need to specify a value for 'X-Content-Type-Options'.
                          # Simply including it in the template sets its value to 'nosniff'.
        Override: false
      FrameOptions: 
        FrameOption: DENY
        Override: false
      ReferrerPolicy: 
        ReferrerPolicy: same-origin
        Override: false
      StrictTransportSecurity: 
        AccessControlMaxAgeSec: 63072000
        IncludeSubdomains: true
        Preload: true
        Override: false
      XSSProtection: 
        ModeBlock: true # You can set ModeBlock to 'true' OR set a value for ReportUri, but not both
        Protection: true
        Override: false
```

For more information, see [AWS::CloudFront::ResponseHeadersPolicy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudfront-responseheaderspolicy.html) in the *AWS CloudFormation User Guide*\.

------
#### [ CLI ]

To create a response headers policy with the AWS Command Line Interface \(AWS CLI\), use the aws cloudfront create\-response\-headers\-policy command\. You can use an input file to provide the command’s input parameters, rather than specifying each individual parameter as command line input\.

**To create a response headers policy \(CLI with input file\)**

1. Use the following command to create a file named `response-headers-policy.yaml` that contains all of the input parameters for the create\-response\-headers\-policy command\.

   ```
   aws cloudfront create-response-headers-policy --generate-cli-skeleton yaml-input > response-headers-policy.yaml
   ```
**Note**  
The `yaml-input` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate an input file in JSON format\. For more information, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `response-headers-policy.yaml` that you just created\. Edit the file to specify a policy name and the response headers to include in the policy, then save the file\.

   For more information about the response headers policy settings, see [Understanding response headers policies](understanding-response-headers-policies.md)\.

1. Use the following command to create the response headers policy using input parameters from the `response-headers-policy.yaml` file\.

   ```
   aws cloudfront create-response-headers-policy --cli-input-yaml file://response-headers-policy.yaml
   ```

   Make note of the `Id` value in the command’s output\. This is the response headers policy ID, and you need it to attach the policy to a CloudFront distribution’s cache behavior\.

**To attach a response headers policy to an existing distribution \(CLI with input file\)**

1. Use the following command to save the distribution configuration for the CloudFront distribution that you want to update\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront get-distribution-config --id distribution_ID --output yaml > dist-config.yaml
   ```
**Note**  
The `--output yaml` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate the output in JSON format\. For more information, see [Controlling command output from the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `dist-config.yaml` that you just created\. Edit the file, making the following changes to the cache behavior that you are updating to use the response headers policy\.
   + In the cache behavior, add a field named `ResponseHeadersPolicyId`\. For the field’s value, use the response headers policy ID that you noted after creating the policy\.
   + Rename the `ETag` field to `IfMatch`, but don’t change the field’s value\.

   Save the file when finished\.

1. Use the following command to update the distribution to use the response headers policy\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront update-distribution --id distribution_ID --cli-input-yaml file://dist-config.yaml
   ```

**To attach a response headers policy to a new distribution \(CLI with input file\)**

1. Use the following command to create a file named `distribution.yaml` that contains all of the input parameters for the create\-distribution command\.

   ```
   aws cloudfront create-distribution --generate-cli-skeleton yaml-input > distribution.yaml
   ```
**Note**  
The `yaml-input` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate an input file in JSON format\. For more information, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `distribution.yaml` that you just created\. In the default cache behavior, in the `ResponseHeadersPolicyId` field, enter the response headers policy ID that you noted after creating the policy\. Continue editing the file to specify the distribution settings that you want, then save the file when finished\.

   For more information about the distribution settings, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\.

1. Use the following command to create the distribution using input parameters from the `distribution.yaml` file\.

   ```
   aws cloudfront create-distribution --cli-input-yaml file://distribution.yaml
   ```

------
#### [ API ]

To create a response headers policy with the CloudFront API, use [CreateResponseHeadersPolicy](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateResponseHeadersPolicy.html)\. For more information about the fields that you specify in this API call, see [Understanding response headers policies](understanding-response-headers-policies.md) and the API reference documentation for your AWS SDK or other API client\.

After you create a response headers policy, you can attach it to a cache behavior, using one of the following API calls:
+ To attach it to a cache behavior in an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a cache behavior in a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the request headers policy’s ID in the `RequestHeadersPolicyId` field, inside a cache behavior\. For more information about the other fields that you specify in these API calls, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.

------