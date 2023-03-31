# Using the managed origin request policies<a name="using-managed-origin-request-policies"></a>

CloudFront provides a set of managed origin request policies that you can attach to any of your distribution's cache behaviors\. With a managed origin request policy, you don't need to write or maintain your own origin request policy\. The managed policies use settings that are optimized for specific use cases\.

**Topics**
+ [Attaching a managed origin request policy](#attaching-managed-origin-request-policies)
+ [Available managed origin request policies](#managed-origin-request-policies-list)

## Attaching a managed origin request policy<a name="attaching-managed-origin-request-policies"></a>

To use a managed origin request policy, you attach it to a cache behavior in your distribution\. The process is the same as when you create an origin request policy, but instead of creating a new one, you just attach one of the managed origin request policies\. You attach the policy either by name \(with the console\) or by ID \(with the AWS CLI or SDKs\)\. The names and IDs are listed in the following section\.

For more information, see [Creating origin request policies](controlling-origin-requests.md#origin-request-create-origin-request-policy)\.

## Available managed origin request policies<a name="managed-origin-request-policies-list"></a>

The following topics describe the managed origin request policies that you can use\.

**Topics**
+ [AllViewer](#managed-origin-request-policy-all-viewer)
+ [AllViewerAndCloudFrontHeaders\-2022\-06](#managed-origin-request-policy-all-viewer-and-cloudfront)
+ [AllViewerExceptHostHeader](#managed-origin-request-policy-all-viewer-except-host-header)
+ [CORS\-CustomOrigin](#managed-origin-request-policy-cors-custom)
+ [CORS\-S3Origin](#managed-origin-request-policy-cors-s3)
+ [Elemental\-MediaTailor\-PersonalizedManifests](#managed-origin-request-policy-mediatailor)
+ [UserAgentRefererHeaders](#managed-origin-request-policy-user-agent-referer)

### AllViewer<a name="managed-origin-request-policy-all-viewer"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/216adef6-5c7f-47e4-b989-5492eafa07d3)

This policy includes all values \(headers, cookies, and query strings\) from the viewer request\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`216adef6-5c7f-47e4-b989-5492eafa07d3`

This policy has the following settings:
+ **Headers included in origin requests:** All headers in the viewer request
+ **Cookies included in origin requests:** All
+ **Query strings included in origin requests:** All

### AllViewerAndCloudFrontHeaders\-2022\-06<a name="managed-origin-request-policy-all-viewer-and-cloudfront"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/33f36d7e-f396-46d9-90e0-52428a34d9dc)

This policy includes all values \(headers, cookies, and query strings\) from the viewer request, and all [CloudFront headers](adding-cloudfront-headers.md) that were released through June 2022 \(CloudFront headers released after June 2022 are not included\)\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`33f36d7e-f396-46d9-90e0-52428a34d9dc`

This policy has the following settings:
+ **Headers included in origin requests:** All headers in the viewer request, and the following CloudFront headers:
  + `CloudFront-Forwarded-Proto`
  + `CloudFront-Is-Android-Viewer`
  + `CloudFront-Is-Desktop-Viewer`
  + `CloudFront-Is-IOS-Viewer`
  + `CloudFront-Is-Mobile-Viewer`
  + `CloudFront-Is-SmartTV-Viewer`
  + `CloudFront-Is-Tablet-Viewer`
  + `CloudFront-Viewer-Address`
  + `CloudFront-Viewer-ASN`
  + `CloudFront-Viewer-City`
  + `CloudFront-Viewer-Country`
  + `CloudFront-Viewer-Country-Name`
  + `CloudFront-Viewer-Country-Region`
  + `CloudFront-Viewer-Country-Region-Name`
  + `CloudFront-Viewer-Http-Version`
  + `CloudFront-Viewer-Latitude`
  + `CloudFront-Viewer-Longitude`
  + `CloudFront-Viewer-Metro-Code`
  + `CloudFront-Viewer-Postal-Code`
  + `CloudFront-Viewer-Time-Zone`
  + `CloudFront-Viewer-TLS`
+ **Cookies included in origin requests:** All
+ **Query strings included in origin requests:** All

### AllViewerExceptHostHeader<a name="managed-origin-request-policy-all-viewer-except-host-header"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/b689b0a8-53d0-40ab-baf2-68738e2966ac)

This policy does ***not*** include the `Host` header from the viewer request, but does include all others values \(headers, cookies, and query strings\) from the viewer request\.

This policy is intended for use with Amazon API Gateway and AWS Lambda function URL origins\. These origins expect the `Host` header to contain the origin domain name, not the domain name of the CloudFront distribution\. Forwarding the `Host` header from the viewer request to these origins can prevent them from working\.

**Note**  
When you use this managed origin request policy to remove the viewer's `Host` header, CloudFront adds a new `Host` header with the origin's domain name to the origin request\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`b689b0a8-53d0-40ab-baf2-68738e2966ac`

This policy has the following settings:
+ **Headers included in origin requests:** All headers in the viewer request ***except*** for the `Host` header
+ **Cookies included in origin requests:** All
+ **Query strings included in origin requests:** All

### CORS\-CustomOrigin<a name="managed-origin-request-policy-cors-custom"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/59781a5b-3903-41f3-afcb-af62929ccde1)

This policy includes the header that enables cross\-origin resource sharing \(CORS\) requests when the origin is a custom origin\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`59781a5b-3903-41f3-afcb-af62929ccde1`

This policy has the following settings:
+ **Headers included in origin requests:**
  + `Origin`
+ **Cookies included in origin requests:** None
+ **Query strings included in origin requests:** None

### CORS\-S3Origin<a name="managed-origin-request-policy-cors-s3"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/88a5eaf4-2fd4-4709-b370-b4c650ea3fcf)

This policy includes the headers that enable cross\-origin resource sharing \(CORS\) requests when the origin is an Amazon S3 bucket\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`88a5eaf4-2fd4-4709-b370-b4c650ea3fcf`

This policy has the following settings:
+ **Headers included in origin requests:**
  + `Origin`
  + `Access-Control-Request-Headers`
  + `Access-Control-Request-Method`
+ **Cookies included in origin requests:** None
+ **Query strings included in origin requests:** None

### Elemental\-MediaTailor\-PersonalizedManifests<a name="managed-origin-request-policy-mediatailor"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/775133bc-15f2-49f9-abea-afb2e0bf67d2)

This policy is intended for use with an origin that is an AWS Elemental MediaTailor endpoint\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`775133bc-15f2-49f9-abea-afb2e0bf67d2`

This policy has the following settings:
+ **Headers included in origin requests:**
  + `Origin`
  + `Access-Control-Request-Headers`
  + `Access-Control-Request-Method`
  + `User-Agent`
  + `X-Forwarded-For`
+ **Cookies included in origin requests:** None
+ **Query strings included in origin requests:** All

### UserAgentRefererHeaders<a name="managed-origin-request-policy-user-agent-referer"></a>

[View this policy in the CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home#/policies/origin/acba4595-bd28-49b8-b9fe-13317c0390fa)

This policy includes only the `User-Agent` and `Referer` headers\. It doesn't include any query strings or cookies\.

When using AWS CloudFormation, the AWS CLI, or the CloudFront API, the ID for this policy is:

`acba4595-bd28-49b8-b9fe-13317c0390fa`

This policy has the following settings:
+ **Headers included in origin requests:**
  + `User-Agent`
  + `Referer`
+ **Cookies included in origin requests:** None
+ **Query strings included in origin requests:** None