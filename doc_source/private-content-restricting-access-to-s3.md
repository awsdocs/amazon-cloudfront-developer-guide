# Restricting access to an Amazon S3 origin<a name="private-content-restricting-access-to-s3"></a>

CloudFront provides two ways to send authenticated requests to an Amazon S3 origin: *origin access control* \(OAC\) and *origin access identity* \(OAI\)\. We recommend using OAC because it supports:
+ All Amazon S3 buckets in all AWS Regions, including opt\-in Regions launched after December 2022
+ Amazon S3 [server\-side encryption with AWS KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html) \(SSE\-KMS\)
+ Dynamic requests \(`PUT` and `DELETE`\) to Amazon S3

OAI doesn't work for the scenarios in the preceding list, or it requires extra workarounds in those scenarios\. The following topics describe how to use OAC with an Amazon S3 origin\. For information about how to migrate from OAI to OAC, see [Migrating from origin access identity \(OAI\) to origin access control \(OAC\)](#migrate-from-oai-to-oac)\.

**Note**  
If your origin is an Amazon S3 bucket configured as a [website endpoint](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html), you must set it up with CloudFront as a custom origin\. That means you can't use OAC \(or OAI\)\. However, you can restrict access to a custom origin by setting up custom headers and configuring the origin to require them\. For more information, see [ Restricting access to files on custom origins](private-content-overview.md#forward-custom-headers-restrict-access)\.

