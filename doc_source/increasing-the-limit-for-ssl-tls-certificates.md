# Increasing the Limit for SSL/TLS Certificates<a name="increasing-the-limit-for-ssl-tls-certificates"></a>

There are limits on the number of SSL/TLS certificates that you can import into [AWS Certificate Manager](http://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) or upload to [AWS Identity and Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. There also is a limit on the number of SSL/TLS certificates that you can use with an AWS account when you configure CloudFront to serve HTTPS requests by using dedicated IP addresses\. However, you can request higher limits\.


+ [Certificates That You Can Import into ACM](#certificates-to-import-into-acm)
+ [Certificates That You Can Upload to IAM](#certificates-to-upload-into-iam)
+ [Certificates That You Can Use with Dedicated IP Addresses](#certificates-using-dedicated-ip-address)

## Certificates That You Can Import into ACM<a name="certificates-to-import-into-acm"></a>

For the limit on the number of certificates that you can import into ACM, see [ACM Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_acm) in the *Amazon Web Services General Reference*\.

To request a higher limit, [create a case](https://console.aws.amazon.com/support/home?region=us-west-2#/case/create?issueType=service-limit-increase) with the AWS Support Center\. Specify the following values:

+ **Regarding** – Accept the default value of **Service Limit Increase**

+ **Limit Type** – Choose **Certificate Manager**

+ **Region** – Specify the AWS Region where you want to import certificates

+ **Limit** – Choose **\(ACM\) Number of ACM Certificates**

Then fill out the rest of the form\.

## Certificates That You Can Upload to IAM<a name="certificates-to-upload-into-iam"></a>

For the limit on the number of certificates that you can upload to IAM, see [IAM Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_iam) in the *Amazon Web Services General Reference*\.

To request a higher limit, [create a case](https://console.aws.amazon.com/support/home?region=us-west-2#/case/create?issueType=service-limit-increase) with the AWS Support Center\. Specify the following values:

+ **Regarding** – Accept the default value of **Service Limit Increase**

+ **Limit Type** – Choose **Certificate Manager**

+ **Region** – Specify the AWS Region where you want to import certificates

+ **Limit** – Choose **\(IAM\) Server Certificate Limit**

Then fill out the rest of the form\.

## Certificates That You Can Use with Dedicated IP Addresses<a name="certificates-using-dedicated-ip-address"></a>

For the limit on the number of SSL certificates that you can use for each AWS account when serving HTTPS requests using dedicated IP addresses, see [CloudFront Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_cloudfront) in the *Amazon Web Services General Reference*\.

To request a higher limit, [create a case](https://console.aws.amazon.com/support/home?region=us-west-2#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) with the AWS Support Center\. Specify the following values:

+ **Regarding** – Accept the default value of **Service Limit Increase**

+ **Limit Type** – Accept the default value of **CloudFront Distributions**

+ **Limit** – Choose **Dedicated IP SSL Certificate Limit per Account**

Then fill out the rest of the form\.