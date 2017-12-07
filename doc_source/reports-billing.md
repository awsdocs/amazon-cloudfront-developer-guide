# AWS Billing and Usage Reports for CloudFront<a name="reports-billing"></a>

Amazon CloudFront is designed so you don't have to pay any up\-front fees or commit to how much content you'll have\. As with the other AWS services, you pay as you go and pay only for what you use\.

The following diagram and table summarize the charges to use CloudFront\. 

![\[Paying for CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)

Your monthly bill from AWS separates your usage and dollar amounts by AWS service and function\. The following table lists the charges that are illustrated in the previous graphic\.


****  

|  | Charge | Comments | 
| --- | --- | --- | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/1.png)  | Storage in an Amazon S3 bucket | You pay normal Amazon S3 storage charges to store objects in your bucket; the charges appear in the Amazon S3 portion of your AWS statement\. | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/2.png)  | Serving objects from edge locations | You incur CloudFront charges when CloudFront responds to requests for your objects\. These charges are lower than the corresponding Amazon S3 charges\. The CloudFront charges appear in the CloudFront portion of your AWS statement\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/callouts/3.png)  | Submitting data to your origin |  You incur CloudFront charges when users transfer data to your origin, which includes `DELETE`, `OPTIONS`, `PATCH`, `POST`, and `PUT` requests\. The CloudFront charges appear in the CloudFront portion of your AWS statement\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.  | 

**Note**  
You also incur a surcharge for HTTPS requests\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.

AWS provides two usage reports for CloudFront:

+ The billing report is a high\-level view of all of the activity for the AWS services that you're using, including CloudFront\. For more information, see [AWS Billing Report for CloudFront](billing-report.md)\.

+ The usage report is a summary of activity for a specific service, aggregated by hour, day, or month\. For more information, see [AWS Usage Report for CloudFront](usage-report.md)\.

In addition, you can view usage charts that provide a graphical representation of your CloudFront usage\. For more information, see [CloudFront Usage Reports](usage-charts.md)\.