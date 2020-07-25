# Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)<a name="CNAMEs"></a>

In CloudFront, an alternate domain name, also known as a CNAME, lets you use your own domain name \(for example, `www.example.com`\) in your files’ URLs instead of using the domain name that CloudFront assigns to your distribution\. Both web and RTMP distributions support alternate domain names\.

When you create a distribution, CloudFront returns a domain name for the distribution, for example:

`d111111abcdef8.cloudfront.net`

When you use the CloudFront domain name for your files, the URL for a file called `/images/image.jpg` is:

`https://d111111abcdef8.cloudfront.net/images/image.jpg`

If you want to use your own domain name, such as `www.example.com`, instead of the `cloudfront.net` domain name, you can add an alternate domain name to your distribution, like `www.example.com`\. You can then use the following URL to view `/images/image.jpg`:

`https://www.example.com/images/image.jpg`

**Topics**
+ [Adding an Alternate Domain Name](#CreatingCNAME)
+ [Moving an Alternate Domain Name to a Different CloudFront Distribution](#alternate-domain-names-move)
+ [Removing an Alternate Domain Name](#alternate-domain-names-remove-domain)
+ [Using Wildcards in Alternate Domain Names That You Add to CloudFront](#alternate-domain-names-wildcard)
+ [Requirements for Using Alternate Domain Names](#alternate-domain-names-requirements)
+ [Restrictions on Using Alternate Domain Names](#alternate-domain-names-restrictions)

## Adding an Alternate Domain Name<a name="CreatingCNAME"></a>

The following task list describes how to use the CloudFront console to add an alternate domain name to your distribution so that you can use your own domain name in your links instead of the CloudFront domain name\. For information about updating your distribution using the CloudFront API, see [Working with distributions](distribution-working-with.md)\.

**Note**  
If you want viewers to use HTTPS with your alternate domain name, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

**Before you begin:** Make sure that you do the following before you update your distribution to add an alternate domain name:
+ Register the domain name with Route 53 or another domain provider\.
+ Add a certificate from an authorized certificate authority \(CA\) to CloudFront that covers the domain name you plan to use with the distribution, to validate that you are authorized to use the domain\. For more information, see [ Requirements for Using Alternate Domain Names](#alternate-domain-names-requirements)\.<a name="CreatingCNAMEProcess"></a>

**Adding an Alternate Domain Name**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID for the distribution that you want to update\.

1. On the **General** tab, choose **Edit**\.

1. Update the following values:  
**Alternate Domain Names \(CNAMEs\)**  
Add your alternate domain names\. Separate domain names with commas, or type each domain name on a new line\.  
**SSL Certificate \(Web Distributions Only\)**  
Choose the following setting:  
   + **Use HTTPS** – Choose **Custom SSL Certificate**, and then choose a certificate from the list\. The list includes certificates provisioned by AWS Certificate Manager \(ACM\), certificates that you purchased from another CA and uploaded to ACM, and certificates that you purchased from another CA and uploaded to the IAM certificate store\. 

     If you uploaded a certificate to the IAM certificate store but it doesn’t appear in the list, review the procedure [Importing an SSL/TLS Certificate](cnames-and-https-procedures.md#cnames-and-https-uploading-certificates) to confirm that you correctly uploaded the certificate\. 

     If you choose this setting, we recommend that you use only an alternate domain name in your object URLs \(`https://www.example.com/logo.jpg`\)\. If you use your CloudFront distribution domain name \(`https://d111111abcdef8.cloudfront.net/logo.jpg`\), a viewer might behave as follows, depending on the value that you choose for **Clients Supported**:
     + **All Clients**: If the viewer doesn’t support SNI, it displays a warning because the CloudFront domain name doesn’t match the domain name in your TLS/SSL certificate\.
     + **Only Clients that Support Server Name Indication \(SNI\)**: CloudFront drops the connection with the viewer without returning the object\.  
**Clients Supported \(Web Distributions Only\)**  
Choose an option:  
   + **All Clients**: CloudFront serves your HTTPS content using dedicated IP addresses\. If you select this option, you incur additional charges when you associate your SSL/TLS certificate with a distribution that is enabled\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.
   + **Only Clients that Support Server Name Indication \(SNI\) \(Recommended\)**: Older browsers or other clients that don't support SNI must use another method to access your content\.
For more information, see [Choosing How CloudFront Serves HTTPS Requests](cnames-https-dedicated-ip-or-sni.md)\.

1. Choose **Yes, Edit**\.

1. On the **General** tab for the distribution, confirm that **Distribution Status** has changed to **Deployed**\. If you try to use an alternate domain name before the updates to your distribution have been deployed, the links that you create in the following steps might not work\.

1. Configure the DNS service for the domain to route traffic for the domain, such as www\.example\.com, to the CloudFront domain name for your distribution, such as d111111abcdef8\.cloudfront\.net\. The method that you use depends on whether you’re using Route 53 as the DNS service provider for the domain or another provider\.
**Note**  
 If your DNS record already points to a distribution that is not the distribution that you are updating, then you only add the alternate domain name to your distribution after you update your DNS\. For more information, see [Restrictions on Using Alternate Domain Names](#alternate-domain-names-restrictions)\.   
**Route 53**  
Create an alias resource record set\. With an alias resource record set, you don’t pay for Route 53 queries\. In addition, you can create an alias resource record set for the root domain name \(example\.com\), which DNS doesn’t allow for CNAMEs\. For more information, see [Routing traffic to an Amazon CloudFront web distribution by using your domain name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.  
**Another DNS service provider**  
Use the method provided by your DNS service provider to add a CNAME record for your domain\. This new CNAME record will redirect DNS queries from your domain \(for example, www\.example\.com\) to the CloudFront domain name for your distribution \(for example, d111111abcdef8\.cloudfront\.net\)\. For more information, see the documentation provided by your DNS service provider\.  
If you already have an existing CNAME record for your domain name, update that record or replace it with a new one that points to the CloudFront domain name for your distribution\.  
In addition, confirm that your CNAME resource record set points to your distribution’s domain name and not to one of your origin servers\.

1. Using `dig` or a similar DNS tool, confirm that the resource record set that you created in step 7 points to the domain name for your distribution\.

   The following example shows a `dig` request on the www\.example\.com domain, as well as the relevant part of the response\.

   ```
   PROMPT> dig www.example.com
   
   ; <<> DiG 9.3.3rc2 <<> www.example.com
   ;; global options:	printcmd
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15917
   ;; flags: qr rd ra; QUERY: 1, ANSWER: 9, AUTHORITY: 2, ADDITIONAL: 0
   
   ;; QUESTION SECTION:
   ;www.example.com.     IN    A
   
   ;; ANSWER SECTION:
   www.example.com. 10800 IN	CNAME	d111111abcdef8.cloudfront.net.
   ...
   ```

   The answer section shows a CNAME record that routes queries for www\.example\.com to the CloudFront distribution domain name d111111abcdef8\.cloudfront\.net\. If the name on the right side of `CNAME` is the domain name for your CloudFront distribution, the CNAME record is configured correctly\. If that is any other value, for example, the domain name for your Amazon S3 bucket, then the CNAME record is configured incorrectly\. In that case, go back to step 4 and correct the CNAME record to point to the domain name for your distribution\.

1. Test the alternate domain name by visiting URLs with your domain name instead of the CloudFront domain name for your distribution\.

1. In your application, change the URLs for your objects to use your alternate domain name instead of the domain name of your CloudFront distribution\.

## Moving an Alternate Domain Name to a Different CloudFront Distribution<a name="alternate-domain-names-move"></a>

If you want to move an alternate domain name from one CloudFront distribution to another distribution, the steps you must take depend on the domain name that you want to move: 
+ For a subdomain like `www.example.com`, you can move the domain yourself\. For detailed steps, see [ Move a subdomain name to another distribution](#alternate-domain-names-move-subdomain)\.
+ For a domain like `example.com` \(an apex domain\), you must work with AWS Support to move the domain to another distribution [ Move an apex domain name to another distribution](#alternate-domain-names-move-domain)\.

### Move a subdomain name to another distribution<a name="alternate-domain-names-move-subdomain"></a>

Follow these steps to move a subdomain name, for example `www.example.com`\.<a name="MovingASubDomainToNewDistribution"></a>

**To move a subdomain name to a new distribution**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. If you don’t have a new distribution to move the domain name to, create one\. For more information, see [Creating a Distribution](distribution-web-creating-console.md)\.

1. Add to the distribution an alternate domain name that includes a wildcard for the alias record set or CNAME record\. For example, if the subdomain name that you want to move to the new distribution is `www.example.com`, add the alternate domain name `*.example.com`\. For more information, see [ Using Wildcards in Alternate Domain Names That You Add to CloudFront](#alternate-domain-names-wildcard)\.
**Note**  
You can’t add a wildcard to a top\-level domain name, such as `*.com`, so if you want to move a domain name like `example.com` to a new distribution, see [ Move an apex domain name to another distribution](#alternate-domain-names-move-domain)\.

1. Update the DNS configuration for your subdomain to point to the new distribution\. For example, you would update the DNS service for the subdomain `www.example.com` to route traffic to the CloudFront domain name for your distribution, `d111111abcdef8.cloudfront.net`\.

   To update the configuration, do one of the following: 
   + **If you’re using Route 53,** update alias records or CNAME records, depending how you set up the alternate domain name originally\. For more information, see [ Editing records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html) in the *Amazon Route 53 Developer Guide*\.
   + **If you’re using another DNS service provider,** use the method provided by the DNS service provider to update the CNAME record that directs traffic to CloudFront\. For more information, see the documentation provided by your DNS service provider\.
**Note**  
At this point, the subdomain is still served by the original distribution because that is where the alternate domain is currently configured\.

1. Using `dig` or a similar DNS tool, confirm that the resource record set that you created in step 4 points to the domain name for your distribution\.

   The following example shows a `dig` request on the www\.example\.com domain, as well as the relevant part of the response\.

   ```
   PROMPT> dig www.example.com
   
   ; <<> DiG 9.3.3rc2 <<> www.example.com
   ;; global options:	printcmd
   ;; Got answer:
   ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15917
   ;; flags: qr rd ra; QUERY: 1, ANSWER: 9, AUTHORITY: 2, ADDITIONAL: 0
   
   ;; QUESTION SECTION:
   ;www.example.com.     IN    A
   
   ;; ANSWER SECTION:
   www.example.com. 10800 IN	CNAME	d111111abcdef8.cloudfront.net.
   ...
   ```

   The answer section shows a CNAME record that routes queries for www\.example\.com to the CloudFront distribution domain name d111111abcdef8\.cloudfront\.net\. If the name on the right side of `CNAME` is the domain name for your CloudFront distribution, the CNAME record is configured correctly\. If that is any other value, for example, the domain name for your Amazon S3 bucket, then the CNAME record is configured incorrectly\. In that case, go back to step 4 and correct the CNAME record to point to the domain name for your distribution\.
**Note**  
When using Route 53 alias records, it is not possible to use `dig` to confirm that the resource record points to the new distribution\. In this case, you can either change the type of the resource record from alias to CNAME, or wait until the record’s time to live \(TTL\) value has expired\.

1. Remove the CNAME from the existing distribution and then add it to the new CloudFront distribution where the wildcard alternate name was added previously\.
**Note**  
While these changes propagate, the alternate domain name may be served by either the original or new distribution at random\. This behavior may persist for a few minutes after both distributions have reached status **Deployed**\.

1. Test the alternate domain name by visiting some URLs that use your domain name instead of the CloudFront domain name for your distribution\.

1. \(Optional\) The wildcard alternate name may now be removed from the new distribution\.

1. If you’re no longer using the original distribution, delete it\. For more information, see [Deleting a Distribution](HowToDeleteDistribution.md)\.

### Move an apex domain name to another distribution<a name="alternate-domain-names-move-domain"></a>

For apex domain names, like example\.com, you must contact AWS Support to move the domain name to another CloudFront distribution\. The extra steps are required because moving a domain yourself, as described in the previous procedure, requires setting up domain routing using a wildcard for part of the domain name\. For apex domains, for this step, you would have to set up routing as \*\.com, which isn’t allowed\.

Before you get started, if you don’t have a new distribution to move the domain name to, create one\. For more information, see [Creating a Distribution](distribution-web-creating-console.md)\.

Moving a domain name like example\.com to a new distribution takes two steps: 

Step 1: Provide proof to AWS Support that you own the domain name by creating a TXT record for your domain at your DNS service provider\. This helps prevent someone else from making changes to your distribution configuration\.

Step 2: Request that AWS Support move your domain to the new CloudFront distribution\.

Here are the specific steps to take\.<a name="MovingADomainToNewDistributionStep1"></a>

**Step 1: Create a TXT record for your domain**

1. Sign in to your DNS service provider website\.

   If your service provider is Route 53, [sign in to the Route 53 console](https://console.aws.amazon.com/route53/)\.

1. Create a TXT record for your domain like the following:

   `domain name TXT CloudFrontdistribution domain name`

   For example: `example.com TXT d111111abcdef8.cloudfront.net`
   + If your DNS service provider is Route 53, go to step 3 for detailed steps\.
   + If your domain is hosted by another DNS service provider, see the documentation at the DNS service provider\. You might need to request that your service provider create the TXT record for you\.
**Tip**  
If your service provider does not allow a TXT record for a domain to have the same information as a CNAME record, consider creating a TXT record that uses your domain name with an underscore \(\_\) prepended to it\. For an example, see the following Knowledge Center article: [Resolve CNAME Already Exists Error](https://aws.amazon.com/premiumsupport/knowledge-center/resolve-cnamealreadyexists-error/)\.

1. If your DNS service provider is Route 53, use the following steps to create a TXT record to prove domain ownership:

   1. On the **Hosted Zones** page, double\-click the row for the hosted zone in which you want to edit records\.

   1. Choose **Create Record Set**\.

   1. Enter the following values:
      + **Name**: The domain name you want to move to a new CloudFront distribution
      + **Type**: TXT
      + **Alias**: No
      + **TTL**: 60 seconds
      + **Value**: The name of the CloudFront distribution that you want to add this domain name to, such as d123\.cloudfront\.net
      + **Routing policy**: Simple

   1.  Choose **Create**\.

**Step 2: Request that AWS Support move your domain to the new CloudFront distribution**
+ Sign in to AWS and [contact AWS support](https://console.aws.amazon.com/support/home#/) to request that they verify that you own the domain, and move the domain to the new CloudFront distribution\.
**Note**  
AWS Support can’t verify your domain ownership until they can view the TXT record that you created for your domain\. Be aware that records that you create at your DNS provider can take a while \(up to several days\) to propagate through the DNS system\.

## Removing an Alternate Domain Name<a name="alternate-domain-names-remove-domain"></a>

If you want to stop routing traffic for a domain or subdomain to a CloudFront distribution, follow the steps in this section to update both the DNS configuration and the CloudFront distribution\.

It’s important that you remove the alternate domain names from the distribution as well as update your DNS configuration\. This helps prevent issues later if you want to associate the domain name with another CloudFront distribution\. If an alternate domain name is already associated with one distribution, it can’t be set up with another\.

**Note**  
If you want to remove the alternate domain name from this distribution so you can add it to another one, follow the steps in [ Moving an Alternate Domain Name to a Different CloudFront Distribution](#alternate-domain-names-move)\. If you follow the steps here instead \(to remove a domain\) and then add the domain to another distribution, there will be a period of time during which the domain won’t link to the new distribution because CloudFront is propagating to the updates to edge locations\.<a name="RemovingADomain"></a>

**To remove an alternate domain name from a distribution**

1. To start, route internet traffic for your domain to another resource that isn’t your CloudFront distribution, such as an Elastic Load Balancing load balancer\. Or you can delete the DNS record that’s routing traffic to CloudFront\.

   Do one of the following, depending on the DNS service for your domain:
   + **If you’re using Route 53**, update or delete alias records or CNAME records\. For more information, see [Editing records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html) or [Deleting records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-deleting.html)\.
   + **If you’re using another DNS service provider**, use the method provided by the DNS service provider to update or delete the CNAME record that directs traffic to CloudFront\. For more information, see the documentation provided by your DNS service provider\.

1. After you update your domain’s DNS records, wait until the changes have propagated and DNS resolvers are routing traffic to the new resource\. You can check to see when this is complete by creating some test links that use your domain in the URL\.

1. Sign in to the AWS Management Console and open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/), and update your CloudFront distribution to remove the domain name by doing the following:

   1. Choose the ID for the distribution that you want to update\.

   1. On the **General** tab, choose **Edit**\.

   1. In **Alternate Domain Names \(CNAMEs\)**, remove the alternate domain name \(or domain names\) that you no longer want to use for your distribution\.

   1. Choose **Yes, Edit**\.

## Using Wildcards in Alternate Domain Names That You Add to CloudFront<a name="alternate-domain-names-wildcard"></a>

When you add alternate domain names, you can use the \* wildcard at the beginning of a domain name instead of adding subdomains individually\. For example, with an alternate domain name of `*.example.com`, you can use any domain name that ends with example\.com in your object URLs, such as `www.example.com`, `product-name.example.com`, and `marketing.product-name.example.com`\. The name of an object is the same regardless of the domain name, for example: 

`www.example.com/images/image.jpg`

`product-name.example.com/images/image.jpg`

`marketing.product-name.example.com/images/image.jpg`

Follow these requirements for alternate domain names that include wildcards:
+ The alternate domain name must begin with an asterisk and a dot \( `*.` \)\.
+ You *cannot* use a wildcard to replace part of a subdomain name, like this: `*domain.example.com`\.
+ You cannot replace a subdomain in the middle of a domain name, like this: `subdomain.*.example.com`\.
+ All alternate domain names, including alternate domain names that use wildcards, must be covered by the subject alternative name \(SAN\) on the certificate\.

A wildcard alternate domain name, such as `*.example.com`, can include another alternate domain name, such as `example.com`, as long as they’re both in the same CloudFront distribution or they’re in distributions that were created by using the same AWS account\.

## Requirements for Using Alternate Domain Names<a name="alternate-domain-names-requirements"></a>

When you add an alternate domain name, such as www\.example\.com, to a CloudFront distribution, the following are requirements:

**Alternate domain names must be lowercase**  
All alternate domain names \(CNAMEs\) must be lowercase to be valid\.

**Alternate domain names must be covered by a valid SSL/TLS certificate**  
To add an alternate domain name \(CNAME\) to use with a CloudFront distribution, you must attach to your distribution a trusted, valid SSL/TLS certificate that covers the alternate domain name\. This ensures that only people with access to your domain’s certificate can associate with CloudFront a CNAME related to your domain\.  
A trusted certificate is one that is issued by AWS Certificate Manager \(ACM\) or by another valid certificate authority \(CA\); you can’t use a self\-signed certificate\. CloudFront supports the same certificate authorities as Mozilla\. For the current list, see [ Mozilla Included CA Certificate List](https://wiki.mozilla.org/CA/Included_Certificates)\.   
To verify an alternate domain name by using the certificate that you attach, including alternate domain names that include wildcards, CloudFront checks the subject alternative name \(SAN\) on the certificate\. The alternate domain name that you’re adding must be covered by the SAN\.  
Only one certificate can be attached to a CloudFront distribution at a time\.
You prove that you are authorized to add a specific alternate domain name to your distribution by doing one of the following:  
+ Attaching a certificate that includes the alternate domain name, like `product-name.example.com`\.
+ Attaching a certificate that includes a \* wildcard at the beginning of a domain name, to cover multiple subdomains with one certificate\. When you specify a wildcard, you can add multiple subdomains as alternate domain names in CloudFront\.
The following examples illustrate how using wildcards in domain names in a certificate work to authorize you to add specific alternate domain names in CloudFront\.  
+ You want to add `marketing.example.com` as an alternate domain name\. You list in your certificate the following domain name: `*.example.com`\. When you attach this certificate to CloudFront, you can add any alternate domain name for your distribution that replaces the wildcard at that level, including `marketing.example.com`\. You can also, for example, add the following alternate domain names:
  + `product.example.com`
  + `api.example.com`

  However, you can’t add alternate domain names that are at levels higher or lower than the wildcard\. For example, you can’t add the alternate domain names `example.com` or `marketing.product.example.com`\. 
+ You want to add `example.com` as an alternate domain name\. To do this, you must list the domain name `example.com` itself on the certificate that you attach to your distribution\.
+ You want to add `marketing.product.example.com` as an alternate domain name\. To do this, you can list `*.product.example.com` on the certificate, or you can list `marketing.product.example.com` itself on the certificate\.

**Permission to change DNS configuration**  
When you add alternate domain names, you must create CNAME records to route DNS queries for the domain names to your CloudFront distribution\. To do this, you must have permission to create CNAME records with the DNS service provider for the alternate domain names that you’re using\. Typically, this means that you own the domains, but you might be developing an application for the domain owner\.

**Alternate Domain Names and HTTPS**  
If you want viewers to use HTTPS with an alternate domain name, you must complete some additional configuration\. For more information, see [Using Alternate Domain Names and HTTPS](using-https-alternate-domain-names.md)\.

## Restrictions on Using Alternate Domain Names<a name="alternate-domain-names-restrictions"></a>

Note the following restrictions on using alternate domain names:

**Maximum number of alternate domain names**  
For the current maximum number of alternate domain names that you can add to a distribution, or to request a higher quota \(formerly known as limit\), see [General Quotas on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.

**Duplicate and overlapping alternate domain names**  
You cannot add an alternate domain name to a CloudFront distribution if the alternate domain name already exists in another CloudFront distribution, even if your AWS account owns the other distribution\.  
However, you can add a wildcard alternate domain name, such as `*.example.com`, that includes \(that overlaps with\) a non\-wildcard alternate domain name, such as `www.example.com`\. Overlapping alternate domain names can be in the same distribution or in separate distributions as long as both distributions were created by using the same AWS account\.  
If you have overlapping alternate domain names in two distributions, CloudFront sends the request to the distribution with the more specific name match, regardless of the distribution that the DNS record points to\. For example, `marketing.domain.com` is more specific than `*.domain.com`\.

**Alternate domain names that already point to a distribution**  
If your DNS record points to a distribution that is not the distribution that you are creating or modifying, then you can’t add the alternate domain name to your distribution\. In this scenario, you must update your DNS at your DNS provider before you can add the domain name for your CloudFront distribution\.  
To correct this, sign in to your DNS provider and remove the existing DNS record, or contact your DNS provider to remove it for you\. Then create the correct DNS record for your distribution, following the steps for adding or changing the alternate domain name for a distribution\. For more information, see [Adding an Alternate Domain Name](#CreatingCNAMEProcess) or [ Moving an Alternate Domain Name to a Different CloudFront Distribution](#alternate-domain-names-move)\.

**Domain fronting**  
CloudFront includes protection against domain fronting occurring across different AWS accounts\. Domain fronting is a scenario in which a non\-standard client creates a TLS/SSL connection to a domain name in one AWS account, but then makes an HTTPS request for an unrelated name in another AWS account\. For example, the TLS connection might connect to `www.example.com`, and then issue a request for `www.example.org`\.  
To prevent cases where domain fronting crosses different AWS accounts, CloudFront makes sure that the AWS account that owns the certificate that it serves for a specific connection always matches the AWS account that owns the request that it handles on that same connection\.  
If the two AWS account numbers do not match, CloudFront responds with an HTTP 421 Misdirected Request response to give the client a chance to connect using the correct domain\.

**Adding an alternate domain name at the top node \(zone apex\) for a domain**  
When you add an alternate domain name to a distribution, you typically create a CNAME record in your DNS configuration to route DNS queries for the domain name to your CloudFront distribution\. However, you can’t create a CNAME record for the top node of a DNS namespace, also known as the zone apex; the DNS protocol doesn’t allow it\. For example, if you register the DNS name `example.com`, the zone apex is `example.com`\. You can’t create a CNAME record for `example.com`, but you can create CNAME records for `www.example.com`, `newproduct.example.com`, and so on\.  
If you’re using Route 53 as your DNS service, you can create an alias resource record set, which has two advantages over CNAME records\. You can create an alias resource record set for a domain name at the top node \(example\.com\)\. In addition, when you use an alias resource record set, you don’t pay for Route 53 queries\.  
If you enable IPv6, you must create two alias resource record sets: one to route IPv4 traffic \(an A record\) and one to route IPv6 traffic \(an AAAA record\)\. For more information, see [Enable IPv6](distribution-web-values-specify.md#DownloadDistValuesEnableIPv6) in the topic [Values That You Specify When You Create or Update a Distribution](distribution-web-values-specify.md)\. 
For more information, see [Routing traffic to an Amazon CloudFront web distribution by using your domain name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.