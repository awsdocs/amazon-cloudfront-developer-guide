# CloudFront pricing<a name="CloudFrontPricing"></a>

Amazon CloudFront is designed so you don’t have to pay any up\-front fees or commit to how much content you’ll have\. As with the other AWS services, you pay as you go and pay only for what you use\. For information about prices, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

**Tip**  
To avoid surprise charges from CloudFront \(or any AWS service\), you can use AWS Budgets\. With AWS Budgets you can set cost thresholds and get notifications by email or Amazon SNS topic when your actual or forecasted charges exceed a threshold\. For more information, see [Managing your costs with AWS Budgets](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-managing-costs.html) and [Creating a budget](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-create.html) in the *AWS Billing and Cost Management User Guide*\. To get started, go to [AWS Budgets in the console](https://console.aws.amazon.com/billing/home?#/budgets)\.

AWS provides two usage reports for CloudFront: a billing report and a report that summarizes usage activity\. To learn more about these reports, see [AWS billing and usage reports for CloudFront](reports-billing.md)\.

The following diagram and list summarize the charges to use CloudFront\.

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/Charges.png)

Your monthly bill from AWS allocates your usage and dollar amounts by AWS service and function\. The following explains the charges that are illustrated in the previous graphic\. For more information about prices, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

1. **Charge for storage in an Amazon S3 bucket\.** You pay normal Amazon S3 storage charges to store objects in your bucket\. The charges appear in the Amazon S3 portion of your AWS statement\.

1. **Charge for serving objects from edge locations\.** You incur CloudFront charges when CloudFront responds to requests for your objects\. The charges include data transfer for WebSocket data from server to client\. The CloudFront charges appear in the CloudFront portion of your AWS statement as *region* *\-DataTransfer\-Out\-Bytes*\. 

1. **Charge for submitting data to your origin\.** You incur CloudFront charges when users transfer data to your origin, which includes `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests\. The charges include data transfer for WebSocket data from client to server\. The CloudFront charges appear in the CloudFront portion of your AWS statement as *region* *\-DataTransfer\-Out\-OBytes*\.

Be aware of the following:
+ You also incur a surcharge for HTTPS requests, and an additional surcharge for requests that also have field\-level encryption enabled or that use [Origin Shield](origin-shield.md) as an incremental caching layer\. For more information about prices, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.
+ You do not incur any additional CloudFront charges when you use origin groups\. You continue to pay the same request fees and data transfer rates as you do when you use CloudFront with any other AWS or non\-AWS origin\. For more information, see [Using CloudFront Origin Groups](DownloadDistS3AndCustomOrigins.md#concept_origin_groups)\. 