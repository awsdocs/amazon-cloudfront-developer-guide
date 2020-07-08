# Supported protocols and ciphers<a name="secure-connections-supported-viewer-protocols-ciphers"></a>

You can choose HTTPS settings both for communication between viewers and CloudFront, and between CloudFront and your origin:
+ **Between viewers and CloudFront** – If you require HTTPS between viewers and CloudFront, you also choose a security policy, which determines the protocols that viewers and CloudFront can use to communicate\. In addition, a security policy determines which ciphers CloudFront can use to encrypt the content that it returns to viewers\.
+ **Between CloudFront and your origin** – If you require HTTPS between CloudFront and your origin, you also choose the protocols that CloudFront and your origin use to communicate\. The protocols that you choose determine which ciphers your origin can use to encrypt content that it returns to CloudFront\.

**Topics**
+ [Supported SSL/TLS protocols and ciphers for communication between viewers and CloudFront](#secure-connections-supported-ciphers)
+ [Supported SSL/TLS protocols and ciphers for communication between CloudFront and your origin](#secure-connections-supported-ciphers-cloudfront-to-origin)

## Supported SSL/TLS protocols and ciphers for communication between viewers and CloudFront<a name="secure-connections-supported-ciphers"></a>

To choose whether to require HTTPS between viewers and CloudFront, specify the applicable value for [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\.

If you choose to require HTTPS, you also choose the security policy that you want CloudFront to use for HTTPS connections\. A security policy determines two settings:
+ The minimum SSL/TLS protocol that CloudFront uses to communicate with viewers\.
+ The ciphers that CloudFront can use to encrypt the content that it returns to viewers\.

To choose a security policy, specify the applicable value for [Security Policy](distribution-web-values-specify.md#DownloadDistValues-security-policy)\. The following table lists the protocols and ciphers that CloudFront can use for each security policy\. 

A viewer must support at least one of the supported ciphers to establish an HTTPS connection with CloudFront\. If you’re using an SSL/TLS certificate in AWS Certificate Manager, a viewer must support one of the \*\-RSA\-\* ciphers\. CloudFront chooses a cipher in the listed order from among the ciphers that the viewer supports\. See also [OpenSSL and RFC cipher names](#secure-connections-openssl-rfc-cipher-names)\.


****  

|  | Security policy |  | SSLv3 | TLSv1 | TLSv1\_2016 | TLSv1\.1\_2016 | TLSv1\.2\_2018 | TLSv1\.2\_2019 | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| SSL/TLS protocols supported | 
| TLSv1\.2 | ♦ | ♦ | ♦ | ♦ | ♦ | ♦ | 
| TLSv1\.1 | ♦ | ♦ | ♦ | ♦ |  |  | 
| TLSv1 | ♦ | ♦ | ♦ |  |  |  | 
| SSLv3 | ♦ |  |  |  |  |  | 
| Ciphers supported | 
| ECDHE\-RSA\-AES128\-GCM\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES128\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES128\-SHA | ♦ | ♦ | ♦ | ♦ |  |  | 
| ECDHE\-RSA\-AES256\-GCM\-SHA384 | ♦ | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES256\-SHA384 | ♦ | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES256\-SHA | ♦ | ♦ | ♦ | ♦ |  |  | 
| AES128\-GCM\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ |  | 
| AES256\-GCM\-SHA384 | ♦ | ♦ | ♦ | ♦ | ♦ |  | 
| AES128\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ |  | 
| AES256\-SHA | ♦ | ♦ | ♦ | ♦ |  |  | 
| AES128\-SHA | ♦ | ♦ | ♦ | ♦ |  |  | 
| DES\-CBC3\-SHA | ♦ | ♦ |  |  |  |  | 
| RC4\-MD5 | ♦ |  |  |  |  |  | 

### OpenSSL and RFC cipher names<a name="secure-connections-openssl-rfc-cipher-names"></a>

OpenSSL and IETF RFC 5246, [The Transport Layer Security \(TLS\) Protocol Version 1\.2](https://tools.ietf.org/html/rfc5246), use different names for the same ciphers\. The following table maps the OpenSSL name to the RFC name for each cipher\.


****  

| OpenSSL cipher name | RFC cipher name | 
| --- | --- | 
|  ECDHE\-RSA\-AES128\-GCM\-SHA256  |  TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256  | 
|  ECDHE\-RSA\-AES128\-SHA256  |  TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256   | 
|  ECDHE\-RSA\-AES128\-SHA  |  TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA  | 
|  ECDHE\-RSA\-AES256\-GCM\-SHA384  |  TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384   | 
|  ECDHE\-RSA\-AES256\-SHA384  |  TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384   | 
|  ECDHE\-RSA\-AES256\-SHA  |  TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA  | 
|  AES128\-GCM\-SHA256  |  TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256  | 
|  AES256\-GCM\-SHA384  |  TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384  | 
|  AES128\-SHA256  |  TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256  | 
|  AES256\-SHA  |  TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA  | 
|  AES128\-SHA  |  TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA  | 
|  DES\-CBC3\-SHA   |  TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA   | 
|  RC4\-MD5  |  TLS\_RSA\_WITH\_RC4\_128\_MD5  | 

## Supported SSL/TLS protocols and ciphers for communication between CloudFront and your origin<a name="secure-connections-supported-ciphers-cloudfront-to-origin"></a>

If you choose to [require HTTPS between CloudFront and your origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesOriginProtocolPolicy), you can decide [which SSL/TLS protocol to allow](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html#DownloadDistValuesOriginSSLProtocols) for the secure connection, and then pick any supported cipher for CloudFront \(see the following tables\) to establish an HTTPS connection to your origin\.

CloudFront can forward HTTPS requests to the origin server by using the ECDSA or RSA ciphers listed in this section\. Your origin server must support at least one of these ciphers for CloudFront to establish an HTTPS connection to your origin\. To learn more about the two types of ciphers that CloudFront supports, see [ About RSA and ECDSA Ciphers](using-https-cloudfront-to-custom-origin.md#using-https-cloudfront-to-origin-about-ciphers)\.

**Note**  
The following curves are supported for elliptic\-curve\-based ciphers:  
prime256v1
secp384r1

OpenSSL and IETF RFC 5246, [The Transport Layer Security \(TLS\) Protocol Version 1\.2](https://tools.ietf.org/html/rfc5246), use different names for the same ciphers\. The following tables map the OpenSSL name to the RFC name for each cipher\. 

**Supported RSA ciphers**

CloudFront supports the following RSA ciphers for connections with an origin:


****  

| OpenSSL cipher name | RFC cipher name | 
| --- | --- | 
|  ECDHE\-RSA\-AES128\-SHA256  |  TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256   | 
|  ECDHE\-RSA\-AES256\-SHA384  |  TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384   | 
|  AES256\-SHA  |  TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA  | 
|  AES128\-SHA  |  TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA  | 
|  DES\-CBC3\-SHA   |  TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA   | 
|  RC4\-MD5   |  TLS\_RSA\_WITH\_RC4\_128\_MD5  | 

**Supported ECDSA ciphers**

CloudFront supports the following ECDSA ciphers for connections with an origin:


****  

| OpenSSL cipher name | RFC cipher name | 
| --- | --- | 
|  ECDHE\-ECDSA\-AES256\-GCM\-SHA384  |  TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384  | 
|  ECDHE\-ECDSA\-AES256\-SHA384  |  TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_CBC\_SHA384  | 
|  ECDHE\-ECDSA\-AES256\-SHA  |  TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_CBC\_SHA  | 
|  ECDHE\-ECDSA\-AES128\-GCM\-SHA256  |  TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256  | 
|  ECDHE\-ECDSA\-AES128\-SHA256  |  TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_CBC\_SHA256  | 
|  ECDHE\-ECDSA\-AES128\-SHA  |  TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_CBC\_SHA  | 