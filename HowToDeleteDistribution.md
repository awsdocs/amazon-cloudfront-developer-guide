# Deleting a Distribution<a name="HowToDeleteDistribution"></a>

If you no longer want to use a distribution, use the following procedure to delete it using the CloudFront console\. 

You can also delete a distribution using the CloudFront API:
+ To delete a web distribution, use the `DeleteDistribution` API action\. For more information, go to [DeleteDistribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteDistribution.html) in the *Amazon CloudFront API Reference*\.
+ To delete an RTMP distribution, use the `DeleteStreamingDistribution` API action\. For more information, go to [DeleteStreamingDistribution](http://docs.aws.amazon.com/cloudfront/latest/APIReference/API_DeleteStreamingDistribution.html) in the *Amazon CloudFront API Reference*\.

**Note**  
CloudFront lets you create a total of up to 200 web distributions and 100 RTMP distributions for an AWS account\.<a name="HowToDeleteDistributionProcedure"></a>

**To Delete a CloudFront Distribution Using the CloudFront Console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. In the right pane of the CloudFront console, find the distribution that you want to delete\.

1. If the value of the **State** column is **Disabled**, skip to Step 7\.

   If the value of **State** is **Enabled** and the value of **Status** is **Deployed**, continue with Step 4 to disable the distribution before deleting it\.

   If the value of **State** is **Enabled** and the value of **Status** is **InProgress**, wait until **Status** changes to **Deployed**\. Then continue with Step 4 to disable the distribution before deleting it\.

1. In the right pane of the CloudFront console, check the check box for the distribution that you want to delete\.

1. Click **Disabled** to disable the distribution, and click **Yes, Disable** to confirm\. Then click **Close**\.

1. The value of the **State** column immediately changes to **Disabled**\. Wait until the value of the **Status** column changes to **Deployed**\.

1. Check the check box for the distribution that you want to delete\.

1. Click **Delete**, and click **Yes, Delete** to confirm\. Then click **Close**\.