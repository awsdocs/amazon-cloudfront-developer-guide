# Configuring On\-Demand Progressive Downloads<a name="on-demand-streaming-progressive"></a>

To use CloudFront to distribute media files using progressive download, perform the following tasks:

1. Transcode your media files, if applicable\. 

1. Create a CloudFront web distribution\. No special settings are required\.

1. Upload your files to the origin that you specified when you created your distribution\. 

1. For links in your application \(for example, a media player\), specify the name of the media file in the same format that you use for other objects that you're distributing using CloudFront\. For more information, see [Format of URLs for Objects](LinkFormat.md)\.