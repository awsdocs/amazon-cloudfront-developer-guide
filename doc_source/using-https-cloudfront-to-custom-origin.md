# Requiring HTTPS for Communication Between CloudFront and Your Custom Origin<a name="using-https-cloudfront-to-custom-origin"></a>

If you're using an Amazon S3 bucket as your origin, see [Requiring HTTPS for Communication Between CloudFront and Your Amazon S3 Origin](using-https-cloudfront-to-s3-origin.md)\.

If you want to require HTTPS for communication between CloudFront and your custom origin, and you're using the domain name that CloudFront assigned to your distribution in the URLs for your objects \(for example, https://d111111abcdef8\.cloudfront\.net/logo\.jpg\), perform the following procedures to change the **Origin Protocol Policy** setting for the applicable origins in your distribution, and install an SSL/TLS certificate on your custom origin server\.

## Changing CloudFront Settings<a name="using-https-cloudfront-to-origin-distribution-setting"></a>

The following procedure explains how to configure CloudFront to use HTTPS to communicate with an Elastic Load Balancing load balancer, an Amazon EC2 instance, or another custom origin\. For information about using the CloudFront API to update a web distribution, see [PUT Distribution Config](http://docs.aws.amazon.com/cloudfront/latest/APIReference/PutConfig.html) in the *Amazon CloudFront API Reference*\. <a name="using-https-cloudfront-to-custom-origin-procedure"></a>

**To configure CloudFront to require HTTPS between CloudFront and your custom origin**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

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
Choose the **Origin SSL Protocols** for the applicable origins in your distribution\. The SSLv3 protocol is less secure, so we recommend that you choose SSLv3 only if your origin doesn't support TLSv1 or later\.  
The TLSv1 handshake is both backwards and forwards compatible with SSLv3, but TLSv1\.1 and TLSv1\.2 are not\. In this case, the openssl only sends a SSLv3 handshake\.

1. Choose **Yes, Edit**\.

1. Repeat steps 3 through 5 for each additional origin that you want to require HTTPS for between CloudFront and your custom origin\.

1. Confirm the following before you use the updated configuration in a production environment:

   + The path pattern in each cache behavior applies only to the requests that you want viewers to use HTTPS for\.

   + The cache behaviors are listed in the order that you want CloudFront to evaluate them in\. For more information, see [Path Pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.

   + The cache behaviors are routing requests to the origins that you changed the **Origin Protocol Policy** for\. 

## Installing an SSL/TLS Certificate on Your Custom Origin Server<a name="using-https-cloudfront-to-origin-certificate"></a>

You can use an SSL/TLS certificate from the following sources on your custom origin:

+ If your origin is an Elastic Load Balancing load balancer, you can use a certificate provided by AWS Certificate Manager \(ACM\)\. You also can use a certificate that is signed by a trusted third\-party certificate authority and imported into ACM\.

+ For origins other than ELB load balancers, you must use a certificate that is signed by a trusted third\-party certificate authority, for example, Comodo, DigiCert, or Symantec\.

When CloudFront uses HTTPS to communicate with your origin, CloudFront verifies that the certificate was issued by a trusted certificate authority\. CloudFront supports the same certificate authorities as Mozilla; for the current list, see [Mozilla Included CA Certificate List](http://www.mozilla.org/en-US/about/governance/policies/security-group/certs/included/)\. You can't use a self\-signed certificate for HTTPS communication between CloudFront and your origin\.

**Important**  
If the origin server returns an expired certificate, an invalid certificate, or a self\-signed certificate, or if the origin server returns the certificate chain in the wrong order, CloudFront drops the TCP connection, returns HTTP status code 502 \(Bad Gateway\), and sets the `X-Cache` header to `Error from cloudfront`\. Also, if the full chain of certificates, including the intermediate certificate, is not present, CloudFront drops the TCP connection\. 

One of the domain names in the certificate must match one or both of the following values:

+ The value that you specified for **Origin Domain Name** for the applicable origin in your distribution\.

+ If you configured CloudFront to forward the `Host` header to your origin, the value of the `Host` header\. For more information about forwarding headers to your origin, see [Configuring CloudFront to Cache Objects Based on Request Headers](header-caching.md)\.