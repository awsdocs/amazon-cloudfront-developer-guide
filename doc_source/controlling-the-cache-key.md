# Controlling the cache key<a name="controlling-the-cache-key"></a>

With Amazon CloudFront, you can control the *cache key* for objects that are cached at CloudFront edge locations\. The cache key is the unique identifier for every object in the cache, and it determines whether a viewer request results in a *cache hit*\. A cache hit occurs when a viewer request generates the same cache key as a prior request, and the object for that cache key is in the edge location’s cache and valid\. When there’s a cache hit, the object is served to the viewer from a CloudFront edge location, which has the following benefits:
+ Reduced load on your origin server
+ Reduced latency for the viewer

You can get better performance from your website or application when you have a higher *cache hit ratio* \(a higher proportion of viewer requests result in a cache hit\)\. One way to improve your cache hit ratio is to include only the minimum necessary values in the cache key\. For more information, see [Understanding the cache key](understanding-the-cache-key.md)\.

To control the cache key, you use a CloudFront *cache policy*\. You attach a cache policy to one or more cache behaviors in a CloudFront distribution\.

**Contents**
+ [Understanding cache policies](#cache-key-understand-cache-policy)
  + [Policy information](#cache-key-understand-cache-policy-info)
  + [Time to live \(TTL\) settings](#cache-key-understand-cache-policy-ttl)
  + [Cache key settings](#cache-key-understand-cache-policy-settings)
+ [Creating cache policies](#cache-key-create-cache-policy)
+ [Using the managed cache policies](using-managed-cache-policies.md)
  + [Attaching a managed cache policy](using-managed-cache-policies.md#attaching-managed-cache-policies)
  + [Understanding the managed cache policies](using-managed-cache-policies.md#managed-cache-policies-list)
+ [Understanding the cache key](understanding-the-cache-key.md)
  + [The default cache key](understanding-the-cache-key.md#cache-key-default)
  + [Customizing the cache key](understanding-the-cache-key.md#cache-key-custom)

## Understanding cache policies<a name="cache-key-understand-cache-policy"></a>

You can use a cache policy to improve your cache hit ratio by controlling the values \(URL query strings, HTTP headers, and cookies\) that are included in the cache key\. CloudFront provides some predefined cache policies, known as *managed policies*, for common use cases\. You can use these managed policies, or you can create your own cache policy that’s specific to your needs\. For more information about the managed policies, see [Using the managed cache policies](using-managed-cache-policies.md)\.

A cache policy contains the following settings, which are categorized into *policy information*, *time to live \(TTL\) settings*, and *cache key settings*\.

### Policy information<a name="cache-key-understand-cache-policy-info"></a>

**Name**  
A name to identify the cache policy\. In the console, you use the name to attach the cache policy to a cache behavior\.

**Comment**  
A comment to describe the cache policy\. This is optional, but it can help you identify the purpose of the cache policy\.

### Time to live \(TTL\) settings<a name="cache-key-understand-cache-policy-ttl"></a>

The time to live \(TTL\) settings work together with the `Cache-Control` and `Expires` HTTP headers \(if they’re in the origin response\) to determine how long objects in the CloudFront cache remain valid\.

**Minimum TTL**  
The minimum amount of time, in seconds, that you want objects to stay in the CloudFront cache before CloudFront checks with the origin to see if the object has been updated\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

**Maximum TTL**  
The maximum amount of time, in seconds, that objects stay in the CloudFront cache before CloudFront checks with the origin to see if the object has been updated\. CloudFront uses this setting only when the origin sends `Cache-Control` or `Expires` headers with the object\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

**Default TTL**  
The default amount of time, in seconds, that you want objects to stay in the CloudFront cache before CloudFront checks with the origin to see if the object has been updated\. CloudFront uses this setting’s value as the object’s TTL only when the origin does *not* send `Cache-Control` or `Expires` headers with the object\. For more information, see [Managing How Long Content Stays in an Edge Cache \(Expiration\)](Expiration.md)\.

### Cache key settings<a name="cache-key-understand-cache-policy-settings"></a>

Cache key settings specify the values in viewer requests that CloudFront includes in the cache key\. The values can include URL query strings, HTTP headers, and cookies\. The values that you include in the cache key are automatically included in requests that CloudFront sends to the origin, known as *origin requests*\. For information about controlling origin requests without affecting the cache key, see [Controlling origin requests](controlling-origin-requests.md)\.

**Query strings**  
The URL query strings in viewer requests that CloudFront includes in the cache key and in origin requests\. For query strings, you can choose one of the following settings:  
+ **None** – The query strings in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\.
+ **All** – All query strings in viewer requests are included in the cache key and are also automatically included in origin requests\.
+ **Whitelist** – You specify which of the query strings in viewer requests are included in the cache key and automatically included in origin requests\.
+ **All\-Except** – You specify which of the query strings in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\. All other query strings, expect for the ones you specify, *are* included in the cache key and automatically included in origin requests\.
When you use the **Whitelist** or **All\-Except** setting, you specify query strings by their name, not their value\. For example, consider the following URL path:  

```
/content/stories/example-story.html?split-pages=false
```
In this case, you specify the query string as `split-pages`, not as `split-pages=false`\. However, CloudFront includes the full query string, including its value, in the cache key and in origin requests\.

**Headers**  
The HTTP headers in viewer requests that CloudFront includes in the cache key and in origin requests\. For headers, you can choose one of the following settings:  
+ **None** – The HTTP headers in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\.
+ **Whitelist** – You specify which of the HTTP headers in viewer requests are included in the cache key and automatically included in origin requests\.
When you use the **Whitelist** setting, you specify HTTP headers by their name, not their value\. For example, consider the following HTTP header:  

```
Accept-Language: en-US,en;q=0.5
```
In this case, you specify the header as `Accept-Language`, not as `Accept-Language: en-US,en;q=0.5`\. However, CloudFront includes the full header, including its value, in the cache key and in origin requests\.  
You can also include certain headers generated by CloudFront in the cache key\. For more information, see [Using the CloudFront HTTP headers](using-cloudfront-headers.md)\.

**Cookies**  
The cookies in viewer requests that CloudFront includes in the cache key and in origin requests\. For cookies, you can choose one of the following settings:  
+ **None** – The cookies in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\.
+ **All** – All cookies in viewer requests are included in the cache key and are automatically included in origin requests\.
+ **Whitelist** – You specify which of the cookies in viewer requests are included in the cache key and automatically included in origin requests\.
+ **All\-Except** – You specify which of the cookies in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\. All other cookies, expect for the ones you specify, *are* included in the cache key and automatically included in origin requests\.
When you use the **Whitelist** or **All\-Except** setting, you specify cookies by their name, not their value\. For example, consider the following `Cookie` header:  

```
Cookie: session_ID=abcd1234
```
In this case, you specify the cookie as `session_ID`, not as `session_ID=abcd1234`\. However, CloudFront includes the full cookie, including its value, in the cache key and in origin requests\.

**Cache compressed objects \(uses the `Accept-Encoding` header\)**  
This setting enables CloudFront to request and cache objects that are encoded in the `gzip` compression format, when the viewer supports it\. Viewers indicate their support for `gzip` encoding by using the `Accept-Encoding` HTTP header\.  
Enable this setting when either of the following are true:  
+ Your origin server returns `gzip` encoded objects in response to requests that contain the `Accept-Encoding: gzip` HTTP header\.
+ The cache behavior that this cache policy is attached to is configured with [CloudFront edge compression](ServingCompressedFiles.md)\.
If your origin server does not return `gzip` encoded objects, or the cache behavior is not configured with CloudFront edge compression, don’t enable this setting\. If you do, it might cause an increase in errors returned to the viewer\.  
The following explains how this setting affects a CloudFront distribution:    
**When this setting is enabled**  
When this setting is enabled, CloudFront’s behavior depends on whether the viewer request included the `Accept-Encoding` HTTP header\.  
+ When the viewer request does *not* include the `Accept-Encoding` header:
  + CloudFront does not include this header in the cache key\.
  + CloudFront does not include this header in the corresponding origin request\.
+ When the viewer request *does* include the `Accept-Encoding` header, CloudFront’s behavior depends on whether `gzip` is one of the header values\.
  + When `gzip` *is* one of the header values:
    + CloudFront includes `Accept-Encoding: gzip` in the cache key\.
    + CloudFront includes `Accept-Encoding: gzip` in the corresponding origin request\. Other values that were in the `Accept-Encoding` header sent by the viewer are not included\.
  + When `gzip` is *not* one of the header values:
    + CloudFront does not include this header in the cache key\.
    + CloudFront includes `Accept-Encoding: identity` in the corresponding origin request\. Other values that were in the `Accept-Encoding` header sent by the viewer are not included\.
When this setting is enabled, do not include the `Accept-Encoding` header in an [origin request policy](controlling-origin-requests.md) that’s associated with the same cache behavior\. CloudFront always includes this header in origin requests when this setting is enabled, so including `Accept-Encoding` in an origin request policy has no effect\.  
**When this setting is disabled**  
When this setting is disabled, CloudFront treats the `Accept-Encoding` header the same as any other HTTP header in the viewer request\. By default, it’s not included in the cache key and it’s not included in origin requests\. You can include it in the headers whitelist in a cache policy or an origin request policy like any other HTTP header\.

## Creating cache policies<a name="cache-key-create-cache-policy"></a>

You can use a cache policy to improve your cache hit ratio by controlling the values \(URL query strings, HTTP headers, and cookies\) that are included in the cache key\. You can create a cache policy in the CloudFront console, with the AWS Command Line Interface \(AWS CLI\), or with the CloudFront API\.

After you create a cache policy, you attach it to one or more cache behaviors in a CloudFront distribution\.

### Creating cache policies \(console\)<a name="cache-key-create-cache-policy-console"></a>

**To create a cache policy \(console\)**

1. Sign in to the AWS Management Console and open the **Policies** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v2/home?#/policies](https://console.aws.amazon.com/cloudfront/v2/home?#/policies)\.

1. Choose **Create cache policy**\.

1. Choose the desired setting for this cache policy\. For more information, see [Understanding cache policies](#cache-key-understand-cache-policy)\.

1. When finished, choose **Create cache policy**\.

After you create a cache policy, you can attach it to a cache behavior\.

**To attach a cache policy to an existing distribution \(console\)**

1. Open the **Distributions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/home#distributions:](https://console.aws.amazon.com/cloudfront/home#distributions:)\.

1. Choose the distribution to update, then choose the **Behaviors** tab\.

1. Choose the cache behavior to update, then choose **Edit**\.

   Or, to create a new cache behavior, choose **Create Behavior**\.

1. For **Cache and origin request settings**, make sure that **Use a cache policy and origin request policy** is chosen\.

1. For **Cache Policy**, choose the cache policy to attach to this cache behavior\.

1. At the bottom of the page, choose **Yes, Edit**\.

**To attach a cache policy to a new distribution \(console\)**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/home](https://console.aws.amazon.com/cloudfront/home)\.

1. Choose **Create Distribution**, then for **Web**, choose **Get Started**\.

1. For **Cache and origin request settings**, make sure that **Use a cache policy and origin request policy** is chosen\.

1. For **Cache Policy**, choose the cache policy to attach to this distribution’s default cache behavior\.

1. Choose the desired settings for the origin, default cache behavior, and distribution\. For more information, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

1. When finished, choose **Create Distribution**\.

### Creating cache policies \(AWS CLI\)<a name="cache-key-create-cache-policy-cli"></a>

To create a cache policy with the AWS Command Line Interface \(AWS CLI\), use the aws cloudfront create\-cache\-policy command\. You can use an input file to provide the command’s input parameters, rather than specifying each individual parameter as command line input\.

**To create a cache policy \(CLI with input file\)**

1. Use the following command to create a file named `cache-policy.yaml` that contains all of the input parameters for the create\-cache\-policy command\.

   ```
   aws cloudfront create-cache-policy --generate-cli-skeleton yaml-input > cache-policy.yaml
   ```
**Note**  
The `yaml-input` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate an input file in JSON format\. For more information, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `cache-policy.yaml` that you just created\. Edit the file to specify the cache policy settings that you want, then save the file\. You can remove optional fields from the file, but don’t remove the required fields\.

   For more information about the cache policy settings, see [Understanding cache policies](#cache-key-understand-cache-policy)\.

1. Use the following command to create the cache policy using input parameters from the `cache-policy.yaml` file\.

   ```
   aws cloudfront create-cache-policy --cli-input-yaml file://cache-policy.yaml
   ```

   Make note of the `Id` value in the command’s output\. This is the cache policy ID, and you need it to attach the cache policy to a CloudFront distribution’s cache behavior\.

**To attach a cache policy to an existing distribution \(CLI with input file\)**

1. Use the following command to save the distribution configuration for the CloudFront distribution that you want to update\. Replace *distribution\_ID* with the distribution’s ID\.

   ```
   aws cloudfront get-distribution-config --id distribution_ID --output yaml > dist-config.yaml
   ```
**Note**  
The `--output yaml` option is available only in [version 2 of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)\. With version 1 of the AWS CLI, you can generate the output in JSON format\. For more information, see [Controlling command output from the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output.html) in the *AWS Command Line Interface User Guide*\.

1. Open the file named `dist-config.yaml` that you just created\. Edit the file, making the following changes to each cache behavior that you are updating to use a cache policy\.
   + In the cache behavior, add a field named `CachePolicyId`\. For the field’s value, use the cache policy ID that you noted after creating the policy\.
   + Remove the `MinTTL`, `MaxTTL`, `DefaultTTL`, and `ForwardedValues` fields from the cache behavior\. These settings are specified in the cache policy, so you can’t include these fields and a cache policy in the same cache behavior\.
   + Rename the `ETag` field to `IfMatch`, but don’t change the field’s value\.

   Save the file when finished\.

1. Use the following command to update the distribution to use the cache policy\. Replace *distribution\_ID* with the distribution’s ID\.

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

1. Open the file named `distribution.yaml` that you just created\. In the default cache behavior, in the `CachePolicyId` field, enter the cache policy ID that you noted after creating the policy\. Continue editing the file to specify the distribution settings that you want, then save the file when finished\.

   For more information about the distribution settings, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

1. Use the following command to create the distribution using input parameters from the `distribution.yaml` file\.

   ```
   aws cloudfront create-distribution --cli-input-yaml file://distribution.yaml
   ```

### Creating cache policies \(API\)<a name="cache-key-create-cache-policy-api"></a>

To create a cache policy with the CloudFront API, use [CreateCachePolicy](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateCachePolicy.html)\. For more information about the fields that you specify in this API call, see [Understanding cache policies](#cache-key-understand-cache-policy) and the API reference documentation for your AWS SDK or other API client\.

After you create a cache policy, you can attach it to a cache behavior, using one of the following API calls:
+ To attach it to a cache behavior in an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a cache behavior in a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the cache policy’s ID in the `CachePolicyId` field, inside a cache behavior\. For more information about the other fields that you specify in these API calls, see [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.