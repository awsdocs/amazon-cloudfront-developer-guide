# Task List for Streaming Media Files Using RTMP<a name="distribution-rtmp-creating"></a>

This section summarizes the general process for configuring on\-demand streaming using the Adobe RTMP protocol for any media player\. If you're using JW Player for your media player, see [On\-Demand Video Streaming Using CloudFront and JW Player](TutorialStreamingJWPlayer.md)\.

The following task list summarizes the process for creating a web distribution\.<a name="create-streaming-distribution-task-list"></a>

**To Create an RTMP Distribution**

1. Create an Amazon S3 bucket for your media files\. If you are using a different Amazon S3 bucket for your media player, create an Amazon S3 bucket for the media player files, too\.

   The names of your buckets must be all lowercase and cannot contain spaces\.

1. Choose and configure a media player to play your media files\. For more information, refer to the documentation for the media player\.

1. Upload the files for your media player to the origin from which you want CloudFront to get the files\. If you are using an Amazon S3 bucket as the origin for the media player, make the files \(not the bucket\) publicly readable\.

1. Create a web distribution for your media player\. \(You can also use an existing distribution\.\) For more information, see [Task List for Creating a Web Distribution](distribution-web-creating.md)\.

1. Upload your media files to the Amazon S3 bucket that you created for the media files, and make the content \(not the bucket\) publicly readable\.
**Important**  
Media files in a Flash Video container must include the \.flv filename extension, or the media will not stream\.

   You can put media player files and media files in the same bucket\. 

1. Create an RTMP distribution for your media files:
   + For more information about creating a web distribution using the CloudFront console, see [Creating an RTMP Distribution Using the CloudFront Console](distribution-rtmp-creating-console.md)\.
   + For information about creating a web distribution using the CloudFront API, go to [POST Streaming Distribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/CreateStreamingDistribution.html) in the *Amazon CloudFront API Reference*\.

1. Configure your media player\. For more information, see [Configuring the Media Player](Streaming_URLs.md)\.

If you have trouble getting your content to play, see [Troubleshooting RTMP Distributions](Streaming_Troubleshooting.md)\.