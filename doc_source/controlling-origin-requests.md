# Controlling origin requests<a name="controlling-origin-requests"></a>

When a viewer request to CloudFront results in a *cache miss* \(the requested object is not cached at the edge location\), CloudFront sends a request to the origin to retrieve the object\. This is called an *origin request*\. The origin request always includes the following information from the viewer request:
+ The URL path \(the path only, without URL query strings or the domain name\)
+ The request body \(if there is one\)
+ The HTTP headers that CloudFront automatically includes in every origin request, including `Host`, `User-Agent`, and `X-Amz-Cf-Id`\.

Other information from the viewer request, such as URL query strings, HTTP headers, and cookies, is not included in the origin request by default\. But you might want to receive some of this other information at the origin, for example to collect data for analytics or telemetry\. You can use an *origin request policy* to control the information that’s included in an origin request\. 

Origin request policies are separate from [cache policies](controlling-the-cache-key.md), which control the cache key\. This separation enables you to receive additional information at the origin and also maintain a good *cache hit ratio* \(the proportion of viewer requests that result in a cache hit\)\. You do this by separately controlling which information is included in origin requests \(using the origin request policy\) and which is included in the cache key \(using the cache policy\)\.

Although the two kinds of policies are separate, they are related\. All URL query strings, HTTP headers, and cookies that you include in the cache key \(using a cache policy\) are automatically included in origin requests\. Use the origin request policy to specify the information that you want to include in origin requests, but *not* include in the cache key\. Just like a cache policy, you attach an origin request policy to one or more cache behaviors in a CloudFront distribution\.

You can also use an origin request policy to add additional HTTP headers to an origin request that were not included in the viewer request\. These additional headers are added by CloudFront before sending the origin request, with header values that are determined automatically based on the viewer request\. For more information, see [Using the CloudFront HTTP headers](using-cloudfront-headers.md)\.

