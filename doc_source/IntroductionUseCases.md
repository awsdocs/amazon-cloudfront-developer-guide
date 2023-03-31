# CloudFront use cases<a name="IntroductionUseCases"></a>

Using CloudFront can help you accomplish a variety of goals\. This section lists just a few, together with links to more information, to give you an idea of the possibilities\.

**Topics**
+ [Accelerate static website content delivery](#IntroductionUseCasesStaticWebsite)
+ [Serve video on demand or live streaming video](#IntroductionUseCasesStreaming)
+ [Encrypt specific fields throughout system processing](#IntroductionUseCasesFieldLevelEncryption)
+ [Customize at the edge](#IntroductionUseCasesProgrammableCDN)
+ [Serve private content by using Lambda@Edge customizations](#IntroductionUseCasesPrivateContentAtTheEdge)

## Accelerate static website content delivery<a name="IntroductionUseCasesStaticWebsite"></a>

CloudFront can speed up the delivery of your static content \(for example, images, style sheets, JavaScript, and so on\) to viewers across the globe\. By using CloudFront, you can take advantage of the AWS backbone network and CloudFront edge servers to give your viewers a fast, safe, and reliable experience when they visit your website\.

A simple approach for storing and delivering static content is to use an Amazon S3 bucket\. Using S3 together with CloudFront has a number of advantages, including the option to use [origin access control](private-content-restricting-access-to-s3.md) to easily restrict access to your S3 content\.

For more information about using S3 together with CloudFront, including a AWS CloudFormation template to help you get started quickly, see [ Amazon S3 \+ Amazon CloudFront: A Match Made in the Cloud](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-s3-amazon-cloudfront-a-match-made-in-the-cloud/)\.

## Serve video on demand or live streaming video<a name="IntroductionUseCasesStreaming"></a>

CloudFront offers several options for streaming your media to global viewers—both pre\-recorded files and live events\.
+ For video on demand \(VOD\) streaming, you can use CloudFront to stream in common formats such as MPEG DASH, Apple HLS, Microsoft Smooth Streaming, and CMAF, to any device\.
+ For broadcasting a live stream, you can cache media fragments at the edge, so that multiple requests for the manifest file that delivers the fragments in the right order can be combined, to reduce the load on your origin server\.

For more information about how to deliver streaming content with CloudFront, see [Video on demand and live streaming video with CloudFront](on-demand-streaming-video.md)\.

## Encrypt specific fields throughout system processing<a name="IntroductionUseCasesFieldLevelEncryption"></a>

When you configure HTTPS with CloudFront, you already have secure end\-to\-end connections to origin servers\. When you add field\-level encryption, you can protect specific data throughout system processing in addition to HTTPS security, so that only certain applications at your origin can see the data\.

To set up field\-level encryption, you add a public key to CloudFront, and then specify the set of fields that you want to be encrypted with the key\. For more information, see [Using field\-level encryption to help protect sensitive data](field-level-encryption.md)\.

## Customize at the edge<a name="IntroductionUseCasesProgrammableCDN"></a>

Running serverless code at the edge opens up a number of possibilities for customizing the content and experience for viewers, at reduced latency\. For example, you can return a custom error message when your origin server is down for maintenance, so viewers don't get a generic HTTP error message\. Or you can use a function to help authorize users and control access to your content, before CloudFront forwards a request to your origin\.

Using Lambda@Edge with CloudFront enables a variety of ways to customize the content that CloudFront delivers\. To learn more about Lambda@Edge and how to create and deploy functions with CloudFront, see [Customizing at the edge with Lambda@Edge](lambda-at-the-edge.md)\. To see a number of code samples that you can customize for your own solutions, see [Lambda@Edge example functions](lambda-examples.md)\.

## Serve private content by using Lambda@Edge customizations<a name="IntroductionUseCasesPrivateContentAtTheEdge"></a>

Using Lambda@Edge can help you configure your CloudFront distribution to serve private content from your own custom origin, in addition to using signed URLs or signed cookies\.

To serve private content using CloudFront, you do the following:
+ Require that your users \(viewers\) access content using [signed URLs or signed cookies](PrivateContent.md)\.
+ Restrict access to your origin so that it's only available from CloudFront's origin\-facing servers\. To do this, you can do one of the following:
  + For an Amazon S3 origin, you can [use an origin access control \(OAC\)](private-content-restricting-access-to-s3.md)\.
  + For a custom origin, you can do the following:
    + If the custom origin is protected by an Amazon VPC security group or AWS Firewall Manager, you can [use the CloudFront managed prefix list](LocationsOfEdgeServers.md#managed-prefix-list) to allow inbound traffic to your origin from only CloudFront's origin\-facing IP addresses\.
    + Use a custom HTTP header to restrict access to only requests from CloudFront\. For more information, see [ Restricting access to files on custom origins](private-content-overview.md#forward-custom-headers-restrict-access) and [Adding custom headers to origin requests](add-origin-custom-headers.md)\. For an example that uses a custom header to restrict access to an Application Load Balancer origin, see [Restricting access to Application Load Balancers](restrict-access-to-load-balancer.md)\.
    + If the custom origin requires custom access control logic, you can use Lambda@Edge to implement that logic, as described in this blog post: [Serving Private Content Using Amazon CloudFront & Lambda@Edge](https://aws.amazon.com/blogs/networking-and-content-delivery/serving-private-content-using-amazon-cloudfront-aws-lambdaedge/)\.