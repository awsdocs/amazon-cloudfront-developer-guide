# Configuring Alternate Domain Names and HTTPS<a name="cnames-and-https-procedures"></a>

To use alternate domain names in the URLs for your files and to use HTTPS between viewers and CloudFront, perform the applicable procedures\.

**Topics**
+ [Requesting Permission to Use Three or More Dedicated IP SSL/TLS Certificates](#cnames-and-https-multiple-certificates)
+ [Getting an SSL/TLS Certificate](#cnames-and-https-getting-certificates)
+ [Importing an SSL/TLS Certificate](#cnames-and-https-uploading-certificates)
+ [Updating Your CloudFront Distribution](#cnames-and-https-updating-cloudfront)

## Requesting Permission to Use Three or More Dedicated IP SSL/TLS Certificates<a name="cnames-and-https-multiple-certificates"></a>

If you need permission to permanently associate three or more SSL/TLS dedicated IP certificates with CloudFront, perform the following procedure\. For more details about HTTPS requests, see [Choosing How CloudFront Serves HTTPS Requests](cnames-https-dedicated-ip-or-sni.md)\.

**Note**  
This procedure is for using three or more dedicated IP certificates across your CloudFront distributions\. The default value is 2\. Keep in mind you cannot bind more than one SSL certificate to a distribution\.  
You can only associate a single SSL/TLS certificate to a CloudFront distribution at a time\. This number is for the total number of dedicated IP SSL certificates you can use across all of your CloudFront distributions\.<a name="cnames-and-https-multiple-certificates-procedure"></a>

**To request permission to use three or more certificates with a CloudFront distribution**

1. Go to the [Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-cloudfront-distributions) and create a case\. 

1. Indicate how many certificates you need permission to use, and describe the circumstances in your request\. We'll update your account as soon as possible\. 

1. Continue with the next procedure\.

## Getting an SSL/TLS Certificate<a name="cnames-and-https-getting-certificates"></a>

Get an SSL/TLS certificate if you don’t already have one\. For more information, see the applicable documentation:
+ To use a certificate provided by AWS Certificate Manager \(ACM\), see the [AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)\. Then skip to [Updating Your CloudFront Distribution](#cnames-and-https-updating-cloudfront)\.
**Note**  
We recommend that you use ACM to provision, manage, and deploy SSL/TLS certificates on AWS managed resources\. You must request an ACM certificate in the US East \(N\. Virginia\) Region\.
+ To get a certificate from a third\-party certificate authority \(CA\), see the documentation provided by the certificate authority\. When you have the certificate, continue with the next procedure\.
+ To create a self\-signed certificate, see the documentation for the application that you're using to create and sign the certificate\. Then continue with the next procedure\.

## Importing an SSL/TLS Certificate<a name="cnames-and-https-uploading-certificates"></a>

If you got your certificate from a third\-party CA, import the certificate into ACM or upload it to the IAM certificate store:

**ACM \(Recommended\)**  
ACM lets you import third\-party certificates from the ACM console, as well as programmatically\. For information about importing a certificate to ACM, see [Importing Certificates into AWS Certificate Manager](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate.html) in the *AWS Certificate Manager User Guide*\. You must import the certificate in the US East \(N\. Virginia\) Region\.

**IAM certificate store**  
If ACM is not available in your Region, use the following AWS CLI command to upload your third\-party certificate to the IAM certificate store\. \(For a list of the Regions where ACM is available, see [AWS Certificate Manager](https://docs.aws.amazon.com/general/latest/gr/rande.html#acm_region) in the "AWS Regions and Endpoints" chapter of the *Amazon Web Services General Reference*\.\)  

```
aws iam upload-server-certificate --server-certificate-name CertificateName --certificate-body file://public_key_certificate_file --private-key file://privatekey.pem --certificate-chain file://certificate_chain_file --path /cloudfront/path/
```
Note the following:  
+ **AWS account** – You must upload the certificate to the IAM certificate store using the same AWS account that you used to create your CloudFront distribution\.
+ **\-\-path parameter** – When you upload the certificate to IAM, the value of the `--path` parameter \(certificate path\) must start with `/cloudfront/`, for example, `/cloudfront/production/` or `/cloudfront/test/`\. The path must end with a /\.
+ **Existing certificates** – You must specify values for the `--server-certificate-name` and `--path` parameters that are different from the values that are associated with existing certificates\.
+ **Using the CloudFront console** – The value that you specify for the `--server-certificate-name` parameter in the AWS CLI, for example, `myServerCertificate`, appears in the **SSL Certificate** list in the CloudFront console\.
+ **Using the CloudFront API** – Make note of the alphanumeric string that the AWS CLI returns, for example, `AS1A2M3P4L5E67SIIXR3J`\. This is the value that you will specify in the `IAMCertificateId` element\. You don't need the IAM ARN, which is also returned by the CLI\.
For more information about the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) and the [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)\.

## Updating Your CloudFront Distribution<a name="cnames-and-https-updating-cloudfront"></a>

To update settings for your distribution, perform the following procedure:<a name="cnames-and-https-updating-cloudfront-procedure"></a>

**To configure your CloudFront distribution for alternate domain names**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID for the distribution that you want to update\.

1. On the **General** tab, choose **Edit**\.

1. Update the following values:  
**Alternate Domain Names \(CNAMEs\)**  
Add the applicable alternate domain names\. Separate domain names with commas, or type each domain name on a new line\.  
**SSL Certificate \(Web Distributions Only\)**  
Choose **Custom SSL Certificate**, and choose a certificate from the list\.  
Up to 100 certificates are listed here\. If you have more than 100 certificates and you don't see the certificate that you want to add, you can type a certificate ARN in the field to choose it\.  
If you uploaded a certificate to the IAM certificate store but it's not listed, and you can't choose it by typing the name in the field, review the procedure [Importing an SSL/TLS Certificate](#cnames-and-https-uploading-certificates) to confirm that you correctly uploaded the certificate\.   
After you associate your SSL/TLS certificate with your CloudFront distribution, do not delete the certificate from ACM or the IAM certificate store until you remove the certificate from all distributions and until the status of the distributions has changed to **Deployed**\.  
**Clients Supported \(Web Distributions Only\)**  
Choose the applicable option:  
   + **All clients**: CloudFront serves your HTTPS content using dedicated IP addresses\. If you select this option, you incur additional charges when you associate your SSL/TLS certificate with a distribution that is enabled\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing)\.
   + **Only clients that Support Server Name Indication \(SNI\)**: Older browsers or other clients that don't support SNI must use another method to access your content\.
For more information, see [Choosing How CloudFront Serves HTTPS Requests](cnames-https-dedicated-ip-or-sni.md)\.

1. Choose **Yes, Edit**\.

1. Configure CloudFront to require HTTPS between viewers and CloudFront:

   1. On the **Behaviors** tab, choose the cache behavior that you want to update, and choose **Edit**\.

   1. Specify one of the following values for **Viewer Protocol Policy**:  
**Redirect HTTP to HTTPS**  
Viewers can use both protocols, but HTTP requests are automatically redirected to HTTPS requests\. CloudFront returns HTTP status code `301 (Moved Permanently)` along with the new HTTPS URL\. The viewer then resubmits the request to CloudFront using the HTTPS URL\.  
CloudFront doesn't redirect `DELETE`, `OPTIONS`, `PATCH`, `POST`, or `PUT` requests from HTTP to HTTPS\. If you configure a cache behavior to redirect to HTTPS, CloudFront responds to HTTP `DELETE`, `OPTIONS`, `PATCH`, `POST`, or `PUT` requests for that cache behavior with HTTP status code `403 (Forbidden)`\.
When a viewer makes an HTTP request that is redirected to an HTTPS request, CloudFront charges for both requests\. For the HTTP request, the charge is only for the request and for the headers that CloudFront returns to the viewer\. For the HTTPS request, the charge is for the request, and for the headers and the file returned by your origin\.  
**HTTPS Only**  
Viewers can access your content only if they're using HTTPS\. If a viewer sends an HTTP request instead of an HTTPS request, CloudFront returns HTTP status code `403 (Forbidden)` and does not return the file\.

   1. Choose **Yes, Edit**\.

   1. Repeat steps a through c for each additional cache behavior that you want to require HTTPS for between viewers and CloudFront\.

1. Confirm the following before you use the updated configuration in a production environment:
   + The path pattern in each cache behavior applies only to the requests that you want viewers to use HTTPS for\.
   + The cache behaviors are listed in the order that you want CloudFront to evaluate them in\. For more information, see [Path Pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.
   + The cache behaviors are routing requests to the correct origins\. 