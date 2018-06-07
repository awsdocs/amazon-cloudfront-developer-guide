# Delivering On\-Demand Video with CloudFront and AWS Media Services<a name="on-demand-video"></a>

To use AWS products to deliver on\-demand video streaming, you can use Amazon S3 to store the content in its original format, [AWS Elemental MediaConvert](http://docs.aws.amazon.com/mediaconvert/latest/ug/getting-started.html) to transcode the video files into streaming formats, and CloudFront to deliver the video to viewers\.

To create the solution, follow these steps:
+ **Step 1:** Upload your content to an Amazon S3 bucket\. To learn more about working with S3, see [the Amazon Simple Storage Service Developer Guide](http://docs.aws.amazon.com/AmazonS3/latest/dev/)\.
+ **Step 2:**Use AWS Elemental MediaConvert to convert your video into the formats required by the players your viewers will be using\. You can also create assets of that vary in size and quality for adaptive bitrate streaming, which adjusts the viewing quality depending on the viewer's available bandwidth\. AWS Elemental MediaConvert takes content that you upload to an S3 bucket, transcodes it, and stores the result in another S3 bucket\. 
+ **Step 3:** Deliver the converted content by using a CloudFront distribution, so viewers can watch it on any device, whenever they like\. First, you put all segments and manifest files together in a single directory\. Then you set up a CloudFront distribution to serve the content \(see [Configuring On\-Demand Video with AWS Elemental MediaStore](#on-demand-streaming-mediastore)\)\. 

**Tip**  
To learn more about best practices when you implement a video on\-demand workflow with AWS Cloud services, see [Video on Demand on AWS](aws.amazon.comanswers/media-entertainment/video-on-demand-on-aws/)\.  
You can also explore how to use an AWS CloudFormation template to deploy a video\-on\-demand AWS solution together with all the associated components\. To see the steps for using the template, see [Video on Demand Automated Deployment](aws.amazon.comsolutions/latest/video-on-demand/deployment.html)\.

## Configuring On\-Demand Video with AWS Elemental MediaStore<a name="on-demand-streaming-mediastore"></a>

If you store on\-demand videos in [AWS Elemental MediaStore](http://docs.aws.amazon.com/mediastore/latest/ug/getting-started.html), you can create a CloudFront distribution to serve the content\.

To get started, you grant CloudFront access to your AWS Elemental MediaStore container\. Then you create a CloudFront distribution and configure it to work with AWS Elemental MediaStore\.

1. Follow the procedure at [ Allowing Amazon CloudFront to Access Your AWS Elemental MediaStore Container](http://docs.aws.amazon.com/mediastore/latest/ug/cdns-allowing-cloudfront-to-access-mediastore.html), and then return to these steps to create your distribution\.

1. Create a distribution with the following settings:  
**Origin Domain Name**  
The data endpoint that is assigned to your AWS Elemental MediaStore container\. From the dropdown list, choose the AWS Elemental MediaStore container for your live video\. The format of an AWS Elemental MediaStore origin is Container\-OriginEndpointURL\. For example, mymediastore\.data\.mediastore\.us\-east\-1\.amazonaws\.com\. For more information, see [Origin Domain Name](distribution-web-values-specify.md#DownloadDistValuesDomainName)\.  
**Origin Path**  
The folder structure in the AWS Elemental MediaStore container where your objects are stored\. For more information, see [Origin Path](distribution-web-values-specify.md#DownloadDistValuesOriginPath)\.  
**Origin Custom Headers**  
Add header names and values if you want CloudFront to include custom headers when it forwards requests to your origin\.  
**Object Caching**  
If the transcoder that you use can't set cache controls on all objects, choose **Customize**\. If your transcoder can set cache controls on all objects, choose **Origin Cache Headers**\.   
**Minimum TTL, Maximum TTL, and Default TTL**  
Set as appropriate for your caching needs and segment durations\.  
**Error Caching Minimum TTL**  
Set to 5 seconds or less, to help prevent serving stale content\.

   For the other settings, you can set specific values based on other technical requirements or the needs of your business\. For a list of all the options for web distributions and information about setting them, see [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md)\.

1. After you create your distribution and it’s been provisioned, edit the cache behavior to set up cross\-origin resource sharing \(CORS\) for your origin:

   1. Select the distribution, and then choose **Distribution Settings**\.

   1. Choose **Behaviors**, select your origin, and then choose **Edit**\.

   1. Under **Cache Based on Selected Request Headers**, choose **Whitelist**, and then, under **Whitelist Headers**, select **Origin**\.

   To learn more about CORS, see *Configuring CloudFront to Respect Cross\-Origin Resource Sharing \(CORS\) Settings* in [Configuring CloudFront to Cache Objects Based on Request Headers](header-caching.md)\.

1. For links in your application \(for example, a media player\), specify the name of the media file in the same format that you use for other objects that you're distributing using CloudFront\.