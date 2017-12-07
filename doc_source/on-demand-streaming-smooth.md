# Configuring On\-Demand Smooth Streaming<a name="on-demand-streaming-smooth"></a>

You can use CloudFront for on\-demand streaming of media files that you've transcoded into the Microsoft Smooth Streaming format\. To distribute Smooth Streaming content on demand, you have two options:

+ As the origin for your distribution, specify a web server that can stream files that have been transcoded into Microsoft Smooth Streaming format\.

+ Enable Smooth Streaming in a CloudFront distribution\. Smooth Streaming is a property of cache behaviors, which means that you can use one distribution to distribute Smooth Streaming media files as well as other content\. 

If you enable Smooth Streaming, note the following:

+ You can still distribute other content using the same cache behavior if the content matches the value of **Path Pattern** for that cache behavior\.

+ CloudFront can use either an Amazon S3 bucket or a custom origin for Smooth Streaming media files\. However, CloudFront cannot use a Microsoft IIS Server as an origin if the server is configured for Smooth Streaming\. 

+ You cannot invalidate media files in the Smooth Streaming format\. If you want to update files before they expire, you must rename them\. For more information, see [Adding, Removing, or Replacing Objects in a Distribution](AddRemoveReplaceObjects.md)\.

For information about Smooth Streaming clients, see [Smooth Streaming Primer](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-primer) on the Microsoft website\.

To use CloudFront to stream media files that have been encoded in the Microsoft Smooth Streaming format without using a web server that can stream files in Smooth Streaming format, perform the following tasks:

1. Transcode your media files into Smooth Streaming fragmented\-MP4 format\. For a list of applications that can transcode into Smooth Streaming format, see [Smooth Streaming Primer](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-primer) on the Microsoft website\.

1. Do one of the following:

   + **If you're using the CloudFront console:** When you create a web distribution, enable Smooth Streaming in the default cache behavior\. Alternatively, you can enable Smooth Streaming in the default cache behavior and/or one or more custom cache behaviors in an existing CloudFront web distribution\. 

   + **If you're using the CloudFront API:** Add the `SmoothStreaming` element to the `DistributionConfig` complex type for the default cache behavior and/or one or more custom cache behaviors\. 

1. Upload the files in your Smooth Streaming presentations to the applicable origin\.

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