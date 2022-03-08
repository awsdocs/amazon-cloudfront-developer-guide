# Requiring HTTPS for communication between CloudFront and your custom origin<a name="using-https-cloudfront-to-custom-origin"></a>

You can require HTTPS for communication between CloudFront and your origin\.

**Note**  
If your origin is an Amazon S3 bucket that’s configured as a website endpoint, you can’t configure CloudFront to use HTTPS with your origin because Amazon S3 doesn’t support HTTPS for website endpoints\.

To require HTTPS between CloudFront and your origin, follow the procedures in this topic to do the following:

1. In your distribution, change the **Origin Protocol Policy** setting for the origin\.

1. Install an SSL/TLS certificate on your origin server \(this isn’t required when you use an Amazon S3 origin or certain other AWS origins\)\.

**Topics**
+ [Changing CloudFront settings](#using-https-cloudfront-to-origin-distribution-setting)
+ [Installing an SSL/TLS certificate on your custom origin](#using-https-cloudfront-to-origin-certificate)

## Changing CloudFront settings<a name="using-https-cloudfront-to-origin-distribution-setting"></a>

The following procedure explains how to configure CloudFront to use HTTPS to communicate with an Elastic Load Balancing load balancer, an Amazon EC2 instance, or another custom origin\. For information about using the CloudFront API to update a distribution, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\. <a name="using-https-cloudfront-to-custom-origin-procedure"></a>

**To configure CloudFront to require HTTPS between CloudFront and your custom origin**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. In the top pane of the CloudFront console, choose the ID for the distribution that you want to update\.

1. On the **Origins** tab, choose the origin that you want to update, and then choose **Edit**\.

1. Update the following settings:  
**Origin Protocol Policy**  
Change the **Origin Protocol Policy** for the applicable origins in your distribution:  
   + **HTTPS Only** – CloudFront uses only HTTPS to communicate with your custom origin\.
   + **Match Viewer** – CloudFront communicates with your custom origin using HTTP or HTTPS, depending on the protocol of the viewer request\. For example, if you choose **Match Viewer** for **Origin Protocol Policy** and the viewer uses HTTPS to request an object from CloudFront, CloudFront also uses HTTPS to forward the request to your origin\.

     Choose **Match Viewer** only if you specify **Redirect HTTP to HTTPS** or **HTTPS Only** for **Viewer Protocol Policy**\.

     CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols\.  
**Origin SSL Protocols**  
Choose the **Origin SSL Protocols** for the applicable origins in your distribution\. The SSLv3 protocol is less secure, so we recommend that you choose SSLv3 only if your origin doesn’t support TLSv1 or later\. The TLSv1 handshake is both backwards and forwards compatible with SSLv3, but TLSv1\.1 and TLSv1\.2 are not\. When you choose SSLv3, CloudFront *only* sends SSLv3 handshake requests\.

1. Choose **Yes, Edit**\.

1. Repeat steps 3 through 5 for each additional origin that you want to require HTTPS for between CloudFront and your custom origin\.

1. Confirm the following before you use the updated configuration in a production environment:
   + The path pattern in each cache behavior applies only to the requests that you want viewers to use HTTPS for\.
   + The cache behaviors are listed in the order that you want CloudFront to evaluate them in\. For more information, see [Path pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.
   + The cache behaviors are routing requests to the origins that you changed the **Origin Protocol Policy** for\. 

## Installing an SSL/TLS certificate on your custom origin<a name="using-https-cloudfront-to-origin-certificate"></a>

You can use an SSL/TLS certificate from the following sources on your custom origin:
+ If your origin is an Elastic Load Balancing load balancer, you can use a certificate provided by AWS Certificate Manager \(ACM\)\. You also can use a certificate that is signed by a trusted third\-party certificate authority and imported into ACM\.
+ For origins other than Elastic Load Balancing load balancers, you must use a certificate that is signed by a trusted third\-party certificate authority \(CA\), for example, Comodo, DigiCert, or Symantec\.

The certificate returned from the origin must include one of the following domain names:
+ The domain name in the origin’s **Origin domain** field \(the `DomainName` field in the CloudFront API\)\.
+ The domain name in the `Host` header, if the cache behavior is configured to forward the `Host` header to the origin\.

When CloudFront uses HTTPS to communicate with your origin, CloudFront verifies that the certificate was issued by a trusted certificate authority\. CloudFront supports the same certificate authorities that Mozilla does\. For the current list, see [Mozilla Included CA Certificate List](https://wiki.mozilla.org/CA/Included_Certificates)\. You can’t use a self\-signed certificate for HTTPS communication between CloudFront and your origin\.

**Important**  
If the origin server returns an expired certificate, an invalid certificate, or a self\-signed certificate, or if the origin server returns the certificate chain in the wrong order, CloudFront drops the TCP connection, returns HTTP status code 502 \(Bad Gateway\) to the viewer, and sets the `X-Cache` header to `Error from cloudfront`\. Also, if the full chain of certificates, including the intermediate certificate, is not present, CloudFront drops the TCP connection\.