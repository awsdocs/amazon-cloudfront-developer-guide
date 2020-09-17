# Deleting a Distribution<a name="HowToDeleteDistribution"></a>

If you no longer want to use a distribution, you can delete it by using the CloudFront console or by using the CloudFront API\. 

Be aware that before you can delete a distribution, you must disable it, which requires permission to update the distribution\. For more information about setting permissions for working with CloudFront, including setting UpdateDistribution and DeleteDistribution permissions, see [Customer Managed Policy Examples](access-control-managing-permissions.md#access-policy-examples-for-sdk-cli)\. 

**Note**  
If you disable a distribution that has an alternate domain name associated with it, CloudFront stops accepting traffic for that domain name \(such as www\.example\.com\), even if another distribution has an alternate domain name with a wildcard \(\*\) that matches the same domain \(such as \*\.example\.com\)\.<a name="HowToDeleteDistributionProcedure"></a>

**To Delete a CloudFront Distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the right pane of the CloudFront console, find the distribution that you want to delete\.

1. If the value of the **State** column is **Disabled**, skip to Step 7\.

   If the value of **State** is **Enabled** and the value of **Status** is **Deployed**, continue with Step 4 to disable the distribution before deleting it\.

   If the value of **State** is **Enabled** and the value of **Status** is **InProgress**, wait until **Status** changes to **Deployed**\. Then continue with Step 4 to disable the distribution before deleting it\.

1. In the right pane of the CloudFront console, select the check box for the distribution that you want to delete\.

1. Choose **Disable** to disable the distribution, and choose **Yes, Disable** to confirm\. Then choose **Close**\.
**Note**  
Because CloudFront must propagate this change to all edge locations, it might take a few minutes before the update is complete and you can delete your distribution\.

1. The value of the **State** column immediately changes to **Disabled**\. Wait until the value of the **Status** column changes to **Deployed**\.

1. Check the check box for the distribution that you want to delete\.

1. Choose **Delete**, and choose **Yes, Delete** to confirm\. Then click **Close**\.
**Note**  
If you have just marked your distribution as disabled, CloudFront might still need a few more minutes to propagate that change to the edge locations\. Until propagation is complete, the **Delete** option isn't available\.

You can also delete a distribution using the CloudFront API:
+ To delete a distribution, see [DeleteDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteDistribution.html) in the *Amazon CloudFront API Reference*\.
+ To delete an RTMP distribution, see [DeleteStreamingDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteStreamingDistribution.html) in the *Amazon CloudFront API Reference*\.