**Topics**
+ [Creating a new origin access control](#create-oac-overview-s3)
+ [Migrating from origin access identity \(OAI\) to origin access control \(OAC\)](#migrate-from-oai-to-oac)
+ [Advanced settings for origin access control](#oac-advanced-settings-s3)

## Creating a new origin access control<a name="create-oac-overview-s3"></a>

Complete the steps described in the following topics to set up a new origin access control in CloudFront\.

**Topics**
+ [Prerequisites](#oac-prerequisites-s3)
+ [Giving the origin access control permission to access the S3 bucket](#oac-permission-to-access-s3)
+ [Creating the origin access control](#create-oac-s3)

### Prerequisites<a name="oac-prerequisites-s3"></a>

Before you create and set up origin access control \(OAC\), you must have a CloudFront distribution with an Amazon S3 bucket origin\. This origin must be a regular S3 bucket, not a bucket configured as a [website endpoint](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteEndpoints.html)\. For more information about setting up a CloudFront distribution with an S3 bucket origin, see [Getting started with a simple CloudFront distribution](GettingStarted.SimpleDistribution.md)\.

### Giving the origin access control permission to access the S3 bucket<a name="oac-permission-to-access-s3"></a>

Before you create an origin access control \(OAC\) or set it up in a CloudFront distribution, make sure the OAC has permission to access the S3 bucket origin\. Do this after creating a CloudFront distribution, but before adding the OAC to the S3 origin in the distribution configuration\.

To give the OAC permission to access the S3 bucket, use an S3 [bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) to allow the CloudFront service principal \(`cloudfront.amazonaws.com`\) to access the bucket\. Use a `Condition` element in the policy to allow CloudFront to access the bucket only when the request is on behalf of the CloudFront distribution that contains the S3 origin\.

For information about adding or modifying a bucket policy, see [Adding a bucket policy using the Amazon S3 console](https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html) in the *Amazon S3 User Guide*\.

The following are examples of S3 bucket policies that allow a CloudFront OAC to access an S3 origin\.

**Example S3 bucket policy that allows read\-only access to a CloudFront OAC**  

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowCloudFrontServicePrincipalReadOnly",
        "Effect": "Allow",
        "Principal": {
            "Service": "cloudfront.amazonaws.com"
        },
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::<S3 bucket name>/*",
        "Condition": {
            "StringEquals": {
                "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
            }
        }
    }
}
```

**Example S3 bucket policy that allows read and write access to a CloudFront OAC**  

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowCloudFrontServicePrincipalReadWrite",
        "Effect": "Allow",
        "Principal": {
            "Service": "cloudfront.amazonaws.com"
        },
        "Action": [
            "s3:GetObject",
            "s3:PutObject"
        ],
        "Resource": "arn:aws:s3:::<S3 bucket name>/*",
        "Condition": {
            "StringEquals": {
                "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
            }
        }
    }
}
```

#### SSE\-KMS<a name="oac-permissions-sse-kms"></a>

If the objects in the S3 bucket origin are encrypted using [server\-side encryption with AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html), you must make sure that the OAC has permission to use the AWS KMS key\. To give the OAC permission to use the KMS key, add a statement to the [KMS key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)\. For information about how to modify a key policy, see [Changing a key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying.html) in the *AWS Key Management Service Developer Guide*\.

The following example shows a KMS key policy statement that allows the OAC to use the KMS key\.

**Example KMS key policy statement that allows a CloudFront OAC to access a KMS key for SSE\-KMS**  

```
{
    "Sid": "AllowCloudFrontServicePrincipalSSE-KMS",
    "Effect": "Allow",
    "Principal": {
        "AWS": "arn:aws:iam::<AWS account ID>:root",
        "Service": "cloudfront.amazonaws.com"
    },
    "Action": [
        "kms:Decrypt",
        "kms:Encrypt",
        "kms:GenerateDataKey*"
    ],
    "Resource": "*",
    "Condition": {
            "StringEquals": {
                "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
            }
        }
}
```

### Creating the origin access control<a name="create-oac-s3"></a>

To create an origin access control \(OAC\), you can use the AWS Management Console, AWS CloudFormation, the AWS CLI, or the CloudFront API\.

------
#### [ Console ]

**To create an origin access control**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. In the navigation pane, choose **Origin access**\.

1. Choose **Create control setting**\.

1. On the **Create control setting** form, do the following:

   1. In the **Details** pane, enter a **Name** and \(optionally\) a **Description** for the origin access control\.

   1. In the **Settings** pane, we recommend that you leave the default setting \(**Sign requests \(recommended\)**\)\. For more information, see [Advanced settings for origin access control](#oac-advanced-settings-s3)\.

1. Choose S3 from the **Origin type** dropdown\.

1. Choose **Create**\.

   After the OAC is created, make note of the **Name**\. You need this in the following procedure\.

**To add an origin access control to an S3 origin in a distribution**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose a distribution with an S3 origin that you want to add the OAC to, then choose the **Origins** tab\.

1. Select the S3 origin that you want to add the OAC to, then choose **Edit**\.

1. From the **Origin access control** dropdown menu, choose the OAC that you want to use\.

1. Choose **Save changes**\.

The distribution starts deploying to all of the CloudFront edge locations\. When an edge location receives the new configuration, it signs all requests that it sends to the S3 bucket origin\.

------
#### [ CloudFormation ]

To create an origin access control \(OAC\) with AWS CloudFormation, use the `AWS::CloudFront::OriginAccessControl` resource type\. The following example shows the AWS CloudFormation template syntax, in YAML format, for creating an origin access control\.

```
Type: AWS::CloudFront::OriginAccessControl
Properties: 
  OriginAccessControlConfig: 
      Description: An optional description for the origin access control
      Name: ExampleOAC
      OriginAccessControlOriginType: s3
      SigningBehavior: always
      SigningProtocol: sigv4
```

For more information, see [AWS::CloudFront::OriginAccessControl](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudfront-originaccesscontrol.html) in the *AWS CloudFormation User Guide*\.

------
#### [ CLI ]

To create an origin access control with the AWS Command Line Interface \(AWS CLI\), use the aws cloudfront create\-origin\-access\-control command\. You can use an input file to provide the input parameters for the command, rather than specifying each individual parameter as command line input\.

**To create an origin access control \(CLI with input file\)**

1. Use the following command to create a file that's named `origin-access-control.yaml`\. This file contains all of the input parameters for the create\-origin\-access\-control command\.

   ```
   aws cloudfront create-origin-access-control --generate-cli-skeleton yaml-input > origin-access-control.yaml
   ```

1. Open the `origin-access-control.yaml` file that you just created\. Edit the file to add a name for the OAC, a description \(optional\), and change the `SigningBehavior` to `always`\. Then save the file\.

   For information about other OAC settings, see [Advanced settings for origin access control](#oac-advanced-settings-s3)\.

1. Use the following command to create the origin access control using the input parameters from the `origin-access-control.yaml` file\.

   ```
   aws cloudfront create-origin-access-control --cli-input-yaml file://origin-access-control.yaml
   ```

   Make note of the `Id` value in the command output\. You need it to add the OAC to an S3 bucket origin in a CloudFront distribution\.

**To attach an OAC to an S3 bucket origin in an existing distribution \(CLI with input file\)**

1. Use the following command to save the distribution configuration for the CloudFront distribution that you want to add the OAC to\. The distribution must have an S3 bucket origin\.

   ```
   aws cloudfront get-distribution-config --id <CloudFront distribution ID> --output yaml > dist-config.yaml
   ```

1. Open the file that's named `dist-config.yaml` that you just created\. Edit the file, making the following changes:
   + In the `Origins` object, add the OAC's ID to the field that's named `OriginAccessControlId`\.
   + Remove the value from the field that's named `OriginAccessIdentity`, if one exists\.
   + Rename the `ETag` field to `IfMatch`, but don't change the field's value\.

   Save the file when finished\.

1. Use the following command to update the distribution to use the origin access control\.

   ```
   aws cloudfront update-distribution --id <CloudFront distribution ID> --cli-input-yaml file://dist-config.yaml
   ```

The distribution starts deploying to all of the CloudFront edge locations\. When an edge location receives the new configuration, it signs all requests that it sends to the S3 bucket origin\.

------
#### [ API ]

To create an origin access control with the CloudFront API, use [CreateOriginAccessControl](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateOriginAccessControl.html)\. For more information about the fields that you specify in this API call, see the API reference documentation for your AWS SDK or other API client\.

After you create an origin access control you can attach it to an S3 bucket origin in a distribution, using one of the following API calls:
+ To attach it to an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the origin access control ID in the `OriginAccessControlId` field, inside an origin\. For more information about the other fields that you specify in these API calls, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.

------

## Migrating from origin access identity \(OAI\) to origin access control \(OAC\)<a name="migrate-from-oai-to-oac"></a>

To migrate from a legacy origin access identity \(OAI\) to an origin access control \(OAC\), first update the S3 bucket origin to allow both the OAI and OAC to access the bucket's content\. This makes sure that CloudFront never loses access to the bucket during the transition\. To allow both OAI and OAC to access an S3 bucket, update the [bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) to include two statements, one for each kind of principal\.

The following example S3 bucket policy allows both an OAI and an OAC to access an S3 origin\.

**Example S3 bucket policy that allows read\-only access to an OAI and an OAC**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipalReadOnly",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<S3 bucket name>/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
                }
            }
        },
        {
            "Sid": "AllowLegacyOAIReadOnly",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <origin access identity ID>"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<S3 bucket name>/*"
        }
    ]
}
```

After you update the S3 origin's bucket policy to allow access to both OAI and OAC, you can update the distribution configuration to use OAC instead of OAI\. For more information, see [Creating a new origin access control](#create-oac-overview-s3)\.

After the distribution is fully deployed, you can remove the statement in the bucket policy that allows access to the OAI\. For more information, see [Giving the origin access control permission to access the S3 bucket](#oac-permission-to-access-s3)\.

## Advanced settings for origin access control<a name="oac-advanced-settings-s3"></a>

The CloudFront origin access control feature includes advanced settings that are intended only for specific use cases\. Use the recommended settings unless you have a specific need for the advanced settings\.

Origin access control contains a setting named **Signing behavior** \(in the console\), or `SigningBehavior` \(in the API, CLI, and AWS CloudFormation\)\. This setting provides the following options:

**Always sign origin requests \(recommended setting\)**  
We recommend using this setting, named **Sign requests \(recommended\)** in the console, or `always` in the API, CLI, and AWS CloudFormation\. With this setting, CloudFront always signs all requests that it sends to the S3 bucket origin\.

**Never sign origin requests**  
This setting is named **Do not sign requests** in the console, or `never` in the API, CLI, and AWS CloudFormation\. Use this setting to turn off origin access control for all origins in all distributions that use this origin access control\. This can save time and effort compared to removing an origin access control from all origins and distributions that use it, one by one\. With this setting, CloudFront does not sign any requests that it sends to the S3 bucket origin\.  
To use this setting, the S3 bucket origin must be publicly accessible\. If you use this setting with an S3 bucket origin that's not publicly accessible, CloudFront cannot access the origin\. The S3 bucket origin returns errors to CloudFront and CloudFront passes those errors on to viewers\.

**Don't override the viewer \(client\) `Authorization` header**  
This setting is named **Do not override authorization header** in the console, or `no-override` in the API, CLI, and AWS CloudFormation\. Use this setting when you want CloudFront to sign origin requests only when the corresponding viewer request does not include an `Authorization` header\. With this setting, CloudFront passes on the `Authorization` header from the viewer request when one is present, but signs the origin request \(adding its own `Authorization` header\) when the viewer request doesn't include an `Authorization` header\.  
To pass along the `Authorization` header from the viewer request, you *must* add the `Authorization` header to a [cache policy](controlling-the-cache-key.md) for all cache behaviors that use S3 bucket origins associated with this origin access control\.

## Using an origin access identity \(legacy, not recommended\)<a name="private-content-restricting-access-to-s3-oai"></a>

### Overview of origin access identity<a name="private-content-restricting-access-to-s3-overview"></a>

CloudFront *origin access identity* \(OAI\) provides similar functionality as *origin access control* \(OAC\), but it doesn't work for all scenarios\. This is why we recommend using OAC instead\. Specifically, OAI doesn't support:
+ Amazon S3 buckets in all AWS Regions, including opt\-in Regions
+ Amazon S3 [server\-side encryption with AWS KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html) \(SSE\-KMS\)
+ Dynamic requests \(`PUT`, `POST`, or `DELETE`\) to Amazon S3
+ New AWS Regions launched after December 2022

For information about how to migrating from OAI to OAC, see [Migrating from origin access identity \(OAI\) to origin access control \(OAC\)](#migrate-from-oai-to-oac)\.

### Giving an origin access identity permission to read files in the Amazon S3 bucket<a name="private-content-granting-permissions-to-oai"></a>

When you create an OAI or add one to a distribution with the CloudFront console, you can automatically update the Amazon S3 bucket policy to give the OAI permission to access your bucket\. Alternatively, you can choose to manually create or update the bucket policy\. Whichever method you use, you should still review the permissions to make sure that:
+ Your CloudFront OAI can access files in the bucket on behalf of viewers who are requesting them through CloudFront\.
+ Viewers can't use Amazon S3 URLs to access your files outside of CloudFront\.

**Important**  
If you configure CloudFront to accept and forward all of the HTTP methods that CloudFront supports, make sure you give your CloudFront OAI the desired permissions\. For example, if you configure CloudFront to accept and forward requests that use the `DELETE` method, configure your bucket policy to handle `DELETE` requests appropriately so viewers can delete only files that you want them to\.

#### Using Amazon S3 bucket policies<a name="private-content-updating-s3-bucket-policies"></a>

You can give a CloudFront OAI access to files in an Amazon S3 bucket by creating or updating the bucket policy in the following ways:
+ Using the Amazon S3 bucket's **Permissions** tab in the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.
+ Using [PutBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketPolicy.html) in the Amazon S3 API\.
+ Using the [CloudFront console](https://console.aws.amazon.com/cloudfront/v3/home)\. When you add an OAI to your origin settings in the CloudFront console, you can choose **Yes, update the bucket policy** to tell CloudFront to update the bucket policy on your behalf\.

If you update the bucket policy manually, make sure that you:
+ Specify the correct OAI as the `Principal` in the policy\.
+ Give the OAI the permissions it needs to access objects on behalf of viewers\.

For more information, see the following sections\.

##### Specify an OAI as the `Principal` in a bucket policy<a name="private-content-updating-s3-bucket-policies-principal"></a>

To specify an OAI as the `Principal` in an Amazon S3 bucket policy, use the OAI's Amazon Resource Name \(ARN\), which includes the OAI's ID\. For example:

```
"Principal": {
    "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <origin access identity ID>"
}
```

To find the OAI's ID, see the [Origin access identities page](https://console.aws.amazon.com/cloudfront/v3/home#/oai) in the CloudFront console, or use [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

##### Give permissions to an OAI<a name="private-content-updating-s3-bucket-policies-permissions"></a>

To give the OAI the permissions to access objects in your Amazon S3 bucket, use actions in the policy that relate to specific Amazon S3 API operations\. For example, the `s3:GetObject` action allows the OAI to read objects in the bucket\. For more information, see the examples in the following section, or see [Amazon S3 actions](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service User Guide*\.

##### Amazon S3 bucket policy examples<a name="private-content-updating-s3-bucket-policies-examples"></a>

The following examples show Amazon S3 bucket policies that allow CloudFront OAI to access an S3 bucket\.

To find the OAI's ID, see the [Origin access identities page](https://console.aws.amazon.com/cloudfront/v3/home#/oai) in the CloudFront console, or use [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

**Example Amazon S3 bucket policy that gives the OAI read access**  
The following example allows the OAI to read objects in the specified bucket \(`s3:GetObject`\)\.  

```
{
    "Version": "2012-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <origin access identity ID>"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<S3 bucket name>/*"
        }
    ]
}
```

**Example Amazon S3 bucket policy that gives the OAI read and write access**  
The following example allows the OAI to read and write objects in the specified bucket \(`s3:GetObject` and `s3:PutObject`\)\. This allows viewers to upload files to your Amazon S3 bucket through CloudFront\.  

```
{
    "Version": "2012-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity <origin access identity ID>"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::<S3 bucket name>/*"
        }
    ]
}
```

#### Using Amazon S3 object ACLs \(not recommended\)<a name="private-content-updating-s3-acls"></a>

**Important**  
We recommend [using Amazon S3 bucket policies](#private-content-updating-s3-bucket-policies) to give an OAI access to an S3 bucket\. You can use ACLs as described in this section, but we don't recommend it\.  
Amazon S3 recommends setting [S3 Object Ownership](https://docs.aws.amazon.com/AmazonS3/latest/userguide/about-object-ownership.html) to **bucket owner enforced**, which means that ACLs are disabled for the bucket and the objects in it\. When you apply this setting for Object Ownership, you must use bucket policies to give access to the OAI \(see the previous section\)\.  
This following section is only for legacy use cases that require ACLs\.

You can give a CloudFront OAI access to files in an Amazon S3 bucket by creating or updating the file's ACL in the following ways:
+ Using the Amazon S3 object's **Permissions** tab in the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.
+ Using [PutObjectAcl](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectAcl.html) in the Amazon S3 API\.

When you grant access to an OAI using an ACL, you must specify the OAI using its Amazon S3 canonical user ID\. This is the value of **Amazon S3 canonical user ID** on the [Origin access identities page](https://console.aws.amazon.com/cloudfront/v3/home#/oai) in the CloudFront console\. If you're using the CloudFront API, use the value of the `S3CanonicalUserId` element that was returned when you created the OAI, or call [ListCloudFrontOriginAccessIdentities](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListCloudFrontOriginAccessIdentities.html) in the CloudFront API\.

### Using an origin access identity in Amazon S3 regions that support only signature version 4 authentication<a name="private-content-origin-access-identity-signature-version-4"></a>

Newer Amazon S3 Regions require that you use Signature Version 4 for authenticated requests\. \(For the signature versions supported in each Amazon S3 Region, see [Amazon Simple Storage Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/s3.html) in the *AWS General Reference*\.\) If you're using an origin access identity and if your bucket is in one of the Regions that requires Signature Version 4, note the following:
+ `DELETE`, `GET`, `HEAD`, `OPTIONS`, and `PATCH` requests are supported without qualifications\.
+ If you want to submit `PUT` requests to CloudFront to upload files to your Amazon S3 bucket, you must add an `x-amz-content-sha256` header to the request\.The header value must contain a SHA\-256 hash of the body of the request\. For more information, see the documentation about the `x-amz-content-sha256` header on the [Common Request Headers](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonRequestHeaders.html) page in the *Amazon Simple Storage Service API Reference*\.
+ `POST` requests are not supported\.