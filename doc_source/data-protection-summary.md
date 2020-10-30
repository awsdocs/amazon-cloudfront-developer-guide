# Data Protection in Amazon CloudFront<a name="data-protection-summary"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in Amazon CloudFront\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with CloudFront or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into CloudFront or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

Amazon CloudFront provides several options that you can use to help secure the content that it delivers:
+ Configure HTTPS connections\.
+ Configure field\-level encryption to encrypt data during transit\.
+ Restrict access to content so that only specific people, or people in a specific area, can view it\.

The following topics explain the options in more detail\.

**Topics**
+ [Encryption in Transit](#data-protection-summary-encryption-in-transit)
+ [Encryption at Rest](#data-protection-summary-encryption-at-rest)
+ [Restrict Access to Content](#data-protection-summary-restrict-access)

## Encryption in Transit<a name="data-protection-summary-encryption-in-transit"></a>

To encrypt your data during transit, you configure Amazon CloudFront to require that viewers use HTTPS to request your files, so that connections are encrypted when CloudFront communicates with viewers\. You also can configure CloudFront to use HTTPS to get files from your origin, so that connections are encrypted when CloudFront communicates with your origin\.

For more information, see [Using HTTPS with CloudFront](using-https.md)\.

Field\-level encryption adds an additional layer of security along with HTTPS that lets you protect specific data throughout system processing so that only certain applications can see it\. By configuring field\-level encryption in CloudFront, you can securely upload user\-submitted sensitive information to your web servers\. The sensitive information provided by your clients is encrypted at the edge closer to the user\. It remains encrypted throughout your entire application stack, ensuring that only applications that need the data—and have the credentials to decrypt it—are able to do so\.

For more information, see [Using Field\-Level Encryption to Help Protect Sensitive Data](field-level-encryption.md)\.

## Encryption at Rest<a name="data-protection-summary-encryption-at-rest"></a>

CloudFront uses SSDs which are encrypted for edge location points of presence \(POPs\), and encrypted EBS volumes for Regional Edge Caches \(RECs\)\.

## Restrict Access to Content<a name="data-protection-summary-restrict-access"></a>

Many companies that distribute content over the internet want to restrict access to documents, business data, media streams, or content that is intended for a subset of users\. To securely serve this content by using Amazon CloudFront, you can do one or more of the following:

**Use signed URLs or cookies**  
You can restrict access to content that is intended for selected users—for example, users who have paid a fee—by serving this private content through CloudFront using signed URLs or signed cookies\. For more information, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.

**Restrict access to content in Amazon S3 buckets**  
If you restrict access to your content by using, for example, CloudFront signed URLs or signed cookies, you also won’t want people to view files by using the direct URL for the file\. Instead, you want them to access the files only by using the CloudFront URL, so that your protections work\.  
If you use an Amazon S3 bucket as the origin for a CloudFront distribution, you can set up an origin access identity \(OAI\) to manage direct access to your content\. An origin access identity is a special CloudFront user identity that you can associate with your distribution so that you can secure all or just some of your Amazon S3 content\. For more information about how to configure this, see [Restricting Access to Amazon S3 Content by Using an Origin Access Identity](private-content-restricting-access-to-s3.md)\. 

**Use AWS WAF web ACLs**  
You can use AWS WAF, a web application firewall service, to create a web access control list \(web ACL\) to restrict access to your content\. Based on conditions that you specify, such as the IP addresses that requests originate from or the values of query strings, CloudFront responds to requests either with the requested content or with an HTTP 403 status code \(Forbidden\)\. For more information, see [Using AWS WAF to Control Access to Your Content](distribution-web-awswaf.md)\. 

**Use geo restriction**  
You can use *geo restriction*, also known as *geoblocking*, to prevent users in specific geographic locations from accessing content that you serve through a CloudFront distribution\. There are several options to choose from when you configure geo restrictions\. For more information, see [Restricting the Geographic Distribution of Your Content](georestrictions.md)\.