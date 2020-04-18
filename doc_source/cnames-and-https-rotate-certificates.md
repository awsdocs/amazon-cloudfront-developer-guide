# Rotating SSL/TLS Certificates<a name="cnames-and-https-rotate-certificates"></a>

If you're using certificates provided by AWS Certificate Manager \(ACM\), you don't need to rotate SSL/TLS certificates\. ACM manages certificate renewals for you\. For more information, see [Managed Renewal](https://docs.aws.amazon.com/acm/latest/userguide/acm-renewal.html) in the *AWS Certificate Manager User Guide*\.

**Note**  
ACM does not manage certificate renewals for certificates that you acquire from third\-party certificate authorities and import into ACM\.

If you're using a third\-party certificate authority and you imported certificates into ACM \(recommended\) or uploaded them to the IAM certificate store, you must occasionally replace one certificate with another\. For example, you must replace a certificate when the expiration date on the certificate approaches\.

**Important**  
If you configured CloudFront to serve HTTPS requests by using dedicated IP addresses, you might incur an additional, pro\-rated charge for using one or more additional certificates while you're rotating certificates\. We recommend that you update your distributions promptly to minimize the additional charge\.

To rotate certificates, perform the following procedure\. Viewers can continue to access your content while you rotate certificates as well as after the process is complete\.<a name="rotate-ssl-tls-certificates-proc"></a>

**To rotate SSL/TLS certificates**

1. [Increasing the Quotas for SSL/TLS Certificates](increasing-the-limit-for-ssl-tls-certificates.md) to determine whether you need permission to use more SSL certificates\. If so, request permission and wait until permission is granted before you continue with step 2\.

1. Import the new certificate into ACM or upload it to IAM\. For more information, see [Importing an SSL/TLS Certificate](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cnames-and-https-procedures.html#cnames-and-https-uploading-certificates) in the *Amazon CloudFront Developer Guide*\.

1. Update your distributions one at a time to use the new certificate\. For more information, see [Listing, Viewing, and Updating CloudFront Distributions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToUpdateDistribution.html) in the *Amazon CloudFront Developer Guide*\.

1. \(Optional\) After you have updated all of your CloudFront distributions, you can delete the old certificate from ACM or from IAM\.
**Important**  
Do not delete an SSL/TLS certificate until you remove it from all distributions and until the status of the distributions that you have updated has changed to `Deployed`\.