# On\-Demand and Live Streaming Video with CloudFront<a name="on-demand-streaming-video"></a>

You can use CloudFront to deliver on\-demand video or live streaming video using any HTTP origin\. One way you can set up video workflows in the cloud is by using CloudFront together with [AWS Media Services](https://aws.amazon.com/media-services/)\. 

**Topics**
+ [About Streaming Video: On\-Demand and Live Streaming](#StreamingVideo)
+ [Delivering On\-Demand Video with CloudFront](on-demand-video.md)
+ [Delivering Live Streaming Video with CloudFront and AWS Media Services](live-streaming.md)
+ [Working with RTMP Distributions](distribution-rtmp.md)

## About Streaming Video: On\-Demand and Live Streaming<a name="StreamingVideo"></a>

You must use an encoder to package video content before you can set up CloudFront to distribute it\. These packages contain your audio, video, and captions content in small files called segments, each with a few seconds of content\. They also contain manifest files, which players use to determine which segment to download and when to play it, in a specific order\. Common formats for packages are MPEG DASH, Apple HLS, Microsoft Smooth Streaming, and CMAF\.

On\-demand video streaming  
For on\-demand video streaming, your video content is stored on a server and viewers can watch it at any time\. To make an asset that viewers can stream, use a transcoder, such as [AWS Elemental MediaConvert](https://docs.aws.amazon.com/mediaconvert/latest/ug/getting-started.html), to convert your file into a streaming package\.   
After your video is converted into the right formats, you can store it on a server or in an S3 bucket, and then deliver it with CloudFront as viewers request it\.

Live video streaming  
For live video streaming, you can either stream a live event or set up a 24x7 live channel\. To create live outputs for broadcast and streaming delivery, use an encoder such as MediaLive\.   
Examples of live events are live broadcasting and content aggregators streaming sports tournaments, awards ceremonies, and keynote addresses\.  
A 24x7 live channel might be set up for a studio, broadcaster, or paid TV service operator who wants to package and deliver a live linear channel over the internet, sending content directly to an audience without a third\-party distribution platform\.  
After your video is encoded, you can send it to MediaStore or convert it into different delivery formats by using AWS Elemental MediaPackage\. Using any of these origins, you can set up a CloudFront distribution to deliver the content\. For specific steps and guidance for creating distributions that work together with these services, see [Serving Video Using AWS Elemental MediaStore as the Origin](live-streaming.md#video-streaming-mediastore) and [Serving Live Video Formatted with AWS Elemental MediaPackage](live-streaming.md#live-streaming-with-mediapackage)\.

There are also other companies that provide tools that you can use for streaming video with CloudFront, such as Wowza and Unified Streaming\. For more information about using Wowza with CloudFront, see [ How to bring your Wowza Streaming Engine license to CloudFront live HTTP streaming](https://www.wowza.com/docs/how-to-bring-your-wowza-streaming-engine-license-to-cloudfront-live-http-streaming) on the Wowza website\. For information about using Unified Streaming and CloudFront for on\-demand streaming, see [Amazon Instances](http://docs.unified-streaming.com/tutorials/cloud/amazon/amazon-instances.html) on the Unified Streaming website\.