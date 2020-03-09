# Delivering Video on Demand \(VOD\) with CloudFront<a name="on-demand-video"></a>

To deliver video on demand \(VOD\) streaming with CloudFront, use the following services:
+ Amazon S3 to store the content in its original format and to store the transcoded video\.
+ An encoder \(such as AWS Elemental MediaConvert\) to transcode the video into streaming formats\.
+ CloudFront to deliver the transcoded video to viewers\. For Microsoft Smooth Streaming, see [Configuring Video on Demand for Microsoft Smooth Streaming](#on-demand-streaming-smooth)\.

**To create a VOD solution with CloudFront**

1. Upload your content to an Amazon S3 bucket\. To learn more about working with Amazon S3, see [the Amazon Simple Storage Service Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/)\.

1. Transcode your content by using a MediaConvert job\. The job converts your video into the formats required by the players that your viewers use\. You can also use the job to create assets that vary in resolution and bitrate\. These assets are used for adaptive bitrate \(ABR\) streaming, which adjusts the viewing quality depending on the viewer’s available bandwidth\. MediaConvert stores the transcoded video in an S3 bucket\.

1. Deliver your converted content by using a CloudFront distribution\. Viewers can watch the content on any device, at any time\. 

**Tip**  
You can explore how to use an AWS CloudFormation template to deploy a VOD AWS solution together with all the associated components\. To see the steps for using the template, see [Automated Deployment](https://docs.aws.amazon.com/solutions/latest/video-on-demand/deployment.html) in the *Video on Demand on AWS* guide\.

## Configuring Video on Demand for Microsoft Smooth Streaming<a name="on-demand-streaming-smooth"></a>

You have the following options for using CloudFront to distribute video on demand \(VOD\) content that you’ve transcoded into the Microsoft Smooth Streaming format:
+ Specify a web server that runs Microsoft IIS and supports Smooth Streaming as the origin for your distribution\.
+ Enable Smooth Streaming in the cache behaviors of a CloudFront distribution\. Because you can use multiple cache behaviors in a distribution, you can use one distribution for Smooth Streaming media files as well as other content\. 

**Important**  
If you specify a web server running Microsoft IIS as your origin, do *not* enable Smooth Streaming in the cache behaviors of your CloudFront distribution\. CloudFront can’t use a Microsoft IIS server as an origin if you enable Smooth Streaming as a cache behavior\.

If you enable Smooth Streaming in a cache behavior \(that is, you do not have a server that is running Microsoft IIS\), note the following:
+ You can still distribute other content using the same cache behavior if the content matches the value of **Path Pattern** for that cache behavior\.
+ CloudFront can use either an Amazon S3 bucket or a custom origin for Smooth Streaming media files\. CloudFront cannot use a Microsoft IIS Server as an origin if you enable Smooth Streaming for the cache behavior\. 
+ You cannot invalidate media files in the Smooth Streaming format\. If you want to update files before they expire, you must rename them\. For more information, see [Adding, Removing, or Replacing Content That CloudFront Distributes](AddRemoveReplaceObjects.md)\.

For information about Smooth Streaming clients, see [Smooth Streaming Primer](https://docs.microsoft.com/en-us/iis/media/smooth-streaming/smooth-streaming-primer) on the Microsoft documentation website\.

**To use CloudFront to distribute Smooth Streaming files when a Microsoft IIS web server isn’t the origin**

1. Transcode your media files into Smooth Streaming fragmented MP4 format\.

1. Do one of the following:
   + **If you’re using the CloudFront console:** When you create or update a web distribution, enable Smooth Streaming in one or more of the distribution’s cache behaviors\.
   + **If you’re using the CloudFront API:** Add the `SmoothStreaming` element to the `DistributionConfig` complex type for one or more of the distribution’s cache behaviors\.

1. Upload the Smooth Streaming files to your origin\.

1. Create either a `clientaccesspolicy.xml` or a `crossdomainpolicy.xml` file, and add it to a location that is accessible at the root of your distribution, for example, `https://d111111abcdef8.cloudfront.net/clientaccesspolicy.xml`\. The following is an example policy:

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

   For more information, see [Making a Service Available Across Domain Boundaries](https://docs.microsoft.com/en-us/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc197955(v=vs.95)) on the Microsoft Developer Network website\. 

1. For links in your application \(for example, a media player\), specify the URL for the media file in the following format:

   `https://d111111abcdef8.cloudfront.net/video/presentation.ism/Manifest`