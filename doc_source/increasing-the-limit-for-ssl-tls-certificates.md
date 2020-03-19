# Increasing the Quotas for SSL/TLS Certificates<a name="increasing-the-limit-for-ssl-tls-certificates"></a>

There are quotas \(formerly known as limits\) on the number of SSL/TLS certificates that you can import into [AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) or upload to [AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)\. There also is a quota on the number of SSL/TLS certificates that you can use with an AWS account when you configure CloudFront to serve HTTPS requests by using dedicated IP addresses\. However, you can request higher quotas\.

**Topics**
+ [Certificates That You Can Import into ACM](#certificates-to-import-into-acm)
+ [Certificates That You Can Upload to IAM](#certificates-to-upload-into-iam)
+ [Certificates That You Can Use with Dedicated IP Addresses](#certificates-using-dedicated-ip-address)

## Certificates That You Can Import into ACM<a name="certificates-to-import-into-acm"></a>

For the quota on the number of certificates that you can import into ACM, see [Quotas](https://docs.aws.amazon.com/acm/latest/userguide/acm-limits.html) in the *AWS Certificate Manager User Guide*\.

To request a higher quota, [create a case](https://console.aws.amazon.com/support/cases#/create?issueType=service-limit-increase) in the AWS Support Center\. Specify the following values:
+ Accept the default value of **Service limit increase**\.
+ For **Limit type**, choose **Certificate Manager**\.
+ For **Region**, choose the AWS Region where you want to import certificates\.
+ For **Limit**, choose **Number of ACM certificates**\.

Then fill out the rest of the form and submit it\.

## Certificates That You Can Upload to IAM<a name="certificates-to-upload-into-iam"></a>

For the quota \(formerly known as limit\) on the number of certificates that you can upload to IAM, see [IAM and STS Limits](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-limits.html) in the *IAM User Guide*\.

To request a higher quota, [create a case](https://console.aws.amazon.com/support/cases#/create?issueType=service-limit-increase) in the AWS Support Center\. Specify the following values:
+ Accept the default value of **Service limit increase**\.
+ For **Limit type**, choose **Certificate Manager**\.
+ For **Region**, choose the AWS Region where you want to import certificates\.
+ For **Limit**, choose **Server Certificate Limit \(IAM\)**\.

Then fill out the rest of the form and submit it\.

## Certificates That You Can Use with Dedicated IP Addresses<a name="certificates-using-dedicated-ip-address"></a>

For the quota \(formerly known as limit\) on the number of SSL certificates that you can use for each AWS account when serving HTTPS requests using dedicated IP addresses, see [Quotas on SSL Certificates \(Web Distributions Only\)](cloudfront-limits.md#limits-ssl-certificates)\.

To request a higher quota, [create a case](https://console.aws.amazon.com/support/cases#/create?issueType=service-limit-increase) in the AWS Support Center\. Specify the following values:
+ Accept the default value of **Service limit increase**\.
+ For **Limit Type**, choose **CloudFront Distributions**\.
+ For **Limit**, choose **Dedicated IP SSL Certificate Limit per Account**\.

Then fill out the rest of the form and submit it\.