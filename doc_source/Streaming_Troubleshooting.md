# Troubleshooting RTMP Distributions<a name="Streaming_Troubleshooting"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

If you're having trouble getting your media files to play, check the following items\.


****  

| Item to Check | Description | 
| --- | --- | 
|  Separate distributions for the media player files and media files  |  The media player must be served by a regular HTTP distribution \(for example, domain name d111111abcdef8\.cloudfront\.net\), and media files must be served by an RTMP distribution \(for example, domain name s5c39gqb8ow64r\.cloudfront\.net\)\. Make sure you're not using the same distribution for both\.   | 
|  `/cfx/st` in the file path  |  Confirm that the path for the file includes `/cfx/st`\. You don't need to include `/cfx/st` in the path to the object in the Amazon S3 bucket\. For more information, see [Configuring the Media Player](Streaming_URLs.md)\.   | 
|  File names in the file path  |  Some media players require that you include the file name extension \(for example, `mp4:`\) before the file name in the file path\. Some media players also require that you exclude the file name extension \(for example, `.mp4`\) from the file path\. For more information, see [MPEG Files](Streaming_URLs.md#Streaming_MP4_URLs)\. The names of the media files in your Amazon S3 bucket must always include the applicable file name extension\.   | 
|  Port 1935 on your firewall  |  Adobe Flash Media Server uses port 1935 for RTMP\. Make sure your firewall has this port open\. If it doesn't, the typical message returned is "Unable to play video\." You can also switch to RTMPT to tunnel over HTTP using port 80\.  | 
|  Adobe Flash Player messaging  |  By default, the Adobe Flash Player won't display a message if the video file it's trying to play is missing\. Instead, it waits for the file to show up\. You might want to change this behavior to give your end users a better experience\.  To instead have the player send a message if the video is missing, use `play("vid",0,-1)` instead of `play("vid")`\.   | 