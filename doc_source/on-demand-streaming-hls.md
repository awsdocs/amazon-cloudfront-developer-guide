# Configuring On\-Demand Apple HTTP Live Streaming \(HLS\)<a name="on-demand-streaming-hls"></a>

To use CloudFront to stream media files in Apple HTTP Live Streaming \(HLS\) format, perform the following tasks:

1. Transcode your media files into HLS format\. You can use any transcoding application that supports HLS format, for example, [Amazon Elastic Transcoder](http://aws.amazon.com/elastictranscoder/)\.

1. Create a CloudFront web distribution\. No special settings are required\.

1. Upload your files to the origin that you specified when you created your distribution\. 

1. For links in your application \(for example, a media player\), specify the master playlist in the same format that you use for other objects that you're distributing using CloudFront\. For more information, see [Format of URLs for Objects](LinkFormat.md)\.