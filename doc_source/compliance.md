# CloudFront Compliance<a name="compliance"></a>

For information about CloudFront compliance with various security compliance regulations and audits standards, see the following pages:
+ [AWS Cloud Compliance](https://aws.amazon.com/compliance/)
+ [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)

If you run PCI or HIPAA\-compliant workloads, based on the [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/), we recommend that you log your CloudFront usage data for the last 365 days for future auditing purposes\. To log usage data, you can do the following:
+ Enable CloudFront access logs\. For more information, see [Access Logs](AccessLogs.md)\.
+ Capture requests that are sent to the CloudFront API\. For more information, see [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](logging_using_cloudtrail.md)\.

In addition, see the following for details about how CloudFront is compliant with the PCI DSS and HIPAA standards\.

## PCI DSS<a name="compliance-pci"></a>

CloudFront supports the processing, storage, and transmission of credit card data by a merchant or service provider, and has been validated as being compliant with Payment Card Industry \(PCI\) Data Security Standard \(DSS\)\. For more information about PCI DSS, including how to request a copy of the AWS PCI Compliance Package, see [PCI DSS Level 1](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)\. 

As a security best practice, we recommend that you don't cache credit card information in CloudFront edge caches\. For example, you can configure your origin to include a `Cache-Control:no-cache="`*field\-name*`"` header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information\.

## HIPAA<a name="compliance-hipaa"></a>

The AWS HIPAA compliance program includes CloudFront as a HIPAA Eligible Service\. If you have an executed Business Associate Agreement \(BAA\) with AWS, you can use CloudFront to deliver content containing protected health information \(PHI\)\. For more information, see [HIPAA Compliance\.](https://aws.amazon.com/compliance/hipaa-compliance/) 