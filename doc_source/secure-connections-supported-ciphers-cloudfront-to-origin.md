# Supported protocols and ciphers between CloudFront and the origin<a name="secure-connections-supported-ciphers-cloudfront-to-origin"></a>

If you choose to [require HTTPS between CloudFront and your origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesOriginProtocolPolicy), you can decide [which SSL/TLS protocol to allow](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesOriginSSLProtocols) for the secure connection, and CloudFront can connect to the origin using any of the ECDSA or RSA ciphers listed in the following table\. Your origin must support at least one of these ciphers for CloudFront to establish an HTTPS connection to your origin\.

OpenSSL and [s2n](https://github.com/awslabs/s2n) use different names for ciphers than the TLS standards use \([RFC 2246](https://tools.ietf.org/html/rfc2246), [RFC 4346](https://tools.ietf.org/html/rfc4346), [RFC 5246](https://tools.ietf.org/html/rfc5246), and [RFC 8446](https://tools.ietf.org/html/rfc8446)\)\. The following table includes the OpenSSL and s2n name, and the RFC name, for each cipher\.

For ciphers with elliptic curve key exchange algorithms, CloudFront supports the following elliptic curvers:
+ prime256v1
+ secp384r1
+ X25519


| OpenSSL and s2n cipher name | RFC cipher name | 
| --- | --- | 
| Supported ECDSA ciphers | 
| ECDHE\-ECDSA\-AES256\-GCM\-SHA384 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384 | 
| ECDHE\-ECDSA\-AES256\-SHA384 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_CBC\_SHA384 | 
| ECDHE\-ECDSA\-AES256\-SHA | TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_CBC\_SHA | 
| ECDHE\-ECDSA\-AES128\-GCM\-SHA256 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256 | 
| ECDHE\-ECDSA\-AES128\-SHA256 | TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_CBC\_SHA256 | 
| ECDHE\-ECDSA\-AES128\-SHA | TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_CBC\_SHA | 
| Supported RSA ciphers | 
| ECDHE\-RSA\-AES256\-GCM\-SHA384 | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384 | 
| ECDHE\-RSA\-AES256\-SHA384 | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384 | 
| ECDHE\-RSA\-AES256\-SHA | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA | 
| ECDHE\-RSA\-AES128\-GCM\-SHA256 | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256 | 
| ECDHE\-RSA\-AES128\-SHA256 | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256 | 
| ECDHE\-RSA\-AES128\-SHA | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA | 
| AES256\-SHA | TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA | 
| AES128\-SHA | TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA | 
| DES\-CBC3\-SHA | TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA | 
| RC4\-MD5 | TLS\_RSA\_WITH\_RC4\_128\_MD5 | 

**Supported signature schemes between CloudFront and the origin**

CloudFront supports the following signature schemes for connections between CloudFront and the origin\.
+ TLS\_SIGNATURE\_SCHEME\_RSA\_PKCS1\_SHA256
+ TLS\_SIGNATURE\_SCHEME\_RSA\_PKCS1\_SHA384
+ TLS\_SIGNATURE\_SCHEME\_RSA\_PKCS1\_SHA512
+ TLS\_SIGNATURE\_SCHEME\_RSA\_PKCS1\_SHA224
+ TLS\_SIGNATURE\_SCHEME\_ECDSA\_SHA256
+ TLS\_SIGNATURE\_SCHEME\_ECDSA\_SHA384
+ TLS\_SIGNATURE\_SCHEME\_ECDSA\_SHA512
+ TLS\_SIGNATURE\_SCHEME\_ECDSA\_SHA224
+ TLS\_SIGNATURE\_SCHEME\_RSA\_PKCS1\_SHA1
+ TLS\_SIGNATURE\_SCHEME\_ECDSA\_SHA1