# Delivering live streaming video with CloudFront and AWS Media Services<a name="live-streaming"></a>

To use AWS Media Services with CloudFront to deliver live content to a global audience, follow the guidance included in this section\.

Use [AWS Elemental MediaLive](https://docs.aws.amazon.com/medialive/latest/ug/getting-started.html) to encode live video streams in real time\. To encode a large video stream, MediaLive compresses it into smaller versions \(*encodes*\) that can be distributed to your viewers\.

After you compress a live video stream, you can use either of the following two main options to prepare and serve the content:
+ **Convert your content into required formats, and then serve it:** If you require content in multiple formats, use [AWS Elemental MediaPackage](https://aws.amazon.com/mediapackage/) to package the content for different device types\. When you package the content, you can also implement extra features and add digital rights management \(DRM\) to prevent unauthorized use of your content\. For step\-by\-step instructions for using CloudFront to serve content that MediaPackage formatted, see [Serving live video formatted with AWS Elemental MediaPackage](#live-streaming-with-mediapackage)\.
+ **Store and serve your content using scalable origin:** If MediaLive encoded content in the formats required by all of the devices that your viewers use, use a highly scalable origin like [AWS Elemental MediaStore](https://docs.aws.amazon.com/mediastore/latest/ug/getting-started.html) to serve the content\. For step\-by\-step instructions for using CloudFront to serve content that is stored in a MediaStore container, see [Serving video using AWS Elemental MediaStore as the origin](#video-streaming-mediastore)\.

After you’ve set up your origin by using one of these options, you can distribute live streaming video to viewers by using CloudFront\.

**Tip**  
You can learn about an AWS solution that automatically deploys services for building a highly available real\-time viewing experience\. To see the steps to automatically deploy this solution, see [Live Streaming Automated Deployment](https://docs.aws.amazon.com/solutions/latest/live-streaming/deployment.html)\.

**Topics**
+ [Serving video using AWS Elemental MediaStore as the origin](#video-streaming-mediastore)
+ [Serving live video formatted with AWS Elemental MediaPackage](#live-streaming-with-mediapackage)

## Serving video using AWS Elemental MediaStore as the origin<a name="video-streaming-mediastore"></a>

If you have video stored in an [AWS Elemental MediaStore](https://docs.aws.amazon.com/mediastore/latest/ug/getting-started.html) container, you can create a CloudFront distribution to serve the content\.

To get started, you grant CloudFront access to your MediaStore container\. Then you create a CloudFront distribution and configure it to work with MediaStore\.

**To serve content from an AWS Elemental MediaStore container**

1. Follow the procedure at [Allowing Amazon CloudFront to access your AWS Elemental MediaStore container](https://docs.aws.amazon.com/mediastore/latest/ug/cdns-allowing-cloudfront-to-access-mediastore.html), and then return to these steps to create your distribution\.

1. Create a distribution with the following settings:  
**Origin domain**  
The data endpoint that is assigned to your MediaStore container\. From the dropdown list, choose the MediaStore container for your live video\.  
**Origin path**  
The folder structure in the MediaStore container where your objects are stored\. For more information, see [Origin path](distribution-web-values-specify.md#DownloadDistValuesOriginPath)\.  
**Add custom header**  
Add header names and values if you want CloudFront to add custom headers when it forwards requests to your origin\.  
**Viewer protocol policy**  
Choose **Redirect HTTP to HTTPS**\. For more information, see [Viewer protocol policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\.  
**Cache policy and origin request policy**  
For **Cache policy**, choose **Create policy**, and then create a cache policy that’s appropriate for your caching needs and segment durations\. After you create the policy, refresh the list of cache policies and choose the policy that you just created\.  
For **Origin request policy**, choose **CORS\-CustomOrigin** from the dropdown list\.

   For the other settings, you can set specific values based on other technical requirements or the needs of your business\. For a list of all the options for distributions and information about setting them, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\.

1. For links in your application \(for example, a media player\), specify the name of the media file in the same format that you use for other objects that you’re distributing using CloudFront\.

## Serving live video formatted with AWS Elemental MediaPackage<a name="live-streaming-with-mediapackage"></a>

If you formatted a live stream by using AWS Elemental MediaPackage, you can create a CloudFront distribution and configure cache behaviors to serve the live stream\. The following process assumes that you have already [ created a channel](https://docs.aws.amazon.com/mediapackage/latest/ug/channels-create.html) and [added endpoints](https://docs.aws.amazon.com/mediapackage/latest/ug/channels-add-endpoint.html) for your live video using MediaPackage\.

**Note**  
Instead of using the following process, you can choose to automatically create a CloudFront distribution when you save a channel in MediaPackage\. For more information, see [Creating a Distribution from AWS Elemental MediaPackage](https://docs.aws.amazon.com/mediapackage/latest/ug/cdns-create-mp.html) in the *AWS Elemental MediaPackage User Guide*\.

To create a CloudFront distribution for MediaPackage manually, follow these steps:

**Topics**
+ [Step 1: Create and configure a CloudFront distribution](#live-streaming-with-mediapackage-create-dist)
+ [Step 2: Add the other endpoints as origins](#live-streaming-with-mediapackage-add-endpoints)
+ [Step 3: Configure cache behaviors for all endpoints](#live-streaming-with-mediapackage-create-cache-behavior)
+ [Step 4: Use CloudFront to serve the live stream channel](#live-streaming-with-mediapackage-serve-channel)

### Step 1: Create and configure a CloudFront distribution<a name="live-streaming-with-mediapackage-create-dist"></a>

Complete the following procedure to set up a CloudFront distribution for the live video channel that you created with MediaPackage\.<a name="live-streaming-with-mediapackage-create-dist-procedure"></a>

**To create a distribution for your live video channel**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose **Create distribution**\.

1. Choose the settings for the distribution, including the following:  
**Origin domain**  
The origin where your MediaPackage live video channel and endpoints are\. Choose the text field, then from the dropdown list, choose the MediaPackage channel for your live video\. You can map one channel to several origin endpoints\.  
If you created your channel using another AWS account, type the origin URL value into the field\. The origin must be an HTTPS URL\.  
For more information, see [Origin domain](distribution-web-values-specify.md#DownloadDistValuesDomainName)\.  
**Origin path**  
The path to the MediaPackage endpoint from where the content is served\. When you choose an origin domain name, CloudFront populates the origin path\.  
If you used a channel from another AWS account for **Origin domain**, the **Origin path** field is not filled in for you\. You must get the correct origin path from the other account so that you can enter it manually\.  
For more information about how an origin path works, see [Origin path](distribution-web-values-specify.md#DownloadDistValuesOriginPath)\.

   For the other distribution settings, set specific values based on other technical requirements or the needs of your business\. For a list of all the options for distributions and information about setting them, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\.

   When you finish choosing the other distribution settings, choose **Create distribution**\.

1. Choose the distribution that you just created, then choose **Behaviors**\.

1. Select the default cache behavior, then choose **Edit**\. Specify the correct cache behavior settings for the channel that you chose for the origin\. Later, you’ll add one or more additional origins and edit cache behavior settings for them\.

1. Go to the [CloudFront distributions page](https://console.aws.amazon.com/cloudfront/v3/home#/distributions)\.

1. Wait until the value of the **Last modified** column for your distribution has changed from **Deploying** to a date and time, indicating that CloudFront has created your distribution\.

### Step 2: Add the other endpoints as origins<a name="live-streaming-with-mediapackage-add-endpoints"></a>

Repeat the steps here to add each of your MediaPackage channel endpoints to your distribution\.<a name="live-streaming-with-mediapackage-add-endpoints-procedure"></a>

**To add other endpoints as origins**

1. On the CloudFront console, choose the distribution that you created for your channel\.

1. Choose **Origins**, then choose **Create origin**\.

1. For **Origin domain**, in the dropdown list, choose a MediaPackage endpoint for your channel\. CloudFront automatically completes the **Origin path** field\.

1. For the other settings, set the values based on other technical requirements or the needs of your business\. For more information, see [Origin settings](distribution-web-values-specify.md#DownloadDistValuesOrigin)\.

1. Choose **Create origin**\.

### Step 3: Configure cache behaviors for all endpoints<a name="live-streaming-with-mediapackage-create-cache-behavior"></a>

For each endpoint, you must configure cache behaviors to add path patterns that route requests correctly\. The path patterns that you specify depend on the video format that you’re serving\. The following procedure includes the path pattern information to use for Apple HLS, CMAF, DASH, and Microsoft Smooth Streaming formats\.

You typically set up two cache behaviors for each endpoint:
+ The parent manifest, which is the index to your files\.
+ The segments, which are the files of the video content\.<a name="live-streaming-with-mediapackage-create-cache-behavior-procedure"></a>

**To create a cache behavior for an endpoint**

1. On the CloudFront console, choose the distribution that you created for your channel\.

1. Choose **Behaviors**, then choose **Create behavior**\.

1. For **Path pattern**, type the first pattern indicated in the following guidance for each endpoint type\. For example, for a DASH endpoint, type `*.mpd`\.  
**Path patterns**  
For an HLS endpoint, create the following two cache behaviors:  
   + For parent and child manifests, use `*.m3u8`\.
   + For the content segments, use `*.ts`\.
For a CMAF endpoint, create the following two cache behaviors:  
   + For parent and child manifests, use `*.m3u8`\.
   + For the content segments, use `*.mp4`\.
For a DASH endpoint, create the following two cache behaviors:  
   + For the parent manifest, use `*.mpd`\.
   + For the content segments, use `*.mp4`\.
For a Microsoft Smooth Streaming endpoint, only a manifest is served, so you create only one cache behavior: `index.ism/*`\.

1. For each cache behavior, specify values for the following settings:  
**Viewer protocol policy**  
Choose **Redirect HTTP to HTTPS**\.  
**Cache policy and origin request policy**  
For **Cache policy**, choose **Create policy**\. For your new cache policy, specify the following settings:    
**Minimum TTL**  
Set to 5 seconds or less, to help prevent serving stale content\.  
**Query strings**  
For **Query strings** \(in **Cache key settings**\), choose **Include specified query strings**\. For **Allow**, add the following values by typing them and then choosing **Add item**:  
   + Add `m` as a query string parameter that you want CloudFront to use as the basis for caching\. The MediaPackage response always includes the tag `?m=###` to capture the modified time of the endpoint\. If content is already cached with a different value for this tag, CloudFront requests a new manifest instead of serving the cached version\.
   + If you’re using the time\-shifted viewing functionality in MediaPackage, specify `start` and `end` as additional query string parameters on the cache behavior for manifest requests \(`*.m3u8`, `*.mpd`, and `index.ism/*`\)\. This way, content is served that’s specific to the requested time period in the manifest request\. For more information about time\-shifted viewing and formatting content start and end request parameters, see [Time\-shifted viewing](https://docs.aws.amazon.com/mediapackage/latest/ug/time-shifted.html) in the *AWS Elemental MediaPackage User Guide*\.
   + If you’re using the manifest filtering feature in MediaPackage, specify `aws.manifestfilter` as an additional query string parameter for the cache policy that you use with the cache behavior for manifest requests \(`*.m3u8`, `*.mpd`, and `index.ism/*`\)\. This configures your distribution to forward the `aws.manifestfilter` query string to your MediaPackage origin, which is required for the manifest filtering feature to work\. For more information, see [Manifest filtering](https://docs.aws.amazon.com/mediapackage/latest/ug/manifest-filtering.html) in the *AWS Elemental MediaPackage User Guide*\.

1. Choose **Create**\.

1. After you create the cache policy, go back to the cache behavior creation workflow\. Refresh the list of cache policies, and choose the policy that you just created\.

1. Choose **Create behavior**\.

1. If your endpoint is not a Microsoft Smooth Streaming endpoint, repeat these steps to create a second cache behavior\.

### Step 4: Use CloudFront to serve the live stream channel<a name="live-streaming-with-mediapackage-serve-channel"></a>

After you create the distribution, add the origins, and create the cache behaviors, you can serve the live stream channel using CloudFront\. CloudFront routes requests from viewers to the correct MediaPackage endpoints based on the settings that you configured for the cache behaviors\.

For links in your application \(for example, a media player\), specify the URL for the media file in the standard format for CloudFront URLs\. For more information, see [Customizing the URL format for files in CloudFront](LinkFormat.md)\.