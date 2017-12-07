# On\-Demand Video Streaming Using CloudFront and JW Player<a name="TutorialStreamingJWPlayer"></a>

When you stream media files using CloudFront, you provide both your media file and the media player with which you want end users to play the media file\. To use the JW Player media player to stream media files with CloudFront, perform the procedures in the following topics:

1. [Uploading Media and JW Player Files to an Amazon S3 Bucket](#StreamingJWPlayerUploadFiles)

1. [Creating CloudFront Web and RTMP Distributions](#StreamingJWPlayerCreateDist)

1. [Embedding Video in a Web Page](#StreamingJWPlayerEmbedVideo)

1. [Uploading the HTML File and Playing the Video](#StreamingJWPlayerUploadHTML)

This tutorial is based on the free edition of JW Player version 7\.3\. For more information about JW Player, go to the [JW Player](https://www.jwplayer.com/products/jwplayer/) website\. 

For more information about streaming media using CloudFront, see [Working with RTMP Distributions](distribution-rtmp.md)\.

## Uploading Media and JW Player Files to an Amazon S3 Bucket<a name="StreamingJWPlayerUploadFiles"></a>

You can upload your media files and your media player files to the same Amazon S3 bucket or to separate buckets\. For this tutorial, you'll upload an \.mp4 or \.flv media file and the JW Player media player files to the same bucket\.

**To upload media and JW Player files to an Amazon S3 bucket**

1. If you don't already have the files for the JW Player media player, download the player \(JW Player 7\) from the [Downloads](https://dashboard.jwplayer.com/#/players/downloads) page on the JW Player website\. Then extract the contents of the \.zip file\.

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the Amazon S3 console, choose **Create Bucket**\.

1. In the **Create Bucket** dialog, enter a bucket name\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. Select a region for your bucket\. By default, Amazon S3 creates buckets in the US Standard region\. We recommend that you choose a region close to you to optimize latency, minimize costs, or to address regulatory requirements\.

1. Choose **Create**\.

1. Select your bucket in the **Buckets** pane, and choose **Upload**\.

1. On the **Upload \- Select Files** page, choose **Add Files**, and add the following files:

   + jwplayer\.flash\.swf

   + jwplayer\.js

   + Your \.mp4 or \.flv media file\.  
![\[Upload files to your Amazon S3 bucket.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

1. Grant public read permissions for the files that you added in the previous step\. 

   1. Choose **Set Details**\.

   1. On the **Set Details** page, choose **Set Permissions**\.

   1. On the **Set Permissions** page, choose **Make everything public**\.

1. Choose **Start Upload**\.

## Creating CloudFront Web and RTMP Distributions<a name="StreamingJWPlayerCreateDist"></a>

To configure CloudFront to stream a media file, you need a CloudFront web distribution for the JW Player files and an HTML file, and an RTMP distribution for the media file\. Perform the following two procedures to create a web distribution and an RTMP distribution\.

**To create a CloudFront web distribution for your JW Player files**

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the first page of the **Create Distribution Wizard**, in the **Web** section, choose **Get Started**\.  
![\[Select Web as the distribution type.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

1. On the second page of the wizard, choose in the **Origin Domain Name** field, and select the Amazon S3 bucket that you created in the procedure [To upload media and JW Player files to an Amazon S3 bucket](#StreamingJWPlayerUploadFilesProcedure)\. If you have a lot of Amazon S3 buckets, you can type the first few characters of the bucket name to filter the list\.

1. Accept the default values for the remaining fields, and choose **Create Distribution**\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. This should take less than 15 minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. The domain name also appears on the Distribution Settings page for a selected distribution\.\)

**To create a CloudFront RTMP distribution for your media file**

1. In the CloudFront console, choose **Create Distribution**\.

1. In the **Create Distribution Wizard**, in the **RTMP** section, choose **Get Started**\.

1. On the second page of the wizard, choose in the **Origin Domain Name** field, and select the Amazon S3 bucket that you created in the procedure [To upload media and JW Player files to an Amazon S3 bucket](#StreamingJWPlayerUploadFilesProcedure)\. If you have a lot of Amazon S3 buckets, you can type the first few characters of the bucket name to filter the list\.

1. Accept the default values for the remaining fields on the **Create Distribution** page, and choose **Create Distribution**\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. This should take less than 15 minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. The domain name also appears on the Distribution Settings page for a selected distribution\.

## Embedding Video in a Web Page<a name="StreamingJWPlayerEmbedVideo"></a>

The following example shows you how to embed a video in a web page using the web and RTMP distributions that you created in [Creating CloudFront Web and RTMP Distributions](#StreamingJWPlayerCreateDist)\.

Perform the following steps:

1. Sign in to the [JW Player](https://www.jwplayer.com/products/jwplayer/) website\. If you don't already have a JW Player account, create one\.

1. On the [Downloads](https://dashboard.jwplayer.com/#/players/downloads) page, get the license key for the player that you downloaded earlier in this tutorial\.

1. Copy the HTML code below, and paste it into a text editor\.

1. Review the comments in the HTML file, and replace the following placeholders with the applicable values:

   + WEB\-DISTRIBUTION\-DOMAIN\-NAME

   + RTMP\-DISTRIBUTION\-DOMAIN\-NAME

   + VIDEO\-FILE\-NAME

   + LICENSE\-KEY

1. Save the file with a \.html filename extension, for example, `jwplayer-example.html`\.

```
<HTML>
<HEAD>
<TITLE>Amazon CloudFront Streaming with JW Player 7</TITLE>	

<!-- Call the JW Player JavaScript file, jwplayer.js. 
Replace WEB-DISTRIBUTION-DOMAIN-NAME with the domain name of your 
CloudFront web distribution, for example, d1234.cloudfront.net 
(begins with "d"). This causes a browser to download the JW Player file 
before streaming begins.

Replace LICENSE-KEY with your personal license key from JW Player.
-->

<script type='text/javascript' src='WEB-DISTRIBUTION-NAME/jwplayer.js'></script>
<script type='text/javascript' src='WEB-DISTRIBUTION-NAME/jwplayer.flash.swf'></script>
<script>jwplayer.key="LICENSE-KEY";</script>

</HEAD>

<BODY>
<H1>This video is streamed by CloudFront and played by JW Player 7.</H1>

<!-- Replace RTMP-DISTRIBUTION-DOMAIN-NAME with the domain name of your 
RTMP distribution, for example, s5678.cloudfront.net (begins with "s").

Replace VIDEO-FILE-NAME with the name of your .mp4 or .flv video file, 
including the .mp4 or .flv filename extension. For example, if you uploaded 
my-vacation.mp4, enter my-vacation.mp4. You might need to prepend "mp4:" to the 
name of your video file, for example, mp4:my-vacation.mp4.

If the file is in a subdirectory, include the subdirectory name just before 
the file name, for example:

"rtmp://RTMP-DISTRIBUTION-DOMAIN-NAME/cfx/st/sample-directory/VIDEO-FILE-NAME"
-->


<div id="my-video"></div>
<script type="text/javascript">
jwplayer("my-video").setup({
 'file': 'rtmp://RTMP-DISTRIBUTION-DOMAIN-NAME/cfx/st/VIDEO-FILE-NAME',
 'width': '720',
 'height': '480',
 'primary': 'flash',
 'autostart': 'true'
});
</script>

</BODY>
</HTML>
```

## Uploading the HTML File and Playing the Video<a name="StreamingJWPlayerUploadHTML"></a>

To play the video using the HTML file that you created in [Embedding Video in a Web Page](#StreamingJWPlayerEmbedVideo), upload the file to your Amazon S3 bucket, and use the URL for your CloudFront distribution\. 

**To upload the HTML file and play the video**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Select your bucket, and choose **Upload**\.

1. On the **Upload \- Select Files** page, choose **Add Files**, and add your HTML file\.

1. Grant public read permissions for the HTML file that you added in the previous step\. 

   1. Choose **Set Details**\.

   1. On the **Set Details** page, choose **Set Permissions**\.

   1. On the **Set Permissions** page, choose **Make everything public**\.

1. Choose **Start Upload**\.

1. To play the video, enter the following URL in a web browser:

   `http://domain name of your CloudFront web distribution/your HTML file name`