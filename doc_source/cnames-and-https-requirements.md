# Requirements for Using SSL/TLS Certificates with CloudFront<a name="cnames-and-https-requirements"></a>

The requirements for SSL/TLS certificates are described in this topic\. They apply, except as noted, to both of the following:
+ Certificates for using HTTPS between viewers and CloudFront 
+ Certificates for using HTTPS between CloudFront and your origin

**Topics**
+ [Certificate Issuer](#https-requirements-certificate-issuer)
+ [AWS Region that You Request a Certificate In \(for AWS Certificate Manager\)](#https-requirements-aws-region)
+ [Certificate Format](#https-requirements-certificate-format)
+ [Intermediate Certificates](#https-requirements-intermediate-certificates)
+ [Key Type](#https-requirements-key-type)
+ [Private Key](#https-requirements-private-key)
+ [Permissions](#https-requirements-permissions)
+ [Size of the Public Key](#https-requirements-size-of-public-key)
+ [Supported Types of Certificates](#https-requirements-supported-types)
+ [Certificate Expiration Date and Renewal](#https-requirements-cert-expiration)
+ [Domain Names in the CloudFront Distribution and in the Certificate](#https-requirements-domain-names-in-cert)
+ [Minimum SSL Protocol Version](#https-requirements-minimum-ssl-protocol-version)
+ [Supported HTTP Versions](#https-requirements-supported-http-versions)

## Certificate Issuer<a name="https-requirements-certificate-issuer"></a>

The certificate issuer you must use depends on whether you want to require HTTPS between viewers and CloudFront or between CloudFront and your origin:
+ **HTTPS between viewers and CloudFront** – You can use a certificate that was issued by a trusted certificate authority \(CA\) such as Comodo, DigiCert, or Symantec, or you can use a certificate provided by AWS Certificate Manager \(ACM\)\.
**Important**  
If you want to use an alternate domain name with your CloudFront distribution, you must verify to CloudFront that you have authorized rights to use the alternate domain name\. To do this, you must attach a valid certificate to your distribution, and make sure that the certificate comes from a trusted CA that is listed on the [ Mozilla Included CA Certificate List](http://www.mozilla.org/en-US/about/governance/policies/security-group/certs/included/)\. CloudFront does not allow you to use a self\-signed certificate to verify your authorized rights to use an alternate domain name\.
+ **HTTPS between CloudFront and a custom origin** – If the origin is *not* an Elastic Load Balancing \(ELB\) load balancer, such as Amazon EC2, the certificate must be issued by a trusted CA such as Comodo, DigiCert, or Symantec\. If your origin is an ELB load balancer, you can also use a certificate provided by ACM\.
**Important**  
When CloudFront uses HTTPS to communicate with your origin, CloudFront verifies that the certificate was issued by a trusted CA\. CloudFront supports the same certificate authorities as Mozilla; for the current list, see [ Mozilla Included CA Certificate List](https://wiki.mozilla.org/CA/Included_Certificates)\. You cannot use a self\-signed certificate for HTTPS communication between CloudFront and your origin\.

  For more information about getting and installing an SSL/TLS certificate, refer to the documentation for your HTTP server software and to the documentation for the certificate authority\. For information about ACM, see the [AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)\.

## AWS Region that You Request a Certificate In \(for AWS Certificate Manager\)<a name="https-requirements-aws-region"></a>

If you want to require HTTPS between viewers and CloudFront, you must change the AWS Region to US East \(N\. Virginia\) in the AWS Certificate Manager console before you request or import a certificate\. 

If you want to require HTTPS between CloudFront and your origin, and you're using an ELB load balancer as your origin, you can request or import a certificate in any Region\. 

## Certificate Format<a name="https-requirements-certificate-format"></a>

The certificate must be in X\.509 PEM format\. This is the default format if you're using AWS Certificate Manager\.

## Intermediate Certificates<a name="https-requirements-intermediate-certificates"></a>

If you're using a third\-party certificate authority \(CA\), in the `.pem` file, list all of the intermediate certificates in the certificate chain, beginning with one for the CA that signed the certificate for your domain\. Typically, you'll find a file on the CA website that lists intermediate and root certificates in the proper chained order\.

**Important**  
Do not include the following: the root certificate, intermediate certificates that are not in the trust path, or your CA's public key certificate\. 

Here's an example:

```
-----BEGIN CERTIFICATE----- 
Intermediate certificate 2 
-----END CERTIFICATE----- 
-----BEGIN CERTIFICATE----- 
Intermediate certificate 1 
-----END CERTIFICATE-----
```

## Key Type<a name="https-requirements-key-type"></a>

CloudFront supports only RSA public/private key pairs\.

## Private Key<a name="https-requirements-private-key"></a>

If you're using a certificate from a third\-party certificate authority \(CA\), note the following: 
+ The private key must match the public key that is in the certificate\.
+ The private key also must be an RSA private key in PEM format, where the PEM header is `BEGIN RSA PRIVATE KEY` and the footer is `END RSA PRIVATE KEY`\.
+ The private key cannot be encrypted with a password\.

If AWS Certificate Manager \(ACM\) provided the certificate, ACM doesn't release the private key\. The private key is stored in ACM for use by AWS services that are integrated with ACM\.

## Permissions<a name="https-requirements-permissions"></a>

You must have permission to use and import the SSL/TLS certificate\. If you're using AWS Certificate Manager \(ACM\), we recommend that you use AWS Identity and Access Management permissions to restrict access to the certificates\. For more information, see [Permissions and Policies](http://docs.aws.amazon.com/acm/latest/userguide/assets.html) in the *AWS Certificate Manager User Guide*\.

## Size of the Public Key<a name="https-requirements-size-of-public-key"></a>

The length of the public key for a certificate depends on where you're storing it\.
+ Importing a certificate into AWS Certificate Manager \(ACM\): public key length must be 1024 bits or 2048 bits\. The maximum length for a certificate that you use with CloudFront is 2048 bits, even though ACM supports larger keys\.
+ Uploading a certificate to the AWS Identity and Access Management \(IAM\) certificate store: maximum size of the public key is 2048 bits\.

We recommend using 2048 bits\.

For information about the public keys for certificates provided by ACM, see [ACM Certificate Characteristics](http://docs.aws.amazon.com/acm/latest/userguide/acm-certificate.html) in the *AWS Certificate Manager User Guide*\. 

For information about how to determine the size of the public key, see [Determining the Size of the Public Key in an SSL/TLS Certificate](cnames-and-https-size-of-public-key.md)\.

## Supported Types of Certificates<a name="https-requirements-supported-types"></a>

CloudFront supports all types of certificates, including the following:
+ Domain\-validated certificates
+ Extended validation \(EV\) certificates
+ High\-assurance certificates
+ Wildcard certificates \(`*.example.com`\)
+ Subject alternative name \(SAN\) certificates \(`example.com` and `example.net`\)

## Certificate Expiration Date and Renewal<a name="https-requirements-cert-expiration"></a>

If you're using certificates that you get from a third\-party certificate authority \(CA\), you must monitor certificate expiration dates and renew SSL/TLS certificates that you import into AWS Certificate Manager \(ACM\) or upload to the AWS Identity and Access Management certificate store\. 

If you're using ACM\-provided certificates, ACM manages certificate renewals for you\. For more information, see [Managed Renewal](http://docs.aws.amazon.com/acm/latest/userguide/acm-renewal.html) in the *AWS Certificate Manager User Guide*\.

## Domain Names in the CloudFront Distribution and in the Certificate<a name="https-requirements-domain-names-in-cert"></a>

When you're using a custom origin, the SSL/TLS certificate on your origin includes a domain name in the **Common Name** field, and possibly several more in the **Subject Alternative Names** field\. \(CloudFront supports wildcard characters in certificate domain names\.\) 

**Important**  
When you add an alternate domain name to a distribution, CloudFront checks that the alternate domain name is covered by the certificate that you've attached\. The certificate must cover the alternate domain name in the subject alternate name \(SAN\) field of the certificate\. This means the SAN field must contain an exact match for the alternate domain name, or contain a wildcard at the same level of the alternate domain name that you're adding\.  
For more information, see [ Requirements for Using Alternate Domain Names](CNAMEs.md#alternate-domain-names-requirements)\.

One of the domain names in the certificate must match the domain name that you specify for Origin Domain Name\. If no domain name matches, CloudFront returns HTTP status code `502 (Bad Gateway)` to the viewer\.

## Minimum SSL Protocol Version<a name="https-requirements-minimum-ssl-protocol-version"></a>

If you're using dedicated IP addresses, set the minimum SSL protocol version for the connection between viewers and CloudFront by choosing a security policy\.

For more information, see [Security Policy](distribution-web-values-specify.md#DownloadDistValues-security-policy) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\.

## Supported HTTP Versions<a name="https-requirements-supported-http-versions"></a>

If you associate one certificate with more than one CloudFront distribution, all the distributions associated with the certificate must use the same option for [Supported HTTP Versions](distribution-web-values-specify.md#DownloadDistValuesSupportedHTTPVersions)\. You specify this option when you create or update a CloudFront distribution\.