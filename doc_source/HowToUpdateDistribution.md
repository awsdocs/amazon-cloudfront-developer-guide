# Listing, Viewing, and Updating CloudFront Distributions<a name="HowToUpdateDistribution"></a>

You can use the CloudFront console to list the CloudFront distributions that are associated with your AWS account, view the settings for a distribution, and update most settings\. 

When you save changes to your distribution configuration, CloudFront starts to propagate the changes to all edge locations\. Until your configuration is updated in an edge location, CloudFront continues to serve your content from that location based on the previous configuration\. After your configuration is updated in an edge location, CloudFront immediately starts to serve your content from that location based on the new configuration\.

Your changes don't propagate to every edge location instantaneously\. When propagation is complete, the status of your distribution changes from **InProgress** to **Deployed**\. While CloudFront is propagating your changes to edge locations, we cannot determine whether a given edge location is serving your content based on the previous configuration or the new configuration\.

**To List, View, and Update CloudFront Distributions Using the CloudFront Console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID of the distribution that you want to view or update\.
**Note**  
The top pane lists all of the distributions that are associated with the AWS account that you used when you signed in to the CloudFront console\. 

1. To view or edit RTMP distribution settings, skip to Step 4\.

   To view or edit settings for a web distribution, perform the following steps\. 

   1. In the **Distribution Settings** pane, click the tab for the settings that you want to change: **General**, **Origins**, or **Behaviors**\.

   1. For general settings, click **Edit**\.

      For origins or cache behaviors, click the origin or cache behavior, and click **Edit**\.

   1. Enter or update the applicable values\. For information about the fields, see the following topics: 

      + **General settings:** [Distribution Details](distribution-web-values-specify.md#DownloadDistValuesGeneral)

      + **Origin settings:** [Origin Settings](distribution-web-values-specify.md#DownloadDistValuesOrigin)

      + **Cache behavior settings:** [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)

   1. Click **Yes, Edit**\.

1. To edit or view settings for an RTMP distribution:

   1. In the **Distribution Details** pane, click **Edit**\.

   1. Enter or update the applicable values\. For information about the fields, see [Values that You Specify When You Create or Update an RTMP Distribution](distribution-rtmp-values-specify.md)\.

   1. Click **Yes, Edit**\.