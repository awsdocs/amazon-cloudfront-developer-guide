# CloudFront Use Cases<a name="IntroductionUseCases"></a>

Using CloudFront can help you accomplish a variety of goals\. This section lists just a few, to give you an idea of the possibilities\.

**Topics**
+ [On\-Demand and Live Streaming](#IntroductionUseCasesStreaming)
+ [Field Level Encryption](#IntroductionUseCasesFieldLevelEncryption)
+ [Customize at the Edge](#IntroductionUseCasesProgrammableCDN)

## On\-Demand and Live Streaming<a name="IntroductionUseCasesStreaming"></a>

CloudFront offers several options for streaming your media to global viewers—both pre\-recorded files and live events\.
+ For on\-demand streaming, you can use CloudFront to stream in Microsoft Smooth, HLS, HDS, or MPEG\-DASH formats, to any device\.
+ For broadcasting a live stream, you can cache media fragments at the edge, so that multiple request for the manifest file that delivers the fragments in the right order can be combined, to reduce the load on your origin server\.

For more information about how to deliver streaming content with CloudFront, see [About Streaming Video: On\-Demand and Live Streaming](on-demand-streaming-video.md#StreamingVideo)\.

## Field Level Encryption<a name="IntroductionUseCasesFieldLevelEncryption"></a>

When you configure HTTPS with CloudFront, you already have secure end\-to\-end connections to origin servers\. When you add field\-level encryption, you can protect specific data throughout system processing in addition to HTTPS security, so that only certain applications at your origin can see the data\.

To set up field\-level encryption, you add a public key to CloudFront, and then specify the set of fields that you want to be encrypted with the key\. For more information, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\.

## Customize at the Edge<a name="IntroductionUseCasesProgrammableCDN"></a>

Running serverless code at the edge opens up a world of possibilities for customizing the content and experience for viewers, at reduced latency\. You can, for example, return a custom error message when your origin server is down for maintenance, so viewers don't get a generic HTTP error message\. Or you can use a function to help authorize users and control access to your content, before CloudFront forwards a request to your origin\.

Using Lambda@Edge with CloudFront enables a variety of ways to customize the content that CloudFront delivers\. To learn more about Lambda@Edge and how to create and deploy functions with CloudFront, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\. To see a number of code samples that you can customize for your own solutions, see [Lambda@Edge Example Functions](lambda-examples.md)\.