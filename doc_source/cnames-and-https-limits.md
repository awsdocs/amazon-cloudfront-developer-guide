# Quotas on Using SSL/TLS Certificates with CloudFront \(HTTPS Between Viewers and CloudFront Only\)<a name="cnames-and-https-limits"></a>

Note the following quotas \(formerly known as limits\) on using SSL/TLS certificates with CloudFront\. These quotas apply only to the SSL/TLS certificates that you provision by using AWS Certificate Manager \(ACM\), that you import into ACM, or upload to the IAM certificate store for HTTPS communication between viewers and CloudFront\.

**Maximum Number of Certificates per CloudFront Distribution**  
You can associate a maximum of one SSL/TLS certificate with each CloudFront distribution\.

**Maximum Number of Certificates that You Can Import into ACM or Upload to the IAM Certificate Store**  
If you obtained your SSL/TLS certificates from a third\-party CA, you must store the certificates in one of the following locations:  
+ **AWS Certificate Manager** – For the current quota on the number of ACM certificates, see [Quotas](https://docs.aws.amazon.com/acm/latest/userguide/acm-limits.html) in the *AWS Certificate Manager User Guide*\. The listed quota is a total that includes certificates that you provision by using ACM and certificates that you import into ACM\.
+ **IAM certificate store** – For the current quota \(formerly known as limit\) on the number of certificates that you can upload to the IAM certificate store for an AWS account, see [IAM and STS Limits](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-limits.html) in the *IAM User Guide*\. You can [request a higher quota in the AWS Management Console](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-iam-groups-and-users)\.

**Maximum Number of Certificates per AWS Account \(Dedicated IP Addresses Only\)**  
If you want to serve HTTPS requests by using dedicated IP addresses, note the following:  
+ By default, CloudFront gives you permission to use two certificates with your AWS account, one for everyday use and one for when you need to rotate certificates for multiple distributions\.
+ If you need more than two custom SSL/TLS certificates for your AWS account, go to the [Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) and create a case\. Indicate how many certificates that you need permission to use, and describe the circumstances in your request\. We'll update your account as soon as possible\. 

**Using the Same Certificate for CloudFront Distributions that Were Created by Using Different AWS Accounts**  
If you're using a third\-party CA and you want to use the same certificate with multiple CloudFront distributions that were created by using different AWS accounts, you must import the certificate into ACM or upload it to the IAM certificate store once for each AWS account\.  
If you're using certificates provided by ACM, you can't configure CloudFront to use certificates that were created by a different AWS account\.

**Using the Same Certificate for CloudFront and for Other AWS Services \(IAM Certificate Store Only\) **  
If you bought a certificate from a trusted certificate authority such as Comodo, DigiCert, or Symantec, you can use the same certificate for CloudFront and for other AWS services\. If you're importing the certificate into ACM, you need to import it only once to use it for multiple AWS services\.  
If you're using certificates provided by ACM, the certificates are stored in ACM\.

**Using the Same Certificate for Multiple CloudFront Distributions**  
You can use the same certificate for any or all of the CloudFront distributions that you're using to serve HTTPS requests\. Note the following:  
+ You can use the same certificate both for serving requests using dedicated IP addresses and for serving requests using SNI\. 
+ You can associate only one certificate with each distribution\.
+ Each distribution must include one or more alternate domain names that also appear in the **Common Name** field or the **Subject Alternative Names** field in the certificate\.
+ If you're serving HTTPS requests using dedicated IP addresses and you created all of your distributions by using the same AWS account, you can significantly reduce your cost by using the same certificate for all distributions\. CloudFront charges for each certificate, not for each distribution\. 

  For example, suppose you create three distributions by using the same AWS account, and you use the same certificate for all three distributions\. You would be charged only one fee for using dedicated IP addresses\.

  However, if you're serving HTTPS requests using dedicated IP addresses and using the same certificate to create CloudFront distributions in different AWS accounts, each account is charged the fee for using dedicated IP addresses\. For example, if you create three distributions by using three different AWS accounts and you use the same certificate for all three distributions, each account is charged the full fee for using dedicated IP addresses\.