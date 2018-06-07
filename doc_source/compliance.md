# CloudFront Compliance<a name="compliance"></a>

For information about CloudFront compliance with various security compliance regulations and audits standards, see the following pages:
+ [AWS Cloud Compliance](https://aws.amazon.com/compliance/)
+ [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)

If you run PCI or HIPAA\-compliant workloads, based on the [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/), we recommend that you log your CloudFront usage data for the last 365 days for future auditing purposes\. To log usage data, you can do the following:
+ Enable CloudFront access logs\. For more information, see [Access Logs](AccessLogs.md)\.
+ Capture requests that are sent to the CloudFront API\. For more information, see [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](logging_using_cloudtrail.md)\.

In addition, see the following for details about how CloudFront is compliant with the PCI DSS, HIPAA, and SOC standards\.

## PCI DSS<a name="compliance-pci"></a>

CloudFront supports the processing, storage, and transmission of credit card data by a merchant or service provider, and has been validated as being compliant with Payment Card Industry \(PCI\) Data Security Standard \(DSS\)\. For more information about PCI DSS, including how to request a copy of the AWS PCI Compliance Package, see [PCI DSS Level 1](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)\. 

As a security best practice, we recommend that you don't cache credit card information in CloudFront edge caches\. For example, you can configure your origin to include a `Cache-Control:no-cache="`*field\-name*`"` header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information\.

## HIPAA<a name="compliance-hipaa"></a>

The AWS HIPAA compliance program includes CloudFront as a HIPAA Eligible Service\. If you have an executed Business Associate Agreement \(BAA\) with AWS, you can use CloudFront to deliver content containing protected health information \(PHI\)\. For more information, see [HIPAA Compliance\.](https://aws.amazon.com/compliance/hipaa-compliance/) 

## SOC 1, SOC 2, and SOC 3<a name="compliance-soc"></a>

Amazon CloudFront is compliant with SOC \(System and Organization Control\) measures\. SOC Reports are independent third\-party examination reports that demonstrate how AWS achieves key compliance controls and objectives\. These audits ensure that the appropriate safeguards and procedures are in place to protect against risks that may affect the security, confidentiality, and availability of customer and company data\. The results of these third\-party audits are available on the [AWS SOC Compliance website](https://aws.amazon.com/compliance/soc-faqs/), where customers can view the published reports to get more information about the controls established to support AWS operations and compliance\.