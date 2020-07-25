# Updating a Distribution<a name="HowToUpdateDistribution"></a>

In the CloudFront console, you can see the CloudFront distributions that are associated with your AWS account, view the settings for a distribution, and update most settings\. Be aware that settings changes that you make won't take effect until the distribution has propagated to the AWS edge locations\.<a name="HowToUpdateDistributionProcedure"></a>

**To Update a CloudFront Distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the ID of a distribution\. The list includes all of the distributions associated with the AWS account that you used to sign in to the CloudFront console\.

1. To edit settings for a distribution, choose the **Distribution Settings** tab\.

1. To make updates, do one of the following:

   1. To update general settings, choose **Edit**\. Otherwise, choose the tab for the settings that you want to update: **Origins** or **Behaviors**\.

   1. For settings for an RTMP distribution, choose **Edit**, and then update values\.

      For information about the fields, see [Values that You Specify When You Create or Update an RTMP Distribution](distribution-rtmp-values-specify.md)\.

1. Make the updates, and then, to save your changes, choose **Yes, Edit**\. For information about the fields, see the following topics: 
   + **General settings:** [Distribution Settings](distribution-web-values-specify.md#DownloadDistValuesGeneral)
   + **Origin settings:** [Origin Settings](distribution-web-values-specify.md#DownloadDistValuesOrigin)
   + **Cache behavior settings:** [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)

1. If you want to delete an origin in your distribution, do the following:

   1. Choose **Behaviors**, and then make sure you have moved any default cache behaviors associated with the origin to another origin\.

   1. Choose **Origins**, and then select an origin\.

   1. Choose **Delete**\.

You can also update a distribution by using the CloudFront API:
+ To update a distribution, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.

**Important**  
When you update your distribution, be aware that a number of additional fields are required that are not required to create a distribution\. For a summary of the fields required for when you create or update a distribution, see [Required fields for creating and updating distributions](distribution-overview-required-fields.md)\. To help make sure that all of the required fields are included when you update a distribution by using the CloudFront API, follow the steps described in [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\.

When you save changes to your distribution configuration, CloudFront starts to propagate the changes to all edge locations\. Until your configuration is updated in an edge location, CloudFront continues to serve your content from that location based on the previous configuration\. After your configuration is updated in an edge location, CloudFront immediately starts to serve your content from that location based on the new configuration\.

Your changes don't propagate to every edge location instantaneously\. When propagation is complete, the status of your distribution changes from **InProgress** to **Deployed**\. While CloudFront is propagating your changes, we unfortunately can't determine whether a given edge location is serving your content based on the previous configuration or the new configuration\.