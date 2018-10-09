# Choosing How CloudFront Serves HTTPS Requests<a name="cnames-https-dedicated-ip-or-sni"></a>

If you want your viewers to use HTTPS and to use alternate domain names for your objects, you need to choose one of the following options for how CloudFront serves HTTPS requests:
+ Use a dedicated IP address in each edge location
+ Use Server Name Indication \(SNI\)

This section explains how each option works\.

## Using a Dedicated IP Addresses to Serve HTTPS Requests \(Works for All Clients\)<a name="cnames-https-dedicated-ip"></a>

If you configure CloudFront to serve HTTPS requests using dedicated IP addresses, CloudFront associates your alternate domain name with a dedicated IP address in each CloudFront edge location\. When a viewer submits an HTTPS request for your content, here's what happens:

1. DNS routes the request to the IP address for your distribution in the applicable edge location\.

1. CloudFront uses the IP address to identify your distribution and to determine which SSL/TLS certificate to return to the viewer\. 

1. The viewer and CloudFront perform SSL/TLS negotiation using your SSL/TLS certificate\.

1. CloudFront returns the requested content to the viewer\.

This method works for every HTTPS request, regardless of the browser or other viewer that the user is using\. 

**Important**  
If you configure CloudFront to serve HTTPS requests using dedicated IP addresses, you incur an additional monthly charge\. The charge begins when you associate your SSL/TLS certificate with a distribution and you enable the distribution\. For more information about CloudFront pricing, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing)\. In addition, see [Using the Same Certificate for Multiple CloudFront Distributions](cnames-and-https-limits.md#cnames-and-https-same-certificate-multiple-distributions)\.

## Using SNI to Serve HTTPS Requests \(Works for Most Clients\)<a name="cnames-https-sni"></a>

If you configure CloudFront to serve HTTPS requests using Server Name Indication \(SNI\), CloudFront associates your alternate domain name with an IP address for each edge location, but the IP address is not dedicated to your distribution\. When a viewer submits an HTTPS request for your content, DNS routes the request to the IP address for the applicable edge location\. However, because the IP address isn't dedicated to your distribution, CloudFront can't determine, based on the IP address, which domain the request is for\. 

SSL/TLS negotiation occurs very early in the process of establishing an HTTPS connection\. If CloudFront can't immediately determine which domain the request is for, it drops the connection\. Using a dedicated IP address is one way to associate a request with a domain\. The other is SNI, which is an extension to the TLS protocol that is supported by most modern browsers\. When a viewer that supports SNI submits an HTTPS request for your content, here's what happens:

1. The viewer automatically gets the domain name from the request URL and adds it to a field in the request header\. 

1. When CloudFront receives the request, it finds the domain name in the request header and responds to the request with the applicable SSL/TLS certificate\.

1. The viewer and CloudFront perform SSL/TLS negotiation\.

1. CloudFront returns the requested content to the viewer\.

For a current list of the browsers that support SNI, see the Wikipedia entry [Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)\.

If you want to use SNI but some of your users' browsers don't support SNI, you have several options:
+ Configure CloudFront to serve HTTPS requests by using dedicated IP addresses instead of SNI\.
+ Use the CloudFront SSL/TLS certificate instead of a custom certificate\. This requires that you use the CloudFront domain name for your distribution in the URLs for your objects, for example, `https://d111111abcdef8.cloudfront.net/logo.png`\.

  If you use the default CloudFront certificate, viewers must support the SSL protocol TLSv1 or later\. CloudFront doesn't support SSLv3 with the default CloudFront certificate\.

  You also need to change the SSL/TLS certificate that CloudFront is using from a custom certificate to the default CloudFront certificate:
  + If you haven't used your distribution to distribute your content, you can just change the configuration\. For more information, see [Updating a Distribution](HowToUpdateDistribution.md)\.
  + If you have used your distribution to distribute your content, you need to create a new CloudFront distribution and change the URLs for your objects to reduce or eliminate the amount of time that your content is unavailable\. For more information, see [Reverting from a Custom SSL/TLS Certificate to the Default CloudFront Certificate](cnames-and-https-revert-to-cf-certificate.md)\.
+ If you can control which browser your users use, have them upgrade their browser to one that supports SNI\.
+ Use HTTP instead of HTTPS\.