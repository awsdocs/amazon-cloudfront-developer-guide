# Requiring HTTPS for Communication Between CloudFront and Your Custom Origin<a name="using-https-cloudfront-to-custom-origin"></a>

If you want to require HTTPS for communication between CloudFront and your custom origin, the steps you take depend on whether you're using the domain name that CloudFront assigned to your distribution \(like d111111abcdef8\.cloudfront\.net\) or your own alternate domain name \(like example\.com\)\. 

**Note**  
If your custom origin is an Amazon S3 bucket that’s configured as a website endpoint, you can’t configure CloudFront to use HTTPS with your origin because Amazon S3 doesn’t support HTTPS for website endpoints\.

**Use the default CloudFront domain name**  
If you're using the domain name that CloudFront assigned to your distribution in the URLs for your objects \(for example, https://d111111abcdef8\.cloudfront\.net/logo\.jpg\), you can require HTTPS by following the procedures in this topic to do the following:  
+ Change the **Origin Protocol Policy** setting for specific origins in your distribution
+ Install an SSL/TLS certificate on your custom origin server \(this isn't required when you use an Amazon S3 origin\)

**Use an alternate domain name**  
Instead of using the default domain name with your distribution, you can add an alternate domain name that's easier to work with, like example\.com\.   
To require HTTPS for communication when you use an alternate domain name, follow the steps and guidance in [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\. 

**Topics**
+ [Changing CloudFront Settings](#using-https-cloudfront-to-origin-distribution-setting)
+ [Installing an SSL/TLS Certificate on Your Custom Origin Server](#using-https-cloudfront-to-origin-certificate)
+ [About RSA and ECDSA Ciphers](#using-https-cloudfront-to-origin-about-ciphers)

## Changing CloudFront Settings<a name="using-https-cloudfront-to-origin-distribution-setting"></a>

The following procedure explains how to configure CloudFront to use HTTPS to communicate with an Elastic Load Balancing load balancer, an Amazon EC2 instance, or another custom origin\. For information about using the CloudFront API to update a web distribution, see [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html) in the *Amazon CloudFront API Reference*\. <a name="using-https-cloudfront-to-custom-origin-procedure"></a>

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
Choose the **Origin SSL Protocols** for the applicable origins in your distribution\. The SSLv3 protocol is less secure, so we recommend that you choose SSLv3 only if your origin doesn’t support TLSv1 or later\. The TLSv1 handshake is both backwards and forwards compatible with SSLv3, but TLSv1\.1 and TLSv1\.2 are not\. When you choose SSLv3, CloudFront *only* sends SSLv3 handshake requests\.

1. Choose **Yes, Edit**\.

1. Repeat steps 3 through 5 for each additional origin that you want to require HTTPS for between CloudFront and your custom origin\.

1. Confirm the following before you use the updated configuration in a production environment:
   + The path pattern in each cache behavior applies only to the requests that you want viewers to use HTTPS for\.
   + The cache behaviors are listed in the order that you want CloudFront to evaluate them in\. For more information, see [Path Pattern](distribution-web-values-specify.md#DownloadDistValuesPathPattern)\.
   + The cache behaviors are routing requests to the origins that you changed the **Origin Protocol Policy** for\. 

## Installing an SSL/TLS Certificate on Your Custom Origin Server<a name="using-https-cloudfront-to-origin-certificate"></a>

You can use an SSL/TLS certificate from the following sources on your custom origin:
+ If your origin is an Elastic Load Balancing load balancer, you can use a certificate provided by AWS Certificate Manager \(ACM\)\. You also can use a certificate that is signed by a trusted third\-party certificate authority and imported into ACM\.
+ For origins other than Elastic Load Balancing load balancers, you must use a certificate that is signed by a trusted third\-party certificate authority \(CA\), for example, Comodo, DigiCert, or Symantec\.

When CloudFront uses HTTPS to communicate with your origin, CloudFront verifies that the certificate was issued by a trusted certificate authority\. CloudFront supports the same certificate authorities that Mozilla does\. For the current list, see [Mozilla Included CA Certificate List](https://wiki.mozilla.org/CA/Included_Certificates)\. You can't use a self\-signed certificate for HTTPS communication between CloudFront and your origin\.

**Important**  
If the origin server returns an expired certificate, an invalid certificate, or a self\-signed certificate, or if the origin server returns the certificate chain in the wrong order, CloudFront drops the TCP connection, returns HTTP status code 502 \(Bad Gateway\), and sets the `X-Cache` header to `Error from cloudfront`\. Also, if the full chain of certificates, including the intermediate certificate, is not present, CloudFront drops the TCP connection\. 

The certificate returned from the origin must cover the domain that you specified for **Origin Domain Name** for the corresponding origin in your distribution\. In addition, if you configured CloudFront to forward the `Host` header to your origin, the origin must respond with a certificate matching the domain in the `Host` header\.

## About RSA and ECDSA Ciphers<a name="using-https-cloudfront-to-origin-about-ciphers"></a>

The encryption strength of a communications connection depends on the key size and strength of the algorithm that you choose for your origin server’s certificate\. The two options that CloudFront supports for connections with a custom origin are RSA and Elliptic Curve Digital Signature Algorithm \(ECDSA\)\.

For lists of the RSA and ECDSA ciphers supported by CloudFront, see [Supported protocols and ciphers between CloudFront and the origin](secure-connections-supported-ciphers-cloudfront-to-origin.md)\.

### How RSA Ciphers Work<a name="using-https-cloudfront-to-origin-which-cipher-rsa"></a>

CloudFront and origin servers typically use RSA 2048\-bit asymmetric keys for SSL/TLS termination\. RSA algorithms use the product of two large prime numbers, with another number added to it to create a public key\. The private key is a related number\. The strength of RSA relies on the presumed difficulty of breaking a key that requires factoring the product of two large prime numbers\. However, improvements in computer technology have weakened RSA algorithms because faster computer calculations mean that it’s now easier to break the encryption\.

If you want to maintain encryption strength while continuing to use RSA, one option would be to increase the size of your RSA keys\. However, this approach isn’t easily scalable because using larger keys increases the compute cost for cryptography\.

### How ECDSA Ciphers Work<a name="using-https-cloudfront-to-origin-which-cipher-ecdsa"></a>

Alternatively, you could use an ECDSA certificate\. ECDSA bases its security on a more complex mathematical problem than RSA that is harder to solve, which means that it takes more computer processing time to break ECDSA encryption\. ECDSA is built on the principle that it is difficult to solve for the discrete logarithm of a random elliptic curve when its base is known, also known as the Elliptic Curve Discrete Logarithm Problem \(ECDLP\)\. This means that you can use shorter key lengths to achieve the equivalent security of using RSA with much larger key sizes\.

In addition to providing better security, using ECDSA's smaller keys enables faster computing of algorithms, smaller digital certificates, and fewer bits to transmit during the SSL/TLS handshake\. As a result, the smaller keys reduce the time that it takes for you to create and sign digital certificates for SSL/TLS termination on origin servers\. Using a smaller key size therefore can increase throughput by reducing the compute cycles needed for cryptography, freeing up server resources to process other work\.

### Choosing Between RSA and ECDSA Ciphers<a name="using-https-cloudfront-to-origin-which-cipher-choosing"></a>

Sample tests that we have run to compare, for example, 2048\-bit RSA to 256\-bit ECDSA \(nistp256\) have indicated that the nistp256 option was 95% faster than 2048\-bit RSA while providing the same security strength as 3072\-bit RSA\.

CloudFront continues to support RSA for SSL/TLS connections\. However, if you have concerns about the strength of your current encryption for SSL/TLS authentication for your origin servers, ECDSA could be a better option\. The effort to enable ECDSA digital certificates compared to the security benefit that ECDSA brings is a trade\-off that you will have to weigh in making your decision\. In addition to enabling stronger encryption, the reduction in computational cost of cryptography while using ECDSA at your origin servers is an added advantage\.

### Using ECDSA Ciphers<a name="using-https-cloudfront-to-origin-switch-ciphers"></a>

To use ECDSA for communications between CloudFront and your origin, do the following: 

1. Generate a private key by using one of the supported curves \(prime256v1, secp384r1, or X25519\)\.

1. Generate an ECDSA Digital Certificate in the X\.509 PEM format with a trusted certificate authority\.

1. Set up your origin to prefer the ECDSA certificate\.

Using ECDSA doesn't require any settings changes in the CloudFront console or APIs, and there is no additional fee\.