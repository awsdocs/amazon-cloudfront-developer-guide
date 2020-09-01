# Compliance Validation for Amazon CloudFront<a name="compliance"></a>

Third\-party auditors assess the security and compliance of Amazon CloudFront as part of multiple AWS compliance programs\. These include SOC, PCI, HIPAA, and others\. 

For a list of AWS services in scope of specific compliance programs, see [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)\. For general information, see [AWS Compliance Programs](https://aws.amazon.com/compliance/programs/)\. 

You can download third\-party audit reports using AWS Artifact\. For more information, see [Downloading Reports in AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/downloading-documents.html)\. 

Your compliance responsibility when using CloudFront is determined by the sensitivity of your data, your company’s compliance objectives, and applicable laws and regulations\. AWS provides the following resources to help with compliance:
+ [ Security and Compliance Quick Start Guides](https://aws.amazon.com/quickstart/?awsf.quickstart-homepage-filter=categories%23security-identity-compliance) – These deployment guides discuss architectural considerations and provide steps for deploying security\- and compliance\-focused baseline environments on AWS\.
+ [ Architecting for HIPAA Security and Compliance Whitepaper](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf) – This whitepaper describes how companies can use AWS to create HIPAA\-compliant applications\.

  The AWS HIPAA compliance program includes CloudFront as a HIPAA eligible service\. If you have an executed Business Associate Addendum \(BAA\) with AWS, you can use CloudFront to deliver content that contains protected health information \(PHI\)\. For more information, see [HIPAA Compliance\.](https://aws.amazon.com/compliance/hipaa-compliance/) 
+ [AWS Compliance Resources](https://aws.amazon.com/compliance/resources/) – This collection of workbooks and guides might apply to your industry and location\.
+ [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html) – This AWS service assesses how well your resource configurations comply with internal practices, industry guidelines, and regulations\.
+ [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) – This AWS service provides a comprehensive view of your security state within AWS that helps you check your compliance with security industry standards and best practices\.

## CloudFront Compliance Best Practices<a name="compliance"></a>

This section provides best practices and recommendations for compliance when you use Amazon CloudFront to serve your content\.

If you run PCI\-compliant or HIPAA\-compliant workloads that are based on the [AWS shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/), we recommend that you log your CloudFront usage data for the last 365 days for future auditing purposes\. To log usage data, you can do the following:
+ Enable CloudFront access logs\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.
+ Capture requests that are sent to the CloudFront API\. For more information, see [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](logging_using_cloudtrail.md)\.

In addition, see the following for details about how CloudFront is compliant with the PCI DSS and SOC standards\.

### PCI DSS<a name="compliance-pci"></a>

CloudFront supports the processing, storage, and transmission of credit card data by a merchant or service provider, and has been validated as being compliant with Payment Card Industry \(PCI\) Data Security Standard \(DSS\)\. For more information about PCI DSS, including how to request a copy of the AWS PCI Compliance Package, see [PCI DSS Level 1](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)\. 

As a security best practice, we recommend that you don't cache credit card information in CloudFront edge caches\. For example, you can configure your origin to include a `Cache-Control:no-cache="`*field\-name*`"` header in responses that contain credit card information, such as the last four digits of a credit card number and the card owner's contact information\.

### AWS System and Organization Controls<a name="compliance-soc"></a>

CloudFront is compliant with System and Organization Controls \(SOC\) measures, including SOC 1, SOC 2, and SOC 3\. SOC reports are independent, third\-party examination reports that demonstrate how AWS achieves key compliance controls and objectives\. These audits ensure that the appropriate safeguards and procedures are in place to protect against risks that might affect the security, confidentiality, and availability of customer and company data\. The results of these third\-party audits are available on the [AWS SOC Compliance website](https://aws.amazon.com/compliance/soc-faqs/), where you can view the published reports to get more information about the controls that support AWS operations and compliance\.