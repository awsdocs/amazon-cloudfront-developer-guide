# HTTP 400 Status Code \(Bad Request\)<a name="http-400-bad-request"></a>

Your CloudFront distribution might send error responses with HTTP status code 400 Bad Request, and a message similar to the following:

*The authorization header is malformed; the region '*<AWS Region>*' is wrong; expecting '*<AWS Region>*'*

For example:

*The authorization header is malformed; the region 'us\-east\-1' is wrong; expecting 'us\-west\-2'*

This problem can occur in the following scenario:

1. Your CloudFront distribution's origin is an Amazon S3 bucket\.

1. You moved the S3 bucket from one AWS Region to another\. That is, you deleted the S3 bucket, then later you created a new bucket with the same bucket name, but in a different AWS Region than where the original S3 bucket was located\.

To fix this error, update your CloudFront distribution so that it finds the S3 bucket in the bucket's current AWS Region\.

**To update your CloudFront distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the distribution that produces this error\.

1. Choose **Origins and Origin Groups**\.

1. Find the origin for the S3 bucket that you moved\. Select the check box next to this origin, then choose **Edit**\.

1. Choose **Yes, Edit**\. You do not need to change any settings before choosing **Yes, Edit**\.

When you complete these steps, CloudFront redeploys your distribution\. The distribution's status in the CloudFront console changes to **In Progress** while the distribution is deploying\. When the deployment is complete, the distribution's status changes to **Deployed**, and you should stop receiving the AuthorizationHeaderMalformed error responses\. Even after the status changes to **Deployed**, it might take some time before you stop receiving this error\.