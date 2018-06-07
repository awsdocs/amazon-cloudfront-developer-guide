# Delivering Live Streaming Video with CloudFront and AWS Media Services<a name="live-streaming"></a>

To use AWS Media Services with CloudFront to deliver or broadcast live content to a global audience, follow the guidance included in this section\.

[AWS Elemental MediaLive](http://docs.aws.amazon.com/medialive/latest/ug/getting-started.html) encodes live video streams in real time\. If you have a larger\-sized live video source—for example, video coming from a ground encoder like Elemental Live—you can compress it by using AWS Elemental MediaLive into smaller versions \("encodes"\) that are then distributed to your viewers\.

There are two main options for preparing and serving live streaming content:
+ **Convert your content into required formats, and then serve it:** You can use [AWS Elemental MediaPackage](aws.amazon.commediapackage/) to convert your video content from a single format to multiple formats, and then package the content for different device types\. AWS Elemental MediaPackage lets you implement video features for viewers such as start\-over, pause, rewind, and so on\. AWS Elemental MediaPackage can also protect your content from unauthorized copying by adding Digital Rights Management \(DRM\)\. For step\-by\-step instructions for using AWS Elemental MediaPackage with CloudFront, see [Serving Live Video Formatted with AWS Elemental MediaPackage](#live-streaming-with-mediapackage) in this topic\.
+ **Store and serve your content using scalable origin:** If your encoder already outputs content in the formats required by all of the devices that your viewers use, you can serve the content by using a highly\-scalable origin like an [AWS Elemental MediaStore](http://docs.aws.amazon.com/mediastore/latest/ug/getting-started.html) container\. AWS Elemental MediaStore delivers the content to CloudFront, which then routes it to the audience for the event\.

After you've set up your origin by using one of these options, you can distribute live streaming video to viewers by using CloudFront\.

**Tip**  
To learn more about best practices when you implement a live video streaming workflow with AWS Cloud services, see [Live Streaming Video](aws.amazon.comanswers/media-entertainment/live-streaming/)\.  
You can also learn about an AWS solution that automatically deploys services for building a highly\-available real\-time viewing experience\. To see the steps to automatically deploy this solution, see [Live Streaming Automated Deployment](aws.amazon.comsolutions/latest/live-streaming/deployment.html)\.

## Serving Live Video Formatted with AWS Elemental MediaPackage<a name="live-streaming-with-mediapackage"></a>

If you've used AWS Elemental MediaPackage to format a live stream for viewing, you can create a CloudFront distribution and configure cache behaviors to serve the live stream\. This topic assumes that you have already [ created a channel](http://docs.aws.amazon.com/mediapackage/latest/ug/channels-create.html) and [added endpoints](http://docs.aws.amazon.com/mediapackage/latest/ug/channels-add-endpoint.html) for your live video using AWS Elemental MediaPackage\.

To stream the video with CloudFront, create a web distribution for the channel, and then add each AWS Elemental MediaPackage endpoint as an origin for the distribution\. For each origin, you must configure cache behaviors to route the video content correctly\.

Follow these steps:

**Topics**
+ [Step 1: Create and Configure a CloudFront Distribution for Live Video](#live-streaming-with-mediapackage-create-dist)
+ [Step 2: Add the Other Endpoints as Origins for the Distribution](#live-streaming-with-mediapackage-add-endpoints)
+ [Step 3: Configure Cache Behaviors for all Endpoints](#live-streaming-with-mediapackage-create-cache-behavior)
+ [Step 4: Use CloudFront to Serve the Live Stream Channel](#live-streaming-with-mediapackage-serve-channel)

### Step 1: Create and Configure a CloudFront Distribution for Live Video<a name="live-streaming-with-mediapackage-create-dist"></a>

Complete the following procedure to set up a CloudFront distribution for the live video channel that you created with AWS Elemental MediaPackage<a name="live-streaming-with-mediapackage-create-dist-procedure"></a>

**To create a web distribution for your live video channel**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the **Select a delivery method** page, in the **Web** section, choose **Get Started**\.

1. Choose the settings for the distribution, including the following:  
**Origin Domain Name**  
The origin where your AWS Elemental MediaPackage live video channel and endpoints are\. From the dropdown list, choose the AWS Elemental MediaPackage channel for your live video\. The format of an AWS Elemental MediaPackage origin is ChannelID\-OriginEndpointID\-OriginEndpointURL\. You can map one channel to several origin endpoints\.  
If you created your channel using another AWS account, type the origin URL value into the field\. The origin must be an HTTPS URL\.  
For more information, see [Origin Domain Name](distribution-web-values-specify.md#DownloadDistValuesDomainName) in the [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md) topic\.  
**Origin Path**  
The folder structure in the AWS Elemental MediaPackage where your objects are stored\. When you choose a channel from the dropdown list, the path is filled in for you\.  
Note that if you chose to use a channel from another AWS account for **Origin Domain Name**, the **Origin Path** field is not filled in for you\. You must sign in to the other account and get the correct origin path so you that can enter it manually\.  
For more information about how an origin path works, see [Origin Path](distribution-web-values-specify.md#DownloadDistValuesOriginPath) in the [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md) topic\.  
For the other distribution settings, set specific values based on other technical requirements or the needs of your business\. For a list of all the options for web distributions and information about setting them, see [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md) topic\.

1. Specify the correct cache behavior settings for the channel that you chose for the origin\. You’ll add one or more additional origins later and edit cache behavior settings for them\.

1. Wait until the value of the Status column for your distribution has changed from **In Progress** to **Deployed**, indicating that CloudFront has created your distribution\.

### Step 2: Add the Other Endpoints as Origins for the Distribution<a name="live-streaming-with-mediapackage-add-endpoints"></a>

Repeat the steps here to add each endpoint\.<a name="live-streaming-with-mediapackage-add-endpoints-procedure"></a>

**To add other endpoints as origins**

1. On the CloudFront console, choose the distribution that you created for your channel, and then choose **Distribution Settings**\.

1. On the **Origins** tab, choose **Create Origin**\.

1. For **Origin Domain Name**, in the dropdown list, choose an AWS Elemental MediaPackage endpoint for your channel\. The **Origin Path** field will be automatically filled in for you\.

1. For the other settings, set the values based on other technical requirements or the needs of your business\. For more information, see [Origin Settings](distribution-web-values-specify.md#DownloadDistValuesOrigin) in the [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md) topic\.

1. Choose **Create**\.

### Step 3: Configure Cache Behaviors for all Endpoints<a name="live-streaming-with-mediapackage-create-cache-behavior"></a>

For each endpoint, you must configure cache behaviors to add path patterns that route requests correctly\. The path patterns that you specify depend on the video format that you’re serving\. The procedure in this topic includes the path pattern information to use for HLS, CMAF, DASH, and Microsoft Smooth formats\.

You typically set up two cache behaviors for each endpoint:
+ The parent manifest, which is the index to your files
+ The child manifest, which is the video content itself<a name="live-streaming-with-mediapackage-create-cache-behavior-procedure"></a>

**To create a cache behavior for an endpoint**

1. On the CloudFront console, choose the distribution that you created for your channel, and then choose **Distribution Settings**\.

1. On the **Behaviors** tab, choose **Create Behavior**\.

1. In the **Cache Behavior Settings** section, for **Path Pattern**, type the first path pattern for the endpoint type for this origin, using the following path pattern guidance\. For example, if it’s a DASH endpoint, type `*.mpd` for **Path Pattern**\.  
**Path Patterns**  
For an HLS or CMAF endpoint, create the following two cache behaviors:  
   + A cache behavior with a path pattern of `*.m3u8` \(for the parent manifest\)
   + A cache behavior with a path pattern of `*.ts` \(for the child manifest\)
For a DASH endpoint, create the following two cache behaviors:  
   + A cache behavior with a path pattern of `*.mpd` \(for the parent manifest\)
   + A cache behavior with a path pattern of `*.mp4` \(for the child manifest\)
For a Microsoft Smooth endpoint, only one manifest is served, so you create only one cache behavior:  
   + A cache behavior with a path pattern of `index.ism/*`
For all endpoint formats except for a Microsoft Smooth endpoint, you must repeat the steps, to add two cache behaviors\.

1. Specify values for the following settings, for each cache behavior:  
**Viewer Protocol Policy**  
Choose **Redirect HTTP to HTTPS**\.  
**Cache Based on Selected Request Headers**  
Choose **None \(improves caching\)**\.  
For more information about improving caching, see [Increasing the Proportion of Requests that Are Served from CloudFront Edge Caches](cache-hit-ratio.md)\.  
**Query String Forwarding and Caching**  
Choose **Forward all, cache based on whitelist**\.  
**Query String Whitelist**  
Specify the letter m as the query string parameter that you want CloudFront to use as the basis for caching\. The AWS Elemental MediaPackage response always includes the tag `?m=###` to capture the modified time of the endpoint\. If content is already cached with a different value for this tag, CloudFront requests a new manifest instead of serving the cached version\.  
**Object Caching**  
AWS Elemental MediaPackage sets default `Cache-Control` headers that ensure correct playback behavior\. If you want to use those values, choose Use Origin Cache Headers\. However, you can cache headers for longer segments\. For more information about customizing the time that objects stay in the CloudFront cache, see [Object Caching](distribution-web-values-specify.md#DownloadDistValuesObjectCaching) in the [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md) topic\.

1. Choose **Create**\.

1. If your endpoint is not a Microsoft Smooth endpoint, choose **Create Behavior**, and then repeat these steps to create a second cache behavior\.

### Step 4: Use CloudFront to Serve the Live Stream Channel<a name="live-streaming-with-mediapackage-serve-channel"></a>

After you create the distribution, add the origins, and create the cache behaviors, you can serve the live stream channel using CloudFront\. Content requests from viewers are routed to the correct AWS Elemental MediaPackage endpoints based on the settings that you configured for the cache behaviors\.

For links in your application \(for example, a media player\), specify the URL for the media file in the standard format for CloudFront URLs\. For more information, see [Format of URLs for Objects](LinkFormat.md)\.