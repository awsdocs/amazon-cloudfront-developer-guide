# Restricting Access Using Crossdomain\.xml<a name="Streaming_CrossDomain"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

The Adobe Flash Media Server `crossdomain.xml` file specifies which domains can access media files in a particular domain\. CloudFront supplies a default file that allows all domains to access the media files in your RTMP distribution, and you cannot change this behavior\. If you include a more restrictive `crossdomain.xml` file in your Amazon S3 bucket, CloudFront ignores it\.