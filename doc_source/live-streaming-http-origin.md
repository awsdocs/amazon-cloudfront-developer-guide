# Live HTTP Streaming Using CloudFront and Any HTTP Origin<a name="live-streaming-http-origin"></a>

With Amazon CloudFront live streaming, you can use any live encoder, such as Elemental Live, that outputs HTTP\-based streams to stream live performances, webinars, and other events\. This tutorial walks you through the process of configuring live streaming\.

**Note**  
This tutorial assumes that you have already set up your live encoder\.


+ [Creating a New CloudFront Distribution for Live Streaming](#live-streaming-http-origin-creating-distribution)
+ [Configuring Web Players to Play the Live Stream](#live-streaming-http-origin-configuring-web-players)
+ [\(Optional\) Deleting an AWS CloudFormation Stack for Live Streaming](#live-streaming-http-origin-deleting-stack)

## Creating a New CloudFront Distribution for Live Streaming<a name="live-streaming-http-origin-creating-distribution"></a>

The following procedure uses an AWS CloudFormation template to create a new CloudFront distribution that you will use for your live stream\. 

**To create a new CloudFront distribution for live streaming**

1. Go to the [Amazon Web Services](https://aws.amazon.com/) page, and sign in using your Amazon\.com account or create a new account\.

1. To start the wizard that creates a new CloudFront distribution, choose the following link:

   [Create a CloudFront distribution for live HTTP streaming](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#cstack=sn%7eLiveHTTPStreaming%7cturl%7ehttps:%2f%2fs3.amazonaws.com%2fcloudfront-live%2flive-http-streaming-custom-origin-cloudfront.json)

1. On the **Select Template** page, choose **Next**\.

1. On the **Specify Details** page, type the following values:  
**Stack name**  
Type a descriptive name for your live streaming stack, or accept the default name\.  
**CustomOriginDNSName**  
Type the DNS name of the custom origin that CloudFront will get the live feed from\.  
**CustomOriginHTTPPort**  
Type the HTTP TCP port that you want CloudFront to use to communicate with your custom origin\.  
**DistributionComment**  
Type a comment to help you identify your distribution, which is useful if you manage multiple distributions\. The value that you type here appears in the CloudFront console\.

1. Choose **Next**\.

1. \(Optional\) On the **Options** page, add the key\-value pairs for any tags you plan to use\. In addition, to configure SNS notification, to specify how long you're willing to wait for the stack to be created, to choose whether to roll back changes if stack creation fails, and to enter a stack policy, choose **Advanced**, and adjust settings as desired\. For more information, see [Setting AWS CloudFormation Stack Options](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html)\. 

1. Choose **Next**\.

1. Review the settings for the stack, and then choose **Create**\. AWS CloudFormation creates the stack\.

   Creating your stack creation might take several minutes\. To track the progress of stack creation, select the stack, and then choose the **Events** tab\. If AWS CloudFormation cannot create the stack, the **Events** tab lists error messages\.

   When your stack is ready, in the list of stacks, the status for the stack changes to **CREATE\_COMPLETE**\.

1. When your stack is created, choose the **Outputs** tab, which will display the CloudFront domain name\. You'll need this value when you set up the live stream playback in your web player\.

## Configuring Web Players to Play the Live Stream<a name="live-streaming-http-origin-configuring-web-players"></a>

To play the live stream, embed the manifest URL in the players that your users will play your live stream with\. For example, to play a live stream for which the manifest file is **myStream/playlist\.m3u8** and the CloudFront distribution is **d111111abcdef8\.cloudfront\.net**, you embed the following URL in players:

**http://d111111abcdef8\.cloudfront\.net/myStream/playlist\.m3u8**

## \(Optional\) Deleting an AWS CloudFormation Stack for Live Streaming<a name="live-streaming-http-origin-deleting-stack"></a>

When your live event is over, delete the stack that you created for live streaming\. This deletes the CloudFront distribution that you created for your live\-streaming event\. 

**Note**  
When you stop streaming, you also stop incurring CloudFront charges for this distribution\. There are no charges for the AWS CloudFormation stack\.

**To delete an AWS CloudFormation stack for live streaming**

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. In the upper\-right corner, choose the region in which you created your stack\.

1. Select the stack, and then choose **Delete Stack**\. 

1. Choose **Yes, Delete** to confirm\. 

1. To track the progress of the stack deletion, select the stack, and then choose the **Events** tab\.