# Using Alternate Domain Names \(CNAMEs\)<a name="CNAMEs"></a>

In CloudFront, an alternate domain name, also known as a CNAME, lets you use your own domain name \(for example, `www.example.com`\) for links to your objects instead of using the domain name that CloudFront assigns to your distribution\. Both web and RTMP distributions support alternate domain names\.

When you create a distribution, CloudFront returns a domain name for the distribution, for example:

`d111111abcdef8.cloudfront.net`

When you use the CloudFront domain name for your objects, the URL for an object called `/images/image.jpg` is:

`http://d111111abcdef8.cloudfront.net/images/image.jpg`

If you want to use your own domain name, such as `www.example.com`, instead of the `cloudfront.net` domain name that CloudFront assigned to your distribution, you can add an alternate domain name to your distribution for `www.example.com`\. You can then use the following URL for `/images/image.jpg`:

`http://www.example.com/images/image.jpg`


+ [Using the \* Wildcard in Alternate Domain Names](#alternate-domain-names-wildcard)
+ [Restrictions on Using Alternate Domain Names](#alternate-domain-names-restrictions)
+ [Adding an Alternate Domain Name](#CreatingCNAME)

## Using the \* Wildcard in Alternate Domain Names<a name="alternate-domain-names-wildcard"></a>

When you add alternate domain names, you can use the \* wildcard at the beginning of a domain name instead of specifying subdomains individually\. For example, with an alternate domain name of `*.example.com`, you can use any domain name that ends with example\.com in your object URLs, such as `www.example.com`, `product-name.example.com`, and `marketing.product-name.example.com`\. The name of an object is the same regardless of the domain name, for example: 

`www.example.com/images/image.jpg`

`product-name.example.com/images/image.jpg`

`marketing.product-name.example.com/images/image.jpg`

The alternate domain name must begin with an asterisk and a dot \( `*.` \)\. You *cannot* use a wildcard to replace part of a subdomain name, like this: `*domain.example.com`, and you cannot replace a subdomain in the middle of a domain name, like this: `subdomain.*.example.com`\. 

A wildcard alternate domain name, such as `*.example.com`, can include another alternate domain name, such as `example.com`, as long as they're both in the same CloudFront distribution or they're in distributions that were created by using the same AWS account\. 

## Restrictions on Using Alternate Domain Names<a name="alternate-domain-names-restrictions"></a>

Note the following restrictions on using alternate domain names:

**CNAMEs Must be Lowercase**  
All CNAMES must be lowercase to be valid\.

**Maximum Number of Alternate Domain Names**  
For the current limit on the number of alternate domain names that you can add to a distribution or to request a higher limit, see [General Limits on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.

**Permission to Change DNS Configuration**  
If you're adding alternate domain names to your distribution, you need to create CNAME records to route DNS queries for the domain names to your CloudFront distribution\. You must have permission to create CNAME records with the DNS service provider for the corresponding domains\. Typically, this means that you own the domains, but you may also be developing an application for the domain owner\. 

**Duplicate and Overlapping Alternate Domain Names**  
You cannot add an alternate domain name to a CloudFront distribution if the alternate domain name already exists in another CloudFront distribution, even if your AWS account owns the other distribution\.  
However, you can add a wildcard alternate domain name, such as `*.example.com`, that includes \(that overlaps with\) a non\-wildcard alternate domain name, such as `www.example.com`\. Overlapping domain names can be in the same distribution or in separate distributions as long as both distributions were created by using the same AWS account\.

**Alternate Domain Names at the Zone Apex for a Domain**  
When you add an alternate domain name to a distribution, you need to create a CNAME record in your DNS configuration to route DNS queries for the domain name to your CloudFront distribution\. However, you can't create a CNAME record for the top node of a DNS namespace, also known as the zone apex; the DNS protocol doesn't allow it\. For example, if you register the DNS name `example.com`, the zone apex is `example.com`\. You can't create a CNAME record for `example.com`, but you can create CNAME records for `www.example.com`, `newproduct.example.com`, and so on\.  
If you're using Route 53 as your DNS service, you can create an alias resource record set, which has two advantages over CNAME records\. You can create an alias resource record set for a domain name at the zone apex \(example\.com\)\. In addition, with an alias resource record set, you don't pay for Route 53 queries\.  
If you enable IPv6, you need to create two alias resource record sets, one to route IPv4 traffic \(an A record\) and one to route IPv6 traffic \(an AAAA record\)\. For more information, see [Enable IPv6](distribution-web-values-specify.md#DownloadDistValuesEnableIPv6) in the topic [Values That You Specify When You Create or Update a Web Distribution](distribution-web-values-specify.md)\.
For more information, see [Routing Queries to an Amazon CloudFront Distribution](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.

**Alternate Domain Names and HTTPS**  
If you want viewers to use HTTPS with an alternate domain names, additional configuration is required\. For more information, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

## Adding an Alternate Domain Name<a name="CreatingCNAME"></a>

The following task list describes the process for using the CloudFront console to add an alternate domain name to your distribution so you can use your own domain name in your links instead of the CloudFront domain name that is associated with your distribution\.

**Note**  
If you want viewers to use HTTPS with your alternate domain name, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

For information about updating your distribution using the CloudFront API, see [Working with Distributions](distribution-working-with.md)\.<a name="CreatingCNAMEProcess"></a>

**Adding an Alternate Domain Name Using the CloudFront Console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID for the distribution that you want to update\.

1. On the **General** tab, choose **Edit**\.

1. Update the following values:  
**Alternate Domain Names \(CNAMEs\)**  
Add the applicable alternate domain names\. Separate domain names with commas, or type each domain name on a new line\.  
**SSL Certificate \(Web Distributions Only\)**  
Choose the applicable option, depending on whether you want viewers to use HTTPS to access your content:  

   + **Don't use HTTPS** – Choose **Default CloudFront Certificate**\.

   + **Use HTTPS** – Choose **Custom SSL Certificate**, and choose a certificate from the list\. The list can include certificates provisioned by AWS Certificate Manager \(ACM\), certificates that you purchased from another certificate authority and uploaded to ACM, and certificates that you purchased from another certificate authority and uploaded to the IAM certificate store\. 

     If you uploaded a certificate to the IAM certificate store but it doesn't appear in the list, review the procedure [Importing an SSL/TLS Certificate](cnames-and-https-procedures.md#cnames-and-https-uploading-certificates) to confirm that you correctly uploaded the certificate\. 

     If you choose this setting, we recommend that you use only an alternate domain name in your object URLs \(https://example\.com/logo\.jpg\)\. If you use your CloudFront distribution domain name \(https://d111111abcdef8\.cloudfront\.net/logo\.jpg\) and the viewer supports SNI, then CloudFront behaves normally\. However, a viewer that does not support SNI exhibits one of the following behaviors, depending on the value of **Clients Supported**:

     + **All Clients**: If the viewer doesn't support SNI, it displays a warning because the CloudFront domain name doesn't match the domain name in your SSL/TLS certificate\.

     + **Only Clients that Support Server Name Indication \(SNI\)**: CloudFront drops the connection with the viewer without returning the object\.  
**Clients Supported \(Web Distributions Only\)**  
Choose the applicable options:  

   + **All Clients**: CloudFront serves your HTTPS content using dedicated IP addresses\. If you select this option, you incur additional charges when you associate your SSL/TLS certificate with a distribution that is enabled\. For more information, see [http://aws\.amazon\.com/cloudfront/pricing](http://aws.amazon.com/cloudfront/pricing)\.

   + **Only Clients that Support Server Name Indication \(SNI\)**: Older browsers or other clients that don't support SNI must use another method to access your content\.
For more information, see [Choosing How CloudFront Serves HTTPS Requests](cnames-https-dedicated-ip-or-sni.md)\.

1. Choose **Yes, Edit**\.

1. On the **General** tab for the distribution, confirm that **Distribution Status** has changed to **Deployed**\. If you try to use an alternate domain name before the updates to your distribution have been deployed, the links that you create in the following steps might not work\.

1. Configure the DNS service for the domain to route traffic for the domain, such as example\.com, to the CloudFront domain name for your distribution, such as d111111abcdef8\.cloudfront\.net\. The method that you use depends on whether you're using Route 53 as the DNS service provider for the domain:  
**Route 53**  
Create an alias resource record set\. With an alias resource record set, you don't pay for Route 53 queries\. In addition, you can create an alias resource record set for the root domain name \(example\.com\), which DNS doesn't allow for CNAMEs\. For more information, see [Routing Queries to an Amazon CloudFront Distribution](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.  
**Another DNS service provider**  
Use the method provided by your DNS service provider to add a CNAME resource record set to the hosted zone for your domain\. This new CNAME resource record set will redirect DNS queries from your domain \(for example, www\.example\.com\) to the CloudFront domain name for your distribution \(for example, d111111abcdef8\.cloudfront\.net\)\. For more information, see the documentation provided by your DNS service provider\.  
If you already have an existing CNAME record for your domain name, update that resource record set or replace it with a new one that points to the CloudFront domain name for your distribution\.   
In addition, confirm that your CNAME resource record set points to your distribution's domain name and not to one of your origin servers\.

1. Using dig or a similar tool, confirm that the resource record set that you created in step 7 points to the domain name for your distribution\. For more information about dig, go to [http://www\.kloth\.net/services/dig\.php](http://www.kloth.net/services/dig.php)\.

   The following example shows a dig request on the images\.example\.com domain, as well as the relevant part of the response\.

   ```
   [prompt]--> dig images.example.com
   
   ; <<> DiG 9.3.3rc2 <<> images.example.com
   ;; global options:	printcmd
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15917
   ;; flags: qr rd ra; QUERY: 1, ANSWER: 9, AUTHORITY: 2, ADDITIONAL: 0
   
   ;; QUESTION SECTION:
   ;images.example.com.     IN    A
   
   ;; ANSWER SECTION:
   images.example.com. 10800 IN	CNAME	d111111abcdef8.cloudfront.net.
   ...
   ...
   ```

   The line in the Answer Section shows a CNAME resource record set that routes queries for images\.example\.com to the CloudFront distribution domain name d111111abcdef8\.cloudfront\.net\. The CNAME resource record set is configured correctly if the name on the right side of `CNAME` is the domain name for your CloudFront distribution\. If that is any other value, for example, the domain name for your Amazon S3 bucket, then the CNAME resource record set is configured incorrectly\. In that case, go back to Step 4 and correct the CNAME record to point to the domain name for your distribution\.

1. Test the alternate domain name by creating some test links that use your domain name in the URL instead of the CloudFront domain name for your distribution\.

1. In your application, change the links for your objects to use your alternate domain name instead of the domain name of your CloudFront distribution\.