**Contents**
+ [Understanding origin request policies](#origin-request-understand-origin-request-policy)
  + [Policy information](#origin-request-understand-origin-request-policy-info)
  + [Origin request settings](#origin-request-understand-origin-request-policy-settings)
+ [Creating origin request policies](#origin-request-create-origin-request-policy)
+ [Using the managed origin request policies](using-managed-origin-request-policies.md)
  + [Attaching a managed origin request policy](using-managed-origin-request-policies.md#attaching-managed-origin-request-policies)
  + [Understanding the managed origin request policies](using-managed-origin-request-policies.md#managed-origin-request-policies-list)

## Understanding origin request policies<a name="origin-request-understand-origin-request-policy"></a>

CloudFront provides some predefined origin request policies, known as *managed policies*, for common use cases\. You can use these managed policies, or you can create your own origin request policy that’s specific to your needs\. For more information about the managed policies, see [Using the managed origin request policies](using-managed-origin-request-policies.md)\.

An origin request policy contains the following settings, which are categorized into *policy information* and *origin request settings*\.

### Policy information<a name="origin-request-understand-origin-request-policy-info"></a>

**Name**  
A name to identify the origin request policy\. In the console, you use the name to attach the origin request policy to a cache behavior\.

**Comment**  
A comment to describe the origin request policy\. This is optional\.

### Origin request settings<a name="origin-request-understand-origin-request-policy-settings"></a>

Origin request settings specify the values in viewer requests that are included in requests that CloudFront sends to the origin \(known as origin requests\)\. The values can include URL query strings, HTTP headers, and cookies\. The values that you specify are included in origin requests, but are not included in the cache key\. For information about controlling the cache key, see [Controlling the cache key](controlling-the-cache-key.md)\.

**Query strings**  
The URL query strings in viewer requests that CloudFront includes in origin requests\. For query strings, you can choose one of the following settings:  
+ **None** – The query strings in viewer requests are *not* included in origin requests\.
+ **All** – All query strings in viewer requests are included in origin requests\.
+ **Whitelist** – You specify which of the query strings in viewer requests are included in origin requests\.
When you use the **Whitelist** setting, you specify query strings by their name, not their value\. For example, consider the following URL path:  

```
/content/stories/example-story.html?split-pages=false
```
In this case, you specify the query string as `split-pages`, not as `split-pages=false`\. However, CloudFront includes the full query string, including its value, in origin requests\.

**Headers**  
The HTTP headers in viewer requests that CloudFront includes in origin requests\. For headers, you can choose one of the following settings:  
+ **None** – The HTTP headers in viewer requests are *not* included in origin requests\.
+ **All viewer headers** – All HTTP headers in viewer requests are included in origin requests\.
+ **Whitelist** – You specify which HTTP headers are included in origin requests\.
+ **All viewer headers and whitelisted CloudFront\-\* headers** – All HTTP headers in viewer requests are included in origin requests\. Additionally, you specify which of the CloudFront headers you want to add to origin requests\. For more information about the CloudFront headers, see [Using the CloudFront HTTP headers](using-cloudfront-headers.md)\.
When you use the **Whitelist** or **All viewer headers and whitelisted CloudFront\-\* headers** setting, you specify HTTP headers by their name, not their value\. For example, consider the following HTTP header:  

```
Accept-Language: en-US,en;q=0.5
```
In this case, you specify the header as `Accept-Language`, not as `Accept-Language: en-US,en;q=0.5`\. However, CloudFront includes the full header, including its value, in origin requests\.

**Cookies**  
The cookies in viewer requests that CloudFront includes in origin requests\. For cookies, you can choose one of the following settings:  
+ **None** – The cookies in viewer requests are *not* included in origin requests\.
+ **All** – All cookies in viewer requests are included in origin requests\.
+ **Whitelist** – You specify which of the cookies in viewer requests are included in origin requests\.
When you use the **Whitelist** setting, you specify cookies by their name, not their value\. For example, consider the following `Cookie` header:  

```
Cookie: session_ID=abcd1234
```
In this case, you specify the cookie as `session_ID`, not as `session_ID=abcd1234`\. However, CloudFront includes the full cookie, including its value, in origin requests\.

## Creating origin request policies<a name="origin-request-create-origin-request-policy"></a>

You can use an origin request policy to control the values \(URL query strings, HTTP headers, and cookies\) that are included in requests that CloudFront sends to your origin\. You can create an origin request policy in the CloudFront console, with the AWS Command Line Interface \(AWS CLI\), or with the CloudFront API\.

After you create an origin request policy, you attach it to one or more cache behaviors in a CloudFront distribution\.

Origin request policies are not required\. When a cache behavior does not have an origin request policy attached, the origin request includes all the values that are specified in the [cache policy](controlling-the-cache-key.md#cache-key-understand-cache-policy), but nothing more\.

**Note**  
To use an origin request policy, the cache behavior must also use a [cache policy](controlling-the-cache-key.md)\. You cannot use an origin request policy in a cache behavior without a cache policy\.

### Creating origin request policies \(console\)<a name="origin-request-create-origin-request-policy-console"></a>

**To create an origin request policy \(console\)**

1. Sign in to the AWS Management Console and open the **Policies** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home?#/policies](https://console.aws.amazon.com/cloudfront/v2/home?#/policies)\.

1. Choose **Origin request policy**, then choose **Create origin request policy**\.

1. Choose the desired setting for this origin request policy\. For more information, see [Understanding origin request policies](#origin-request-understand-origin-request-policy)\.

1. When finished, choose **Create origin request policy**\.

After you create an origin request policy, you can attach it to a cache behavior\.

**To attach an origin request policy to an existing distribution \(console\)**

1. Open the **Distributions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/home#distributions:](https://console.aws.amazon.com/cloudfront/home#distributions:)\.

1. Choose the distribution to update, then choose the **Behaviors** tab\.

1. Choose the cache behavior to update, then choose **Edit**\.

   Or, to create a new cache behavior, choose **Create Behavior**\.

1. For **Cache and origin request settings**, make sure that **Use a cache policy and origin request policy** is chosen\.

1. For **Origin Request Policy**, choose the origin request policy to attach to this cache behavior\.

1. At the bottom of the page, choose **Yes, Edit**\.

**To attach an origin request policy to a new distribution \(console\)**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/home](https://console.aws.amazon.com/cloudfront/home)\.

1. Choose **Create Distribution**, then for **Web**, choose **Get Started**\.

1. For **Cache and origin request settings**, make sure that **Use a cache policy and origin request policy** is chosen\.

1. For **Origin Request Policy**, choose the origin request policy to attach to this distribution’s default cache behavior\.

1. Choose the desired settings for the origin, default cache behavior, and distribution\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

1. When finished, choose **Create Distribution**\.

### Creating origin request policies \(AWS CLI\)<a name="origin-request-create-origin-request-policy-cli"></a>

To create an origin request policy with the AWS Command Line Interface \(AWS CLI\), use the aws cloudfront create\-origin\-request\-policy command\. You can use an input file to provide the command’s input parameters, rather than specifying each individual parameter as command line input\.

**To create an origin request policy \(CLI with input file\)**

1. Use the following command to create a file named `origin-request-policy.yaml` that contains all of the input parameters for the create\-origin\-request\-policy command\.

   ```
   aws cloudfront create-origin-request-policy --generate-cli-skeleton yaml-input > origin-request-policy.yaml
   ```
**Note**  
The `yaml-input` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate an input file in JSON format\. For more information, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `origin-request-policy.yaml` that you just created\. Edit the file to specify the origin request policy settings that you want, then save the file\. You can remove optional fields from the file, but don’t remove the required fields\.

   For more information about the origin request policy settings, see [Understanding origin request policies](#origin-request-understand-origin-request-policy)\.

1. Use the following command to create the origin request policy using input parameters from the `origin-request-policy.yaml` file\.

   ```
   aws cloudfront create-origin-request-policy --cli-input-yaml file://origin-request-policy.yaml
   ```

   Make note of the `Id` value in the command’s output\. This is the origin request policy ID, and you need it to attach the origin request policy to a CloudFront distribution’s cache behavior\.

**To attach an origin request policy to an existing distribution \(CLI with input file\)**

1. Use the following command to save the distribution configuration for the CloudFront distribution that you want to update\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront get-distribution-config --id distribution_ID --output yaml > dist-config.yaml
   ```
**Note**  
The `--output yaml` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate the output in JSON format\. For more information, see [Controlling command output from the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `dist-config.yaml` that you just created\. Edit the file, making the following changes to each cache behavior that you are updating to use an origin request policy\.
   + In the cache behavior, add a field named `OriginRequestPolicyId`\. For the field’s value, use the origin request policy ID that you noted after creating the policy\.
   + Rename the `ETag` field to `IfMatch`, but don’t change the field’s value\.

   Save the file when finished\.

1. Use the following command to update the distribution to use the origin request policy\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront update-distribution --id distribution_ID --cli-input-yaml file://dist-config.yaml
   ```

**To attach a cache policy to a new distribution \(CLI with input file\)**

1. Use the following command to create a file named `distribution.yaml` that contains all of the input parameters for the create\-distribution command\.

   ```
   aws cloudfront create-distribution --generate-cli-skeleton yaml-input > distribution.yaml
   ```
**Note**  
The `yaml-input` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate an input file in JSON format\. For more information, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `distribution.yaml` that you just created\. In the default cache behavior, in the `OriginRequestPolicyId` field, enter the origin request policy ID that you noted after creating the policy\. Continue editing the file to specify the distribution settings that you want, then save the file when finished\.

   For more information about the distribution settings, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

1. Use the following command to create the distribution using input parameters from the `distribution.yaml` file\.

   ```
   aws cloudfront create-distribution --cli-input-yaml file://distribution.yaml
   ```

### Creating origin request policies \(API\)<a name="origin-request-create-origin-request-policy-api"></a>

To create an origin request policy with the CloudFront API, use [CreateOriginRequestPolicy](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateOriginRequestPolicy.html)\. For more information about the fields that you specify in this API call, see [Understanding origin request policies](#origin-request-understand-origin-request-policy) and the API reference documentation for your AWS SDK or other API client\.

After you create an origin request policy, you can attach it to a cache behavior, using one of the following API calls:
+ To attach it to a cache behavior in an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a cache behavior in a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the origin request policy’s ID in the `OriginRequestPolicyId` field, inside a cache behavior\. For more information about the other fields that you specify in these API calls, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.