# Choosing How CloudFront Serves HTTPS Requests<a name="cnames-https-dedicated-ip-or-sni"></a>

If you want your viewers to use HTTPS and to use alternate domain names for your files, choose one of the following options for how CloudFront serves HTTPS requests:
+ Use Server Name Indication \(SNI\) – Recommended
+ Use a dedicated IP address in each edge location

This section explains how each option works\.

## Using SNI to Serve HTTPS Requests \(Works for Most Clients\)<a name="cnames-https-sni"></a>

Server Name Indication \(SNI\) is an extension to the TLS protocol that is supported by browsers and clients released after 2010\. If you configure CloudFront to serve HTTPS requests using SNI, CloudFront associates your alternate domain name with an IP address for each edge location\. When a viewer submits an HTTPS request for your content, DNS routes the request to the IP address for the correct edge location\. The IP address to your domain name is determined during the SSL/TLS handshake negotiation; the IP address isn't dedicated to your distribution\. 

The SSL/TLS negotiation occurs early in the process of establishing an HTTPS connection\. If CloudFront can't immediately determine which domain the request is for, it drops the connection\. When a viewer that supports SNI submits an HTTPS request for your content, here's what happens:

1. The viewer automatically gets the domain name from the request URL and adds it to a field in the request header\. 

1. When CloudFront receives the request, it finds the domain name in the request header and responds to the request with the applicable SSL/TLS certificate\.

1. The viewer and CloudFront perform SSL/TLS negotiation\.

1. CloudFront returns the requested content to the viewer\.

For a current list of the browsers that support SNI, see the Wikipedia entry [Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)\.

If you want to use SNI but some of your users' browsers don't support SNI, you have several options:
+ Configure CloudFront to serve HTTPS requests by using dedicated IP addresses instead of SNI\. For more information, see [Using a Dedicated IP Addresses to Serve HTTPS Requests \(Works for All Clients\)](#cnames-https-dedicated-ip)\.
+ Use the CloudFront SSL/TLS certificate instead of a custom certificate\. This requires that you use the CloudFront domain name for your distribution in the URLs for your files, for example, `https://d111111abcdef8.cloudfront.net/logo.png`\.

  If you use the default CloudFront certificate, viewers must support the SSL protocol TLSv1 or later\. CloudFront doesn't support SSLv3 with the default CloudFront certificate\.

  You also must change the SSL/TLS certificate that CloudFront is using from a custom certificate to the default CloudFront certificate:
  + If you haven't used your distribution to distribute your content, you can just change the configuration\. For more information, see [Updating a Distribution](HowToUpdateDistribution.md)\.
  + If you have used your distribution to distribute your content, you must create a new CloudFront distribution and change the URLs for your files to reduce or eliminate the amount of time that your content is unavailable\. For more information, see [Reverting from a Custom SSL/TLS Certificate to the Default CloudFront Certificate](cnames-and-https-revert-to-cf-certificate.md)\.
+ If you can control which browser your users use, have them upgrade their browser to one that supports SNI\.
+ Use HTTP instead of HTTPS\.

## Using a Dedicated IP Addresses to Serve HTTPS Requests \(Works for All Clients\)<a name="cnames-https-dedicated-ip"></a>

Server Name Indication \(SNI\) is one way to associate a request with a domain\. Another way is to use a dedicated IP address\. If you have users who can't upgrade to a browser or client released after 2010, you can use a dedicated IP address to serve HTTPS requests\. For a current list of the browsers that support SNI, see the Wikipedia entry [Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)\. 

**Important**  
If you configure CloudFront to serve HTTPS requests using dedicated IP addresses, you incur an additional monthly charge\. The charge begins when you associate your SSL/TLS certificate with a distribution and you enable the distribution\. For more information about CloudFront pricing, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing)\. In addition, see [Using the Same Certificate for Multiple CloudFront Distributions](cnames-and-https-limits.md#cnames-and-https-same-certificate-multiple-distributions)\.

When you configure CloudFront to serve HTTPS requests using dedicated IP addresses, CloudFront associates your alternate domain name with a dedicated IP address in each CloudFront edge location\. When a viewer submits an HTTPS request for your content, here's what happens:

1. DNS routes the request to the IP address for your distribution in the applicable edge location\.

1. CloudFront uses the IP address to identify your distribution and to determine which SSL/TLS certificate to return to the viewer\. 

1. The viewer and CloudFront perform SSL/TLS negotiation using your SSL/TLS certificate\.

1. CloudFront returns the requested content to the viewer\.

This method works for every HTTPS request, regardless of the browser or other viewer that the user is using\. 