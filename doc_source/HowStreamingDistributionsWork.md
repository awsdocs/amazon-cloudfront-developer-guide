# How RTMP Distributions Work<a name="HowStreamingDistributionsWork"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

To stream media files using CloudFront, you provide two types of files to your end users:
+ Your media files
+ A media player, for example, JW Player, Flowplayer, or Adobe Flash

End users view your media files using the media player that you provide for them; they do not use the media player \(if any\) that is already installed on their computer or other device\.

When an end user streams your media file, the media player begins to play the content of the file while the file is still being downloaded from CloudFront\. The media file is not stored locally on the end user's system\. 

To use CloudFront to serve both the media player and the media files, you need two types of distributions: a web distribution for the media player, and an RTMP distribution for the media files\. Web distributions serve files over HTTP, while RTMP distributions stream media files over RTMP \(or a variant of RTMP\)\.

The following example assumes that your media files and your media player are stored in different buckets in Amazon S3, but that isn't required—you can store media files and your media player in the same Amazon S3 bucket\. You can also make the media player available to end users in other ways, for example, using CloudFront and a custom origin\. However, the media files must use an Amazon S3 bucket as the origin\.

In the following diagram, your site serves a cached copy of the media player to each end user through the `d1234.cloudfront.net` domain\. The media player then accesses cached copies of your media files through the `s5678.cloudfront.net` domain\.

![\[Setup for streaming\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

1. Your media player bucket holds the media player and is the origin server for a regular HTTP distribution\. In this example, the domain name for the distribution is `d1234.cloudfront.net`\. \(The `d` in `d1234.cloudfront.net` indicates that this is a web distribution\.\)

1. Your streaming media bucket holds your media files and is the origin server for an RTMP distribution\. In this example, the domain name for the distribution is `s5678.cloudfront.net`\. \(The `s` in `s5678.cloudfront.net` indicates that this is an RTMP distribution\.\)

When you configure CloudFront to distribute media files, CloudFront uses Adobe Flash Media Server as the streaming server and streams your media files using Adobe's Real\-Time Messaging Protocol \(RTMP\)\. CloudFront accepts RTMP requests over port 1935 and port 80\.

CloudFront supports the following variants of the RTMP protocol:
+ **RTMP** – Adobe's Real\-Time Message Protocol
+ **RTMPT** – Adobe streaming tunneled over HTTP
+ **RTMPE** – Adobe encrypted
+ **RTMPTE** – Adobe encrypted tunneled over HTTP