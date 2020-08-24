# Configuring the Media Player<a name="Streaming_URLs"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

To play a media file, you must configure the media player with the correct path to the file\. How you configure the media depends on which media player you're using and how you're using it\.

When you configure the media player, the path you specify to the media file must contain the characters `cfx/st` immediately after the domain name, for example:

`rtmp://s5c39gqb8ow64r.cloudfront.net/cfx/st/mediafile.flv`\. 

**Note**  
CloudFront follows Adobe's FMS naming requirements\. Different players have their own rules about how to specify streams\. The example above is for JW Player\. Check your player's documentation\. For example, Adobe's Flash Media Server does not allow the `.flv` extension to be present on the play path\. Many players remove the \.`flv` extension for you\. 

Your media player might ask for the path separate from the file name\. For example, with JW Player, you specify a `streamer` and `file` variable:
+ **streamer** – `rtmp://s5c39gqb8ow64r.cloudfront.net/cfx/st` \(with no trailing slash\)
+ **file** – `mediafile.flv` 

If you've stored the media files in a directory in your bucket \(for example, `videos/mediafile.flv`\), then the variables for JW Player would be:
+ **streamer** – `rtmp://s5c39gqb8ow64r.cloudfront.net/cfx/st` \(with no trailing slash\)
+  **file** – `videos/mediafile.flv` 

For more information about JW Player, go to the [JW Player website](http://www.jwplayer.com)\.

## MPEG Files<a name="Streaming_MP4_URLs"></a>

To serve MP3 audio files or H\.264/MPEG\-4 video files, you might need to prefix the file name with `mp3:` or `mp4:`\. Some media players can be configured to add the prefix automatically\. The media player might also require you to specify the file name without the file extension \(for example, `magicvideo` instead of `magicvideo.mp4`\)\. 