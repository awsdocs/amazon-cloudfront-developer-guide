# HTTP 502 Status Code \(Bad Gateway\)<a name="http-502-bad-gateway"></a>

An HTTP 502 status code \(Bad Gateway\) indicates that CloudFront wasn't able to serve the requested object because it couldn't connect to the origin server\. 

**Topics**
+ [SSL/TLS Negotiation Failure Between CloudFront and a Custom Origin Server](#ssl-negotitation-failure)
+ [Origin Is Not Responding with Supported Ciphers/Protocols](#origin-not-responding-with-supported-ciphers-protocols)
+ [SSL/TLS Certificate on the Origin Is Expired, Invalid, Self\-signed, or the Certificate Chain Is in the Wrong Order](#ssl-certificate-expired)
+ [Origin Is Not Responding on Specified Ports in Origin Settings](#origin-not-responding-on-specified-ports)
+ [CloudFront Was Not Able to Resolve Your Origin Domain Due to DNS Issues](#http-502-service-unavailable-origin-domain-dns-issues)
+ [Lambda@Edge Function Validation Errors](#http-502-bad-gateway-lambda-function-invalid)

## SSL/TLS Negotiation Failure Between CloudFront and a Custom Origin Server<a name="ssl-negotitation-failure"></a>

If you use a custom origin and you configured CloudFront to require HTTPS between CloudFront and your origin, the problem might be mismatched domain names\. The SSL/TLS certificate that is installed on your origin includes a domain name in the **Common Name** field and possibly several more in the **Subject Alternative Names** field\. \(CloudFront supports wildcard characters in certificate domain names\.\) One of the domain names in the certificate must match one or both of the following values:
+ The value that you specified for **Origin Domain Name** for the applicable origin in your distribution\.
+ The value of the `Host` header if you configured CloudFront to forward the `Host` header to your origin\. For more information about forwarding the `Host` header to your origin, see [Caching Content Based on Request Headers](header-caching.md)\.

If the domain names don't match, the SSL/TLS handshake fails, and CloudFront returns an HTTP status code 502 \(Bad Gateway\) and sets the `X-Cache` header to `Error from cloudfront`\.

To determine whether domain names in the certificate match the **Origin Domain Name** in the distribution or the `Host` header, you can use an online SSL checker or OpenSSL\. If the domain names don't match, you have two options:
+ Get a new SSL/TLS certificate that includes the applicable domain names\. 

  If you use AWS Certificate Manager \(ACM\), see [Request a Certificate](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-request.html) in the *AWS Certificate Manager User Guide* to request a new certificate\.
+ Change the distribution configuration so CloudFront no longer tries to use SSL to connect with your origin\.

### Online SSL Checker<a name="troubleshooting-ssl-negotiation-failure-online-ssl-checker"></a>

To find an SSL test tool, search the internet for "online ssl checker\." Typically, you specify the name of your domain, and the tool returns a variety of information about your SSL/TLS certificate\. Confirm that the certificate contains your domain name in the **Common Name** or **Subject Alternative Names** fields\.

### OpenSSL<a name="troubleshooting-ssl-negotiation-failure-openssl"></a>

To help troubleshoot HTTP 502 errors from CloudFront, you can use OpenSSL to try to make an SSL/TLS connection to your origin server\. If OpenSSL is not able to make a connection, that can indicate a problem with your origin server’s SSL/TLS configuration\. If OpenSSL is able to make a connection, it returns information about the origin server’s certificate, including the certificate’s common name \(`Subject CN` field\) and subject alternative name \(`Subject Alternative Name` field\)\.

Use the following OpenSSL command to test the connection to your origin server \(replace *origin domain name* with your origin server’s domain name, such as example\.com\):

`openssl s_client –connect origin domain name:443`

If the following are true:
+ Your origin server supports multiple domain names with multiple SSL/TLS certificates
+ Your distribution is configured to forward the `Host` header to the origin

Then add the `-servername` option to the OpenSSL command, as in the following example \(replace *CNAME* with the CNAME that’s configured in your distribution\):

`openssl s_client –connect origin domain name:443 -servername CNAME`

## Origin Is Not Responding with Supported Ciphers/Protocols<a name="origin-not-responding-with-supported-ciphers-protocols"></a>

CloudFront connects to origin servers using ciphers and protocols\. For a list of the ciphers and protocols that CloudFront supports, see [Supported Ciphers and Protocols](secure-connections-supported-viewer-protocols-ciphers.md)\. If your origin does not respond with one of these ciphers or protocols in the SSL/TLS exchange, CloudFront fails to connect\. You can validate that your origin supports the ciphers and protocols by using SSL Labs:
+ [SSL Labs](https://www.ssllabs.com/ssltest)

  Type the domain name of your origin in the **Hostname** field, and then choose **Submit**\. Review the **Common names** and **Alternative names** fields from the test to see if they match your origin's domain name\.

  After the test is finished, find the **Protocols** and **Cipher Suites** sections in the test results to see which ciphers or protocols are supported by your origin\. Compare them with the list of [Supported Ciphers and Protocols](secure-connections-supported-viewer-protocols-ciphers.md)\.

**Note**  
If you're using Elastic Load Balancing, see [SSL Security Policies for Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/elb-security-policy-options.html) in the *Elastic Load Balancing User Guide* to learn how to set the ciphers and protocols\. Using the Predefined Security Policy *ELBSecurityPolicy\-2016\-08* gives CloudFrontaccess to your elastic load balancer\. If you want to restrict it further using a custom policy, you must allow the ciphers that CloudFront supports\.

## SSL/TLS Certificate on the Origin Is Expired, Invalid, Self\-signed, or the Certificate Chain Is in the Wrong Order<a name="ssl-certificate-expired"></a>

If the origin server returns the following, CloudFront drops the TCP connection, returns HTTP status code 502 \(Bad Gateway\), and sets the `X-Cache` header to `Error from cloudfront`:
+ An expired certificate
+ Invalid certificate
+ Self\-signed certificate
+ Certificate chain in the wrong order

**Note**  
If the full chain of certificates, including the intermediate certificate, is not present, CloudFront drops the TCP connection\.

For information about installing an SSL/TLS certificate on your custom origin server, see [Requiring HTTPS for Communication Between CloudFront and Your Custom Origin](using-https-cloudfront-to-custom-origin.md)\.

## Origin Is Not Responding on Specified Ports in Origin Settings<a name="origin-not-responding-on-specified-ports"></a>

When you create an origin on your CloudFront distribution, you can set the ports that CloudFront connects to the origin with for HTTP and HTTPS traffic\. By default, these are TCP 80/443\. You have the option to modify these ports\. If your origin is rejecting traffic on these ports for any reason, or if your backend server isn't responding on the ports, CloudFront will fail to connect\.

To troubleshoot these issues, check any firewalls running in your infrastructure and validate that they are not blocking the supported IP ranges\. For more information, see [AWS IP Address Ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *Amazon Web Services General Reference*\. Additionally, verify whether your web server is running on the origin\.

## CloudFront Was Not Able to Resolve Your Origin Domain Due to DNS Issues<a name="http-502-service-unavailable-origin-domain-dns-issues"></a>

When CloudFront receives a request for an object that is expired or is not stored in its cache, it makes a request to the origin to get the updated object\. To make a successful request to the origin, CloudFront performs a DNS resolution on the origin domain name\. However, when the DNS service that hosts your domain is experiencing issues, CloudFront cannot resolve the domain name to get the IP address, resulting in a 502 error\. To fix this issue, contact your DNS provider, or, if you are using Amazon Route 53, see [Amazon Route 53 DNS](https://aws.amazon.com/premiumsupport/knowledge-center/route-53-dns-website-unreachable/)\.

To further troubleshoot this issue, ensure that the [authoritative name servers](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/route-53-concepts.html#route-53-concepts-authoritative-name-server) of your origin's root domain or zone apex \(such as `example.com`\) are functioning correctly\. Your authoritative name servers then receive the request and return the IP address that is associated with the domain, and are the same as the DNS servers that you used to set up your CloudFront distribution\. Use the following commands to find the name servers for your apex origin:

```
dig OriginAPEXDomainName NS +short 
nslookup –query=NS OriginAPEXDomainName
```

When you have the names of your name servers, use the following commands to query the domain name of your origin against them to make sure that each responds with an answer:

```
dig OriginDomainName @NameServerFromAbove
nslookup OriginDomainName NameServerFromAbove
```

## Lambda@Edge Function Validation Errors<a name="http-502-bad-gateway-lambda-function-invalid"></a>

If you’re using Lambda@Edge, an HTTP 502 status code can indicate that your Lambda function response was incorrectly formed or included invalid content\. For more information about troubleshooting Lambda@Edge errors, see [Testing and Debugging Lambda@Edge Functions](lambda-edge-testing-debugging.md)\.