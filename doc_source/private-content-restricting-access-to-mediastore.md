# Restricting access to a MediaStore origin<a name="private-content-restricting-access-to-mediastore"></a>

CloudFront provides *origin access control* \(OAC\) for restricting access to an AWS Elemental MediaStore origin\.

**Topics**
+ [Creating a new origin access control](#create-oac-overview-mediastore)
+ [Advanced settings for origin access control](#oac-advanced-settings-mediastore)

## Creating a new origin access control<a name="create-oac-overview-mediastore"></a>

Complete the steps described in the following topics to set up a new origin access control in CloudFront\.

**Topics**
+ [Prerequisites](#oac-prerequisites-mediastore)
+ [Giving the origin access control permission to access the MediaStore origin](#oac-permission-to-access-mediastore)
+ [Creating the origin access control](#create-oac-mediastore)

### Prerequisites<a name="oac-prerequisites-mediastore"></a>

Before you create and set up origin access control, you must have a CloudFront distribution with a MediaStore origin\. 

### Giving the origin access control permission to access the MediaStore origin<a name="oac-permission-to-access-mediastore"></a>

Before you create an origin access control or set it up in a CloudFront distribution, make sure the OAC has permission to access the MediaStore origin\. Do this after creating a CloudFront distribution, but before adding the OAC to the MediaStore origin in the distribution configuration\.

To give the OAC permission to access the MediaStore origin, use a MediaStore container policy to allow the CloudFront service principal \(`cloudfront.amazonaws.com`\) to access the origin\. Use a `Condition` element in the policy to allow CloudFront to access the MediaStore container only when the request is on behalf of the CloudFront distribution that contains the MediaStore origin\.

The following are examples of MediaStore container policies that allow a CloudFront OAC to access a MediaStore origin\.

**Example MediaStore container policy that allows read\-only access to a CloudFront OAC**  

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
                "Action": [ 
                  "mediastore:GetObject"
                ],
                "Resource": "arn:aws:mediastore:<region>:<AWS account ID>:container/<container name>/*",
                "Condition": {
                    "StringEquals": {
                      "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
                    },
                    "Bool": {
                      "aws:SecureTransport": "true"
                    }
                }
            }
        ]
}
```

**Example MediaStore container policy that allows read and write access to a CloudFront OAC**  

```
{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AllowCloudFrontServicePrincipalReadWrite",
                "Effect": "Allow",
                "Principal": {
                  "Service": "cloudfront.amazonaws.com"
                },
                "Action": [ 
                  "mediastore:GetObject",
                  "mediastore:PutObject"
                ],
                "Resource": "arn:aws:mediastore:<region>:<AWS account ID>:container/<container name>/*",
                "Condition": {
                    "StringEquals": {
                      "AWS:SourceArn": "arn:aws:cloudfront::<AWS account ID>:distribution/<CloudFront distribution ID>"
                    },
                    "Bool": {
                      "aws:SecureTransport": "true"
                    }
                }
            }
        ]
}
```

**Note**  
To allow write access, you must configure **Allowed HTTP methods** to include `PUT` in your CloudFront distribution's behavior settings\.

### Creating the origin access control<a name="create-oac-mediastore"></a>

To create an OAC, you can use the AWS Management Console, AWS CloudFormation, the AWS CLI, or the CloudFront API\.

------
#### [ Console ]

**To create an origin access control**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. In the navigation pane, choose **Origin access**\.

1. Choose **Create control setting**\.

1. On the **Create control setting** form, do the following:

   1. In the **Details** pane, enter a **Name** and \(optionally\) a **Description** for the origin access control\.

   1. In the **Settings** pane, we recommend that you leave the default setting \(**Sign requests \(recommended\)**\)\. For more information, see [Advanced settings for origin access control](#oac-advanced-settings-mediastore)\.

1. Choose MediaStore from the **Origin type** dropdown\.

1. Choose **Create**\.

   After the OAC is created, make note of the **Name**\. You need this in the following procedure\.

**To add an origin access control to a MediaStore origin in a distribution**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose a distribution with a MediaStore origin that you want to add the OAC to, then choose the **Origins** tab\.

1. Select the MediaStore origin that you want to add the OAC to, then choose **Edit**\.

1. Select **HTTPS only** for your origin's **Protocol**\.

1. From the **Origin access control** dropdown menu, choose the OAC that you want to use\.

1. Choose **Save changes**\.

The distribution starts deploying to all of the CloudFront edge locations\. When an edge location receives the new configuration, it signs all requests that it sends to the MediaStore bucket origin\.

------
#### [ CloudFormation ]

To create an origin access control \(OAC\) with AWS CloudFormation, use the `AWS::CloudFront::OriginAccessControl` resource type\. The following example shows the AWS CloudFormation template syntax, in YAML format, for creating an origin access control\.

```
Type: AWS::CloudFront::OriginAccessControl
Properties: 
  OriginAccessControlConfig: 
      Description: An optional description for the origin access control
      Name: ExampleOAC
      OriginAccessControlOriginType: mediastore
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

   For information about other OAC settings, see [Advanced settings for origin access control](#oac-advanced-settings-mediastore)\.

1. Use the following command to create the origin access control using the input parameters from the `origin-access-control.yaml` file\.

   ```
   aws cloudfront create-origin-access-control --cli-input-yaml file://origin-access-control.yaml
   ```

   Make note of the `Id` value in the command output\. You need it to add the OAC to a MediaStore origin in a CloudFront distribution\.

**To attach an OAC to a MediaStore origin in an existing distribution \(CLI with input file\)**

1. Use the following command to save the distribution configuration for the CloudFront distribution that you want to add the OAC to\. The distribution must have a MediaStore origin\.

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

The distribution starts deploying to all of the CloudFront edge locations\. When an edge location receives the new configuration, it signs all requests that it sends to the MediaStore origin\.

------
#### [ API ]

To create an origin access control with the CloudFront API, use [CreateOriginAccessControl](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateOriginAccessControl.html)\. For more information about the fields that you specify in this API call, see the API reference documentation for your AWS SDK or other API client\.

After you create an origin access control you can attach it to a MediaStore origin in a distribution, using one of the following API calls:
+ To attach it to an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the origin access control ID in the `OriginAccessControlId` field, inside an origin\. For more information about the other fields that you specify in these API calls, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.

------

## Advanced settings for origin access control<a name="oac-advanced-settings-mediastore"></a>

The CloudFront origin access control feature includes advanced settings that are intended only for specific use cases\. Use the recommended settings unless you have a specific need for the advanced settings\.

Origin access control contains a setting named **Signing behavior** \(in the console\), or `SigningBehavior` \(in the API, CLI, and AWS CloudFormation\)\. This setting provides the following options:

**Always sign origin requests \(recommended setting\)**  
We recommend using this setting, named **Sign requests \(recommended\)** in the console, or `always` in the API, CLI, and AWS CloudFormation\. With this setting, CloudFront always signs all requests that it sends to the MediaStore origin\.

**Never sign origin requests**  
This setting is named **Do not sign requests** in the console, or `never` in the API, CLI, and AWS CloudFormation\. Use this setting to turn off origin access control for all origins in all distributions that use this origin access control\. This can save time and effort compared to removing an origin access control from all origins and distributions that use it, one by one\. With this setting, CloudFront does not sign any requests that it sends to the MediaStore origin\.  
To use this setting, the MediaStore origin must be publicly accessible\. If you use this setting with a MediaStore origin that's not publicly accessible, CloudFront cannot access the origin\. The MediaStore origin returns errors to CloudFront and CloudFront passes those errors on to viewers\. For more information, see the example MediaStore container policy for [Public read access over HTTPS](https://docs.aws.amazon.com/mediastore/latest/ug/policies-examples-public-https.html)\.

**Don't override the viewer \(client\) `Authorization` header**  
This setting is named **Do not override authorization header** in the console, or `no-override` in the API, CLI, and AWS CloudFormation\. Use this setting when you want CloudFront to sign origin requests only when the corresponding viewer request does not include an `Authorization` header\. With this setting, CloudFront passes on the `Authorization` header from the viewer request when one is present, but signs the origin request \(adding its own `Authorization` header\) when the viewer request doesn't include an `Authorization` header\.  
To pass along the `Authorization` header from the viewer request, you *must* add the `Authorization` header to a [cache policy](controlling-the-cache-key.md) for all cache behaviors that use MediaStore origins associated with this origin access control\.