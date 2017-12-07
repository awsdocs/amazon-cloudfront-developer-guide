# Using Alternate Domain Names and HTTPS<a name="using-https-alternate-domain-names"></a>

By default, you can deliver your content to viewers over HTTPS by using your CloudFront distribution domain name in your URLs, for example, `https://d111111abcdef8.cloudfront.net/image.jpg`\. For more information, see [Requiring HTTPS for Communication Between Viewers and CloudFront](using-https-viewers-to-cloudfront.md)\.

If you want your viewers to use HTTPS and you want to use your own domain name in the URLs for your objects \(for example, `https://www.example.com/image.jpg`\), you need to perform several additional steps, as explained in this topic\.

**Important**  
When you add a certificate to your distribution, CloudFront immediately propagates the certificate to all of its edge locations\. As new edge locations become available, CloudFront will propagate the certificate to those locations, too\. You can't restrict the edge locations that CloudFront propagates the certificates to\.


+ [Choosing How CloudFront Serves HTTPS Requests](cnames-https-dedicated-ip-or-sni.md)
+ [Requirements for Using SSL/TLS Certificates with CloudFront](cnames-and-https-requirements.md)
+ [Limits on Using SSL/TLS Certificates with CloudFront \(HTTPS Between Viewers and CloudFront Only\)](cnames-and-https-limits.md)
+ [Configuring Alternate Domain Names and HTTPS](cnames-and-https-procedures.md)
+ [Determining the Size of the Public Key in an SSL/TLS Certificate](cnames-and-https-size-of-public-key.md)
+ [Increasing the Limit for SSL/TLS Certificates](increasing-the-limit-for-ssl-tls-certificates.md)
+ [Rotating SSL/TLS Certificates](cnames-and-https-rotate-certificates.md)
+ [Reverting from a Custom SSL/TLS Certificate to the Default CloudFront Certificate](cnames-and-https-revert-to-cf-certificate.md)
+ [Switching from a Custom SSL/TLS Certificate with Dedicated IP Addresses to SNI](cnames-and-https-switch-dedicated-to-sni.md)