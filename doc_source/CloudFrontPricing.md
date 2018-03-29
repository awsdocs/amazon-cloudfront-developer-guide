# CloudFront Pricing<a name="CloudFrontPricing"></a>

Amazon CloudFront is designed so you don't have to pay any up\-front fees or commit to how much content you'll have\. As with the other AWS services, you pay as you go and pay only for what you use\. 

AWS provides two usage reports for CloudFront: a billing report and a report that summarizes usage activity\. To learn more about these reports, see [AWS Billing and Usage Reports for CloudFront](reports-billing.md)\.

The following diagram and list summarize the charges to use CloudFront\. 

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

Your monthly bill from AWS allocates your usage and dollar amounts by AWS service and function\. The following explains the charges that are illustrated in the previous graphic\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

1. **Charge for storage in an Amazon S3 bucket\.** You pay normal Amazon S3 storage charges to store objects in your bucket\. The charges appear in the Amazon S3 portion of your AWS statement\.

1. **Charge for serving objects from edge locations\.** You incur CloudFront charges when CloudFront responds to requests for your objects\. These charges are lower than the corresponding Amazon S3 charges\. The CloudFront charges appear in the CloudFront portion of your AWS statement\. 

1. **Charge for submitting data to your origin\.** You incur CloudFront charges when users transfer data to your origin, which includes `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests\. The CloudFront charges appear in the CloudFront portion of your AWS statement\.

**Note**  
You also incur a surcharge for HTTPS requests, and an additional surcharge for requests that also have field\-level encryption enabled\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.