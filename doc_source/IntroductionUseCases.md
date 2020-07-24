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

A simple approach for storing and delivering static content is to use an Amazon S3 bucket\. Using S3 together with CloudFront has a number of advantages, including the option to use Origin Access Identity \(OAI\) to easily restrict access to your S3 content\.

For more information about using S3 together with CloudFront, including a AWS CloudFormation template to help you get started quickly, see [ Amazon S3 \+ Amazon CloudFront: A Match Made in the Cloud](https://aws.amazon.com/blogs/networking-and-content-delivery/amazon-s3-amazon-cloudfront-a-match-made-in-the-cloud/)\.

## Serve video on demand or live streaming video<a name="IntroductionUseCasesStreaming"></a>

CloudFront offers several options for streaming your media to global viewers—both pre\-recorded files and live events\.
+ For video on demand \(VOD\) streaming, you can use CloudFront to stream in common formats such as MPEG DASH, Apple HLS, Microsoft Smooth Streaming, and CMAF, to any device\.
+ For broadcasting a live stream, you can cache media fragments at the edge, so that multiple requests for the manifest file that delivers the fragments in the right order can be combined, to reduce the load on your origin server\.

For more information about how to deliver streaming content with CloudFront, see [Video on Demand and Live Streaming Video with CloudFront](on-demand-streaming-video.md)\.

## Encrypt specific fields throughout system processing<a name="IntroductionUseCasesFieldLevelEncryption"></a>

When you configure HTTPS with CloudFront, you already have secure end\-to\-end connections to origin servers\. When you add field\-level encryption, you can protect specific data throughout system processing in addition to HTTPS security, so that only certain applications at your origin can see the data\.

To set up field\-level encryption, you add a public key to CloudFront, and then specify the set of fields that you want to be encrypted with the key\. For more information, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\.

## Customize at the edge<a name="IntroductionUseCasesProgrammableCDN"></a>

Running serverless code at the edge opens up a number of possibilities for customizing the content and experience for viewers, at reduced latency\. For example, you can return a custom error message when your origin server is down for maintenance, so viewers don't get a generic HTTP error message\. Or you can use a function to help authorize users and control access to your content, before CloudFront forwards a request to your origin\.

Using Lambda@Edge with CloudFront enables a variety of ways to customize the content that CloudFront delivers\. To learn more about Lambda@Edge and how to create and deploy functions with CloudFront, see [Customizing Content at the Edge with Lambda@Edge](lambda-at-the-edge.md)\. To see a number of code samples that you can customize for your own solutions, see [Lambda@Edge Example Functions](lambda-examples.md)\.

## Serve private content by using Lambda@Edge customizations<a name="IntroductionUseCasesPrivateContentAtTheEdge"></a>

Using Lambda@Edge can help you configure your CloudFront distribution to serve private content from your own custom origin, as an option to using signed URLs or signed cookies\.

You can use several techniques to restrict access to your origin exclusively to CloudFront, including using whitelisting CloudFront IPs in your firewall and using a custom header to carry a shared secret\.

For more information and step\-by\-step instructions, including sample code, see [ Serving Private Content Using Amazon CloudFront & AWS Lambda@Edge](https://aws.amazon.com/blogs/networking-and-content-delivery/serving-private-content-using-amazon-cloudfront-aws-lambdaedge/)\.