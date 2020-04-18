# Deleting Lambda@Edge Functions and Replicas<a name="lambda-edge-delete-replicas"></a>

You can delete a Lambda@Edge function only when the replicas of the function have been deleted by CloudFront\. Replicas of a Lambda function are automatically deleted in the following situations:
+ After you remove the last association for the function from all of your CloudFront distributions\. If more than one distribution uses a function, the replicas are deleted only after you remove the function association from the last distribution\.
+ After you delete the last distribution that a function was associated with\.

Replicas are typically deleted within a few hours\. You cannot manually delete Lambda@Edge function replicas\. This helps prevent a situation where a replica is deleted that is still in use, which would result in an error\.

Don’t build applications that use Lambda@Edge function replicas outside of CloudFront\. These replicas are deleted when their associations with distributions are removed, or when distributions themselves are deleted\. The replica that an outside application depends on might be removed without warning, causing it to fail\.

**To delete a Lambda@Edge function association from a CloudFront distribution \(console\)**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID of the distribution that has the Lambda@Edge function association that you want to delete\.

1. Choose the **Behaviors** tab\.

1. Choose the check box next to the cache behavior that has the Lambda@Edge function association that you want to delete, and then choose **Edit**\.

1. Scroll down to **Lambda Function Associations**, and then choose the **X** icon next to each Lambda@Edge function association that you want to delete\.

1. Choose **Yes, Edit** to save your changes\.

After you delete a Lambda@Edge function association from a CloudFront distribution, you can optionally delete the Lambda function or function version from AWS Lambda\. You can also delete a specific version of a Lambda function if the version doesn’t have any CloudFront distributions associated with it\. If you remove all the associations for a Lambda function version, you can typically delete the function version a few hours later\.