# Restricting access to Application Load Balancers<a name="restrict-access-to-load-balancer"></a>

For a web application or other content that’s served by an Application Load Balancer in Elastic Load Balancing, CloudFront can cache objects and serve them directly to users \(viewers\), reducing the load on your Application Load Balancer\. CloudFront can also help to reduce latency and even absorb some distributed denial of service \(DDoS\) attacks\. However, if users can bypass CloudFront and access your Application Load Balancer directly, you don’t get these benefits\. But you can configure Amazon CloudFront and your Application Load Balancer to prevent users from directly accessing the Application Load Balancer\. This allows users to access the Application Load Balancer only through CloudFront, ensuring that you get the benefits of using CloudFront\.

To prevent users from directly accessing an Application Load Balancer and allow access only through CloudFront, complete these high\-level steps:

1. Configure CloudFront to add a custom HTTP header to requests that it sends to the Application Load Balancer\.

1. Configure the Application Load Balancer to only forward requests that contain the custom HTTP header\.

1. \(Optional\) Require HTTPS to improve the security of this solution\.

For more information, see the following topics\. After you complete these steps, users can only access your Application Load Balancer through CloudFront\.

**Topics**
+ [Configuring CloudFront to add a custom HTTP header to requests](#restrict-alb-add-custom-header)
+ [Configuring an Application Load Balancer to only forward requests that contain a specific header](#restrict-alb-route-based-on-header)
+ [\(Optional\) Improve the security of this solution](#restrict-alb-improve-security)

## Configuring CloudFront to add a custom HTTP header to requests<a name="restrict-alb-add-custom-header"></a>

You can configure CloudFront to add a custom HTTP header to the requests that it sends to your origin \(in this case, an Application Load Balancer\)\.

**Important**  
This use case relies on keeping the custom header name and value secret\. If the header name and value are not secret, other HTTP clients could potentially include them in requests that they send directly to the Application Load Balancer\. This can cause the Application Load Balancer to behave as though the requests came from CloudFront when they did not\. To prevent this, keep the custom header name and value secret\.

You can configure CloudFront to add a custom HTTP header to origin requests with the CloudFront console, AWS CloudFormation, or the CloudFront API\.

**To add a custom HTTP header \(CloudFront console\)**  
In the CloudFront console, use the **Origin Custom Headers** setting in **Origin Settings**\. Enter the **Header Name** and its **Value**, as shown in the following example\.  
The header name and value in this example are just for demonstration\. In production, use randomly generated values\. Treat the header name and value as a secure credential, like a user name and password\.

![\[Origin Custom Headers fields in the CloudFront console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/origin-custom-header.png)
You can edit the **Origin Custom Headers** setting when you create or edit an origin for an existing CloudFront distribution, and when you create a new distribution\. For more information, see [Updating a distribution](HowToUpdateDistribution.md) and [Creating a distribution](distribution-web-creating-console.md)\.

**To add a custom HTTP header \(AWS CloudFormation\)**  
In an AWS CloudFormation template, use the `OriginCustomHeaders` property, as shown in the following example\.  
The header name and value in this example are just for demonstration\. In production, use randomly generated values\. Treat the header name and value as a secure credential, like a user name and password\.

```
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  TestDistribution:
    Type: 'AWS::CloudFront::Distribution'
    Properties:
      DistributionConfig:
        Origins:
          - DomainName: app-load-balancer.example.com
            Id: Example-ALB
            CustomOriginConfig:
              OriginProtocolPolicy: https-only
              OriginSSLProtocols:
                - TLSv1.2
            OriginCustomHeaders:
               - HeaderName: X-Custom-Header
                 HeaderValue: random-value-1234567890
        Enabled: 'true'
        DefaultCacheBehavior:
          TargetOriginId: Example-ALB
          ViewerProtocolPolicy: allow-all
          CachePolicyId: 658327ea-f89d-4fab-a63d-7e88639e58f6
        PriceClass: PriceClass_All
        ViewerCertificate:
          CloudFrontDefaultCertificate: 'true'
```
For more information, see the [Origin](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-cloudfront-distribution-origin.html) and [OriginCustomHeader](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-cloudfront-distribution-origincustomheader.html) properties in the *AWS CloudFormation User Guide*\.

**To add a custom HTTP header \(CloudFront API\)**  
In the CloudFront API, use the `CustomHeaders` object inside `Origin`\. For more information, see [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) and [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*, and the documentation for your SDK or other API client\.

There are some header names that you can’t specify as origin custom headers\. For more information, see [Custom headers that CloudFront can’t add to origin requests](add-origin-custom-headers.md#add-origin-custom-headers-denylist)\.

## Configuring an Application Load Balancer to only forward requests that contain a specific header<a name="restrict-alb-route-based-on-header"></a>

After you configure CloudFront to add a custom HTTP header to the requests that it sends to your Application Load Balancer \(see [the previous section](#restrict-alb-add-custom-header)\), you can configure the load balancer to only forward requests that contain this custom header\. You do this by adding a new rule and modifying the default rule in your load balancer’s listener\.

**Prerequisites**  
To use the following procedures, you need an Application Load Balancer with at least one listener\. If you haven’t created one yet, see [Create an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-application-load-balancer.html) in the *User Guide for Application Load Balancers*\.

The following procedures modify an HTTPS listener\. You can use the same process to modify an HTTP listener\.

**To update the rules in an Application Load Balancer listener**

1. Open the [**Load Balancers** page](https://console.aws.amazon.com/ec2/v2/home#LoadBalancers:) in the Amazon EC2 console\.

1. Choose the load balancer that is the origin for your CloudFront distribution, then choose the **Listeners** tab\.

1. For the listener that you are modifying, choose **View/edit rules**\.  
![\[View/edit rules link for a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-view-edit-rules.png)

1. Choose the icon to add rules\.  
![\[Add rules to a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-add-rules.png)

1. Choose **Insert Rule**\.  
![\[Insert rule in a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-insert-rule.png)

1. For the new rule, do the following:

   1. Choose **Add condition** and then choose **Http header**\. Specify the HTTP header name and value that you added as an origin custom header in CloudFront\.

   1. Choose **Add action** and then choose **Forward to**\. Choose the target group where you want to forward requests\.

   1. Choose **Save** to create the new rule\.  
![\[New rule in a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-rule-settings.png)

1. Choose the icon to edit rules\.  
![\[Edit rules for a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-edit-rules.png)

1. Choose the edit icon for the default rule\.  
![\[Edit the default rule for a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-edit-default-rule.png)

1. For the default rule, do the following:

   1. Delete the default action\.  
![\[Delete default rule for a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-delete-default-action.png)

   1. Choose **Add action** and then choose **Return fixed response**\.

   1. For **Response code**, enter **403**\.

   1. For **Response body**, enter **Access denied**\.

   1. Choose **Update** to update the default rule\.  
![\[Update default rule for a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-update-default-rule.png)

After you complete these steps, your load balancer listener has two rules, as shown in the following image\. The first rule forwards requests that contain the HTTP header \(requests that come from CloudFront\)\. The second rule sends a fixed response to all other requests \(requests that don’t come from CloudFront\)\.

![\[Updated rules for a load balancer in the Amazon EC2 console.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/app-load-balancer-updated-rules.png)

You can verify that the solution works by sending a request to your CloudFront distribution and one to your Application Load Balancer\. The request to CloudFront returns your web application or content, and the one sent directly to your Application Load Balancer returns a `403` response with the plain text message `Access denied`\.

## \(Optional\) Improve the security of this solution<a name="restrict-alb-improve-security"></a>

To improve the security of this solution, you can configure your CloudFront distribution to always use HTTPS when sending requests to your Application Load Balancer\. Remember, this solution only works if you keep the custom header name and value secret\. Using HTTPS can help prevent an eavesdropper from discovering the header name and value\. We also recommend rotating the header name and value periodically\.

**Use HTTPS for origin requests**  
To configure CloudFront to use HTTPS for origin requests, set the **Origin Protocol Policy** setting to **HTTPS Only**\. This setting is available in the CloudFront console, AWS CloudFormation, and the CloudFront API\. For more information, see [Origin protocol policy](distribution-web-values-specify.md#DownloadDistValuesOriginProtocolPolicy)\.

When you configure CloudFront to use HTTPS for origin requests, you need to make sure that your Application Load Balancer has an HTTPS listener \(as shown in [the preceding section](#restrict-alb-route-based-on-header)\)\. This requires that you have an SSL/TLS certificate that matches the domain name that is routed to your Application Load Balancer\. For more information, see [Create an HTTPS listener](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html) in the *User Guide for Application Load Balancers*\.

If the end users \(also known as *viewers*, or *clients*\) of your web application can use HTTPS, you can also configure CloudFront to prefer \(or even require\) HTTPS connections from the end users\. To do this, use the **Viewer Protocol Policy** setting\. You can set it to redirect end users from HTTP to HTTPS, or to reject requests that use HTTP\. This setting is available in the CloudFront console, AWS CloudFormation, and the CloudFront API\. For more information, see [Viewer protocol policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\.

**Rotate the header name and value**  
In addition to using HTTPS, we also recommend rotating the header name and value periodically\. The high\-level steps for doing this are as follows:

1. Configure CloudFront to add an additional custom HTTP header to requests that it sends to the Application Load Balancer\.

1. Update the Application Load Balancer listener rule to forward requests that contain this additional custom HTTP header\.

1. Configure CloudFront to stop adding the original custom HTTP header to requests that it sends to the Application Load Balancer\.

1. Update the Application Load Balancer listener rule to stop forwarding requests that contain the original custom HTTP header\.

For more information about accomplishing these steps, see the preceding sections\.