# Viewing and Updating Distributions<a name="HowToUpdateDistribution"></a>

In the CloudFront console, you can see the CloudFront distributions that are associated with your AWS account, view the settings for a distribution, and update most settings\. Be aware that settings changes that you make won't take effect until the distribution has propagated to the AWS edge locations\.<a name="HowToUpdateDistributionProcedure"></a>

**To Update CloudFront Distributions \(Console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the ID of a distribution\. The list includes all of the distributions associated with the AWS account that you used to sign in to the CloudFront console\.

1. To view or edit settings for a distribution, choose the **Distribution Settings** tab\.

1. To make updates, do one of the following:

   1. For RTMP distribution settings, choose **Edit**, and then update values\.

      For information about the fields, see [Values that You Specify When You Create or Update an RTMP Distribution](distribution-rtmp-values-specify.md)\.

   1. For distribution settings, to update general settings, choose **Edit**\. Otherwise, choose the tab for the settings that you want to view or update: **Origins** or **Behaviors**\.

1. Make the updates, and then, to save your changes, choose **Yes, Edit**\. For information about the fields, see the following topics: 
   + **General settings:** [Distribution Details](distribution-web-values-specify.md#DownloadDistValuesGeneral)
   + **Origin settings:** [Origin Settings](distribution-web-values-specify.md#DownloadDistValuesOrigin)
   + **Cache behavior settings:** [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)

1. If you want to delete an origin in your distribution, do the following:

   1. Choose **Behaviors**, and make sure you have moved any default cache behaviors associated with the origin to another origin\.

   1. Choose **Origins**, and then select an origin\.

   1. Choose **Delete**\.

When you save changes to your distribution configuration, CloudFront starts to propagate the changes to all edge locations\. Until your configuration is updated in an edge location, CloudFront continues to serve your content from that location based on the previous configuration\. After your configuration is updated in an edge location, CloudFront immediately starts to serve your content from that location based on the new configuration\.

Your changes don't propagate to every edge location instantaneously\. When propagation is complete, the status of your distribution changes from **InProgress** to **Deployed**\. While CloudFront is propagating your changes, we unfortunately can't determine whether a given edge location is serving your content based on the previous configuration or the new configuration\.