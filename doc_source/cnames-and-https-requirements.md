# Requirements for using SSL/TLS certificates with CloudFront<a name="cnames-and-https-requirements"></a>

The requirements for SSL/TLS certificates are described in this topic\. They apply to both of the following, except as noted:
+ Certificates for using HTTPS between viewers and CloudFront 
+ Certificates for using HTTPS between CloudFront and your origin

**Topics**
+ [Certificate issuer](#https-requirements-certificate-issuer)
+ [AWS Region for AWS Certificate Manager](#https-requirements-aws-region)
+ [Certificate format](#https-requirements-certificate-format)
+ [Intermediate certificates](#https-requirements-intermediate-certificates)
+ [Key type](#https-requirements-key-type)
+ [Private key](#https-requirements-private-key)
+ [Permissions](#https-requirements-permissions)
+ [Size of the certificate key](#https-requirements-size-of-public-key)
+ [Supported types of certificates](#https-requirements-supported-types)
+ [Certificate expiration date and renewal](#https-requirements-cert-expiration)
+ [Domain names in the CloudFront distribution and in the certificate](#https-requirements-domain-names-in-cert)
+ [Minimum SSL/TLS protocol version](#https-requirements-minimum-ssl-protocol-version)
+ [Supported HTTP versions](#https-requirements-supported-http-versions)

## Certificate issuer<a name="https-requirements-certificate-issuer"></a>

We recommend that you use a certificate issued by [AWS Certificate Manager \(ACM\)](https://aws.amazon.com/certificate-manager/)\. For information about getting a certificate from ACM, see the *[AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)*\. To use an ACM certificate with CloudFront, make sure you request \(or import\) the certificate in the US East \(N\. Virginia\) Region \(`us-east-1`\)\.

 CloudFront supports the same certificate authorities \(CAs\) as Mozilla, so if you don’t use ACM, use a certificate issued by a CA on the [Mozilla Included CA Certificate List](https://wiki.mozilla.org/CA/Included_Certificates)\. For more information about getting and installing a certificate, refer to the documentation for your HTTP server software and to the documentation for the CA\.

## AWS Region for AWS Certificate Manager<a name="https-requirements-aws-region"></a>

To use a certificate in AWS Certificate Manager \(ACM\) to require HTTPS between viewers and CloudFront, make sure you request \(or import\) the certificate in the US East \(N\. Virginia\) Region \(`us-east-1`\)\.

If you want to require HTTPS between CloudFront and your origin, and you’re using a load balancer in Elastic Load Balancing as your origin, you can request or import the certificate in any AWS Region\.

## Certificate format<a name="https-requirements-certificate-format"></a>

The certificate must be in X\.509 PEM format\. This is the default format if you’re using AWS Certificate Manager\.

## Intermediate certificates<a name="https-requirements-intermediate-certificates"></a>

If you’re using a third\-party certificate authority \(CA\), list all of the intermediate certificates in the certificate chain that’s in the `.pem` file, beginning with one for the CA that signed the certificate for your domain\. Typically, you’ll find a file on the CA website that lists intermediate and root certificates in the proper chained order\.

**Important**  
Do not include the following: the root certificate, intermediate certificates that are not in the trust path, or your CA’s public key certificate\.

Here’s an example:

```
-----BEGIN CERTIFICATE-----
Intermediate certificate 2
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
Intermediate certificate 1
-----END CERTIFICATE-----
```

## Key type<a name="https-requirements-key-type"></a>

CloudFront supports RSA and ECDSA public–private key pairs\.

CloudFront supports HTTPS connections to both viewers and origins using RSA and ECDSA certificates\. With [AWS Certificate Manager \(ACM\)](https://console.aws.amazon.com/acm), you can request and import RSA certificates, and import ECDSA certificates, and then associate them with your CloudFront distribution\.

For lists of the RSA and ECDSA ciphers supported by CloudFront that you can negotiate in HTTPS connections, see [Supported protocols and ciphers between viewers and CloudFront](secure-connections-supported-viewer-protocols-ciphers.md) and [Supported protocols and ciphers between CloudFront and the origin](secure-connections-supported-ciphers-cloudfront-to-origin.md)\.

## Private key<a name="https-requirements-private-key"></a>

If you're using a certificate from a third\-party certificate authority \(CA\), note the following: 
+ The private key must match the public key that is in the certificate\.
+ The private key must be in PEM format\.
+ The private key cannot be encrypted with a password\.

If AWS Certificate Manager \(ACM\) provided the certificate, ACM doesn’t release the private key\. The private key is stored in ACM for use by AWS services that are integrated with ACM\.

## Permissions<a name="https-requirements-permissions"></a>

You must have permission to use and import the SSL/TLS certificate\. If you’re using AWS Certificate Manager \(ACM\), we recommend that you use AWS Identity and Access Management permissions to restrict access to the certificates\. For more information, see [Identity and access management](https://docs.aws.amazon.com/acm/latest/userguide/security-iam.html) in the *AWS Certificate Manager User Guide*\.

## Size of the certificate key<a name="https-requirements-size-of-public-key"></a>

The certificate key size that CloudFront supports depends on the type of key and certificate\.

**For RSA certificates:**  
CloudFront supports 1024\-bit and 2048\-bit RSA keys\. We recommend 2048\-bit keys\. The maximum key length for an RSA certificate that you use with CloudFront is 2048 bits, even though ACM supports larger keys\.  
For information about how to determine the size of an RSA key, see [Determining the size of the public key in an SSL/TLS RSA certificate](cnames-and-https-size-of-public-key.md)\.

**For ECDSA certificates:**  
CloudFront supports 256\-bit keys\. To use an ECDSA certificate in ACM to require HTTPS between viewers and CloudFront, use the prime256v1 elliptic curve\.

## Supported types of certificates<a name="https-requirements-supported-types"></a>

CloudFront supports all types of certificates issued by a trusted certificate authority\.

## Certificate expiration date and renewal<a name="https-requirements-cert-expiration"></a>

If you’re using certificates that you get from a third\-party certificate authority \(CA\), you must monitor certificate expiration dates and renew the certificates that you import into AWS Certificate Manager \(ACM\) or upload to the AWS Identity and Access Management certificate store before they expire\.

If you’re using ACM\-provided certificates, ACM manages certificate renewals for you\. For more information, see [Managed renewal](https://docs.aws.amazon.com/acm/latest/userguide/managed-renewal.html) in the *AWS Certificate Manager User Guide*\.

## Domain names in the CloudFront distribution and in the certificate<a name="https-requirements-domain-names-in-cert"></a>

When you’re using a custom origin, the SSL/TLS certificate on your origin includes a domain name in the **Common Name** field, and possibly several more in the **Subject Alternative Names** field\. \(CloudFront supports wildcard characters in certificate domain names\.\) 

One of the domain names in the certificate must match the domain name that you specify for Origin Domain Name\. If no domain name matches, CloudFront returns HTTP status code `502 (Bad Gateway)` to the viewer\.

**Important**  
When you add an alternate domain name to a distribution, CloudFront checks that the alternate domain name is covered by the certificate that you’ve attached\. The certificate must cover the alternate domain name in the subject alternate name \(SAN\) field of the certificate\. This means the SAN field must contain an exact match for the alternate domain name, or contain a wildcard at the same level of the alternate domain name that you’re adding\.  
For more information, see [Requirements for using alternate domain names](CNAMEs.md#alternate-domain-names-requirements)\.

## Minimum SSL/TLS protocol version<a name="https-requirements-minimum-ssl-protocol-version"></a>

If you’re using dedicated IP addresses, set the minimum SSL/TLS protocol version for the connection between viewers and CloudFront by choosing a security policy\.

For more information, see [Security policy](distribution-web-values-specify.md#DownloadDistValues-security-policy) in the topic [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\.

## Supported HTTP versions<a name="https-requirements-supported-http-versions"></a>

If you associate one certificate with more than one CloudFront distribution, all the distributions associated with the certificate must use the same option for [Supported HTTP versions](distribution-web-values-specify.md#DownloadDistValuesSupportedHTTPVersions)\. You specify this option when you create or update a CloudFront distribution\.