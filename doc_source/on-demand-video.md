# Delivering On\-Demand Video with CloudFront<a name="on-demand-video"></a>

To deliver on\-demand video streaming, you can use Amazon S3 to store the content in its original format, use a transcoder, such as [AWS Elemental MediaConvert](https://docs.aws.amazon.com/mediaconvert/latest/ug/getting-started.html), to transcode the video into streaming formats, store the transcoded video in an S3 bucket, and then use CloudFront to deliver the video to viewers\. If you want to use Microsoft Smooth Streaming, see [Configuring On\-Demand Microsoft Smooth Streaming](#on-demand-streaming-smooth)\.

To create the solution, follow these steps:
+ **Step 1:** Upload your content to an Amazon S3 bucket\. To learn more about working with S3, see [the Amazon Simple Storage Service Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/)\.
+ **Step 2:** Use MediaConvert to convert your video into the formats required by the players your viewers will be using\. You can also create assets that vary in resolution and bitrate for adaptive bitrate streaming, which adjusts the viewing quality depending on the viewer's available bandwidth\. MediaConvert outputs the transcoded video to an S3 bucket\. 
+ **Step 3:** Deliver the converted content by using a CloudFront distribution, so viewers can watch it on any device, whenever they like\. 

**Tip**  
You can explore how to use an AWS CloudFormation template to deploy a video\-on\-demand AWS solution together with all the associated components\. To see the steps for using the template, see [Video on Demand Automated Deployment](https://docs.aws.amazon.com/solutions/latest/video-on-demand/deployment.html)\.

**Topics**
+ [Configuring On\-Demand Microsoft Smooth Streaming](#on-demand-streaming-smooth)

## Configuring On\-Demand Microsoft Smooth Streaming<a name="on-demand-streaming-smooth"></a>

You can use CloudFront for providing on\-demand video by using files that you've transcoded into the Microsoft Smooth Streaming format\. To distribute Smooth Streaming content on demand, you have two options:
+ As the origin for your distribution, specify a web server running Microsoft IIS that can stream files that have been transcoded into Microsoft Smooth Streaming format\.
+ Enable Smooth Streaming in a CloudFront distribution\. Smooth Streaming is a property of cache behaviors, which means that you can use one distribution to distribute Smooth Streaming media files as well as other content\. 

**Important**  
If your origin is a web server running Microsoft IIS, do not enable Smooth Streaming when you create your CloudFront distribution\. CloudFront can't use a Microsoft IIS server as an origin if you enable Smooth Streaming\.

If you enable Smooth Streaming for an origin server \(that is, you do not have a server that is running Microsoft IIS\), note the following:
+ You can still distribute other content using the same cache behavior if the content matches the value of **Path Pattern** for that cache behavior\.
+ CloudFront can use either an Amazon S3 bucket or a custom origin for Smooth Streaming media files\. However, CloudFront cannot use a Microsoft IIS Server as an origin if the server is configured for Smooth Streaming\. 
+ You cannot invalidate media files in the Smooth Streaming format\. If you want to update files before they expire, you must rename them\. For more information, see [Adding, Removing, or Replacing Content That CloudFront Distributes](AddRemoveReplaceObjects.md)\.

For information about Smooth Streaming clients, see [Smooth Streaming Primer](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-primer) on the Microsoft website\.

To use CloudFront to stream media files that have been encoded in the Microsoft Smooth Streaming format without using a Microsoft IIS web server that can stream files in Smooth Streaming format, do the following:

1. Transcode your media files into Smooth Streaming fragmented\-MP4 format\.

1. Do one of the following:
   + **If you're using the CloudFront console:** When you create a web distribution, enable Smooth Streaming in the default cache behavior\. Alternatively, you can enable Smooth Streaming in the default cache behavior and/or one or more custom cache behaviors in an existing CloudFront web distribution\. 
   + **If you're using the CloudFront API:** Add the `SmoothStreaming` element to the `DistributionConfig` complex type for the default cache behavior and/or one or more custom cache behaviors\. 

1. Upload the files in your Smooth Streaming presentations to your origin\.

1. Create either a `clientaccesspolicy.xml` or a `crossdomainpolicy.xml` file, and add it to a location that is accessible at the root of your distribution, for example, `http://d111111abcdef8.cloudfront.net/clientaccesspolicy.xml`\. The following is an example policy:

   ```
   <?xml version="1.0" encoding="utf-8"?>
   <access-policy>
   <cross-domain-access>
   <policy>
   <allow-from http-request-headers="*">
   <domain uri="*"/>
   </allow-from>
   <grant-to>
   <resource path="/" include-subpaths="true"/>
   </grant-to>
   </policy>
   </cross-domain-access>
   </access-policy>
   ```

   For more information, see [Making a Service Available Across Domain Boundaries](http://msdn.microsoft.com/en-us/library/cc197955(v=vs.95).aspx) on the Microsoft Developer Network website\. 

1. For links in your application, specify the client manifest in the following format:

   `http://d111111abcdef8.cloudfront.net/video/presentation.ism/Manifest`