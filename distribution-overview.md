# Overview of Web and RTMP Distributions<a name="distribution-overview"></a>

When you want to use CloudFront to distribute your content, you create a distribution and specify configuration settings such as:
+ Your origin, which is the Amazon S3 bucket or HTTP server from which CloudFront gets the files that it distributes\. You can specify any combination of up to 25 Amazon S3 buckets and/or HTTP servers as your origins\. 
+ Whether you want the files to be available to everyone or you want to restrict access to selected users\.
+ Whether you want CloudFront to require users to use HTTPS to access your content\.
+ Whether you want CloudFront to forward cookies and/or query strings to your origin\.
+ Whether you want CloudFront to prevent users in selected countries from accessing your content\.
+ Whether you want CloudFront to create access logs\.

For the current limit on the number of distributions that you can create for each AWS account, see [General Limits on Web Distributions](cloudfront-limits.md#limits-web-distributions) and [Limits on RTMP Distributions](cloudfront-limits.md#limits-rtmp-distributions)\.

The number of files that you can serve per distribution is unlimited\.

## Web Distributions<a name="distribution-overview-web"></a>

You can use web distributions to serve the following content over HTTP or HTTPS:
+ Static and dynamic download content, for example, \.html, \.css, \.js, and image files, using HTTP or HTTPS\.
+ Multimedia content on demand using progressive download and Apple HTTP Live Streaming \(HLS\)\.  For more information, see the applicable topic in [Working with Web Distributions](distribution-web.md)\.

  You can't serve Adobe Flash multimedia content over HTTP or HTTPS, but you can serve it using a CloudFront RTMP distribution\. See [RTMP Distributions](#distribution-overview-rtmp) below\.
+ A live event, such as a meeting, conference, or concert, in real time\. For live streaming, you create the distribution automatically by using an AWS CloudFormation stack\. For more information, see the applicable live\-streaming tutorial in [CloudFront Streaming Tutorials](Tutorials.md)\.

For web distributions, your origin can be either an Amazon S3 bucket or an HTTP server, for example, a web server\. For more information about how web distributions work, including the values that you specify when you create a web distribution, see [Working with Web Distributions](distribution-web.md)\. For information about creating a web distribution, see [Task List for Creating a Web Distribution](distribution-web-creating.md)\.

## RTMP Distributions<a name="distribution-overview-rtmp"></a>

RTMP distributions stream media files using Adobe Media Server and the Adobe Real\-Time Messaging Protocol \(RTMP\)\. An RTMP distribution must use an Amazon S3 bucket as the origin\. 

For information about the values you specify when you create an RTMP distribution, see [Working with RTMP Distributions](distribution-rtmp.md)\. For information about creating an RTMP distribution, see [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)\. 