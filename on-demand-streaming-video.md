# Configuring Video Streaming Web Distributions<a name="on-demand-streaming-video"></a>

You can use CloudFront to deliver on\-demand video or live streaming video using any HTTP origin\. One way you can set up video workflows in the cloud is by using CloudFront together with [AWS Elemental Media Services](aws.amazon.commedia-services/)\. 

**For on\-demand video streaming**, your video content is stored on a server and viewers can watch it at any time\. To make an asset that viewers can stream, use a transcoder, such as [AWS Elemental MediaConvert](http://docs.aws.amazon.com/mediaconvert/latest/ug/getting-started.html), to convert your file into a streaming package\. Common formats for packages are DASH, Apple HLS, and Microsoft \(MS\) Smooth\.

These packages contain your audio, video, and captions content in small files called segments, each with a few seconds of content\. They also contain manifest files, which players use to determine which segment to download and when to play it, in a specific order\.

After your video is converted into the right formats, you can deliver it with CloudFront as viewers request it\.

**For live video streaming**, you can either stream a live event or set up a 24x7 live channel delivery\. 

Examples of live events are live broadcasting and content aggregators streaming sports tournaments, awards ceremonies, and keynote addresses\.

A 24x7 live channel might be set up for a studio, broadcaster, or paid TV service operator who wants to package and deliver a live linear channel over the internet, sending content directly to an audience without a third\-party distribution platform\.

There are also other companies that provide tools that you can use for delivering or viewing live video with CloudFront, such as Wowza and JW Player\. For more information about using Wowza with CloudFront, see [ How to bring your Wowza Streaming Engine license to CloudFront live HTTP streaming](https://www.wowza.com/docs/how-to-bring-your-wowza-streaming-engine-license-to-cloudfront-live-http-streaming) on the Wowza website\. For a tutorial on using JW Player with CloudFront, see [On\-Demand Video Streaming Using CloudFront and JW Player](TutorialStreamingJWPlayer.md)\.

**Topics**
+ [Delivering On\-Demand Video with CloudFront and AWS Media Services](on-demand-video.md)
+ [Delivering Live Streaming Video with CloudFront and AWS Media Services](live-streaming.md)
+ [Configuring On\-Demand Microsoft Smooth Streaming](on-demand-streaming-smooth.md)