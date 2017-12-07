# Supported Protocols and Ciphers<a name="secure-connections-supported-viewer-protocols-ciphers"></a>

You can choose HTTPS settings both for communication between viewers and CloudFront, and between CloudFront and your origin:

+ **Between viewers and CloudFront** – If you require HTTPS between viewers and CloudFront, you also choose a security policy, which determines the protocols that viewers and CloudFront can use to communicate\. In addition, a security policy determines which ciphers CloudFront can use to encrypt the content that it returns to viewers\.

+ **Between CloudFront and your origin** – If you require HTTPS between CloudFront and your origin, you also choose the protocols that CloudFront and your origin use to communicate\. The protocols that you choose determine which ciphers your origin can use to encrypt content that it returns to CloudFront

See the applicable topic\.


+ [Supported SSL/TLS Protocols and Ciphers for Communication Between Viewers and CloudFront](#secure-connections-supported-ciphers)
+ [Supported SSL/TLS Protocols and Ciphers for Communication Between CloudFront and Your Origin](#secure-connections-supported-ciphers-cloudfront-to-origin)

## Supported SSL/TLS Protocols and Ciphers for Communication Between Viewers and CloudFront<a name="secure-connections-supported-ciphers"></a>

To choose whether to require HTTPS between viewers and CloudFront, specify the applicable value for [Viewer Protocol Policy](distribution-web-values-specify.md#DownloadDistValuesViewerProtocolPolicy)\.

If you choose to require HTTPS, you also choose the security policy that you want CloudFront to use for HTTPS connections\. A security policy determines two settings:

+ The SSL/TLS protocol that CloudFront uses to communicate with viewers

+ The cipher that CloudFront uses to encrypt the content that it returns to viewers

We recommend that you specify **TLSv1\.1\_2016** unless your users are using browsers or devices that don't support TLSv1\.1 or later\. When you use a custom SSL certificate and SNI, you must use TLSv1 or later\.

To choose a security policy, specify the applicable value for [Security Policy](distribution-web-values-specify.md#DownloadDistValues-security-policy)\. The following table lists the protocols and ciphers that CloudFront can use for each security policy\. 

A viewer must support at least one of the supported ciphers to establish an HTTPS connection with CloudFront\. If you're using an SSL/TLS certificate in AWS Certificate Manager, a viewer must support one of the \*\-RSA\-\* ciphers\. CloudFront chooses a cipher in the listed order from among the ciphers that the viewer supports\. See also [OpenSSL and RFC Cipher Names](#secure-connections-openssl-rfc-cipher-names)\.


****  

|  | Security Policy |  | SSLv3 | TLSv1\.0 | TLSv1\_2016 | TLSv1\.1\_2016 | TLSv1\.2\_2018 | SSL/TLS Protocols Supported | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| TLSv1\.2 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| TLSv1\.1 | ♦ | ♦ | ♦ | ♦ |  | 
| TLSv1 | ♦ | ♦ | ♦ |  |  | 
| SSLv3 | ♦ |  |  |  |  | 
| Ciphers Supported | 
| ECDHE\-RSA\-AES128\-GCM\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES128\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES128\-SHA | ♦ | ♦ | ♦ | ♦ |  | 
| ECDHE\-RSA\-AES256\-GCM\-SHA384 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES256\-SHA384 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| ECDHE\-RSA\-AES256\-SHA | ♦ | ♦ | ♦ | ♦ |  | 
| AES128\-GCM\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| AES256\-GCM\-SHA384 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| AES128\-SHA256 | ♦ | ♦ | ♦ | ♦ | ♦ | 
| AES256\-SHA | ♦ | ♦ | ♦ | ♦ |  | 
| AES128\-SHA | ♦ | ♦ | ♦ | ♦ |  | 
| DES\-CBC3\-SHA  | ♦ | ♦ |  |  |  | 
| RC4\-MD5 | ♦ |  |  |  |  | 

### OpenSSL and RFC Cipher Names<a name="secure-connections-openssl-rfc-cipher-names"></a>

OpenSSL and IETF RFC 5246, [The Transport Layer Security \(TLS\) Protocol Version 1\.2](https://tools.ietf.org/html/rfc5246), use different names for the same ciphers\. The following table maps the OpenSSL name to the RFC name for each cipher that CloudFront supports\. 


****  

| OpenSSL Cipher Name | RFC Cipher Name | 
| --- | --- | 
| ECDHE\-RSA\-AES128\-GCM\-SHA256 | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256 | 
| ECDHE\-RSA\-AES128\-SHA256 | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256  | 
| ECDHE\-RSA\-AES128\-SHA | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA | 
| ECDHE\-RSA\-AES256\-GCM\-SHA384 | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_GCM\_SHA384  | 
| ECDHE\-RSA\-AES256\-SHA384 | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384  | 
| ECDHE\-RSA\-AES256\-SHA | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA | 
| AES128\-GCM\-SHA256 | TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256 | 
| AES256\-GCM\-SHA384 | TLS\_RSA\_WITH\_AES\_256\_GCM\_SHA384 | 
| AES128\-SHA256 | TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA256 | 
| AES256\-SHA | TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA | 
| AES128\-SHA | TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA | 
| DES\-CBC3\-SHA  | TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA  | 
| RC4\-MD5 | TLS\_RSA\_WITH\_RC4\_128\_MD5 | 

## Supported SSL/TLS Protocols and Ciphers for Communication Between CloudFront and Your Origin<a name="secure-connections-supported-ciphers-cloudfront-to-origin"></a>

You can choose whether to require HTTPS between CloudFront and your origin and, if so, which protocols to allow:

+ To choose whether to require HTTPS between CloudFront and your origin, specify the applicable value for [Origin Protocol Policy \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](distribution-web-values-specify.md#DownloadDistValuesOriginProtocolPolicy)\.

+ To choose the SSL/TLS protocols that you want CloudFront and your origin to use to communicate, specify the applicable values for [Origin SSL Protocols \(Amazon EC2, Elastic Load Balancing, and Other Custom Origins Only\)](distribution-web-values-specify.md#DownloadDistValuesOriginSSLProtocols)\.

CloudFront forwards HTTPS requests to the origin server by using the following ciphers\. Your origin server must support at least one of these ciphers for CloudFront to establish an HTTPS connection to your origin\.


****  

| OpenSSL Cipher Name | RFC Cipher Name | 
| --- | --- | 
| ECDHE\-RSA\-AES128\-SHA256 | TLS\_ECDHE\_RSA\_WITH\_AES\_128\_CBC\_SHA256  | 
| ECDHE\-RSA\-AES256\-SHA384 | TLS\_ECDHE\_RSA\_WITH\_AES\_256\_CBC\_SHA384  | 
| AES256\-SHA | TLS\_RSA\_WITH\_AES\_256\_CBC\_SHA | 
| AES128\-SHA | TLS\_RSA\_WITH\_AES\_128\_CBC\_SHA | 
| DES\-CBC3\-SHA  | TLS\_RSA\_WITH\_3DES\_EDE\_CBC\_SHA  | 
| RC4\-MD5  | TLS\_RSA\_WITH\_RC4\_128\_MD5 | 