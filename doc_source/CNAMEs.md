# Using custom URLs by adding alternate domain names \(CNAMEs\)<a name="CNAMEs"></a>

In CloudFront, an alternate domain name, also known as a CNAME, lets you use your own domain name \(for example, www\.example\.com\) in your files’ URLs instead of using the domain name that CloudFront assigns to your distribution\.

When you create a distribution, CloudFront provides a domain name for the distribution, such as d111111abcdef8\.cloudfront\.net\.

If you want to use your own domain name, such as www\.example\.com, instead of the cloudfront\.net domain name, you can add an alternate domain name to your distribution\.

**Topics**
+ [Adding an alternate domain name](#CreatingCNAME)
+ [Moving an alternate domain name to a different distribution](#alternate-domain-names-move)
+ [Removing an alternate domain name](#alternate-domain-names-remove-domain)
+ [Using wildcards in alternate domain names](#alternate-domain-names-wildcard)
+ [Requirements for using alternate domain names](#alternate-domain-names-requirements)
+ [Restrictions on using alternate domain names](#alternate-domain-names-restrictions)

## Adding an alternate domain name<a name="CreatingCNAME"></a>

The following task list describes how to use the CloudFront console to add an alternate domain name to your distribution so that you can use your own domain name in your links instead of the CloudFront domain name\. For information about updating your distribution using the CloudFront API, see [Working with distributions](distribution-working-with.md)\.

**Note**  
If you want viewers to use HTTPS with your alternate domain name, see [Using alternate domain names and HTTPS](using-https-alternate-domain-names.md)\.

**Before you begin:** Make sure that you do the following before you update your distribution to add an alternate domain name:
+ Register the domain name with Route 53 or another domain registrar\.
+ Get an SSL/TLS certificate from an authorized certificate authority \(CA\) that covers the domain name\. Add the certificate to your distribution to validate that you are authorized to use the domain\. For more information, see [Requirements for using alternate domain names](#alternate-domain-names-requirements)\.<a name="CreatingCNAMEProcess"></a>

**Adding an alternate domain name**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose the ID for the distribution that you want to update\.

1. On the **General** tab, choose **Edit**\.

1. Update the following values:  
**Alternate Domain Names \(CNAMEs\)**  
Add your alternate domain names\. Separate domain names with commas, or type each domain name on a new line\.  
**SSL Certificate**  
Choose the following setting:  
   + **Use HTTPS** – Choose **Custom SSL Certificate**, and then choose a certificate from the list\. The list includes certificates provisioned by AWS Certificate Manager \(ACM\), certificates that you purchased from another CA and uploaded to ACM, and certificates that you purchased from another CA and uploaded to the IAM certificate store\. 

     If you uploaded a certificate to the IAM certificate store but it doesn’t appear in the list, review the procedure [Importing an SSL/TLS certificate](cnames-and-https-procedures.md#cnames-and-https-uploading-certificates) to confirm that you correctly uploaded the certificate\. 

     If you choose this setting, we recommend that you use only an alternate domain name in your object URLs \(https://www\.example\.com/logo\.jpg\)\. If you use your CloudFront distribution domain name \(https://d111111abcdef8\.cloudfront\.net\.cloudfront\.net/logo\.jpg\), a viewer might behave as follows, depending on the value that you choose for **Clients Supported**:
     + **All Clients**: If the viewer doesn’t support SNI, it displays a warning because the CloudFront domain name doesn’t match the domain name in your TLS/SSL certificate\.
     + **Only Clients that Support Server Name Indication \(SNI\)**: CloudFront drops the connection with the viewer without returning the object\.  
**Clients Supported**  
Choose an option:  
   + **All Clients**: CloudFront serves your HTTPS content using dedicated IP addresses\. If you select this option, you incur additional charges when you associate your SSL/TLS certificate with a distribution that is enabled\. For more information, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.
   + **Only Clients that Support Server Name Indication \(SNI\) \(Recommended\)**: Older browsers or other clients that don't support SNI must use another method to access your content\.
For more information, see [Choosing how CloudFront serves HTTPS requests](cnames-https-dedicated-ip-or-sni.md)\.

1. Choose **Yes, Edit**\.

1. On the **General** tab for the distribution, confirm that **Distribution Status** has changed to **Deployed**\. If you try to use an alternate domain name before the updates to your distribution have been deployed, the links that you create in the following steps might not work\.

1. Configure the DNS service for the alternate domain name \(such as www\.example\.com\) to route traffic to the CloudFront domain name for your distribution \(such as d111111abcdef8\.cloudfront\.net\)\. The method that you use depends on whether you’re using Route 53 as the DNS service provider for the domain or another provider\.
**Note**  
 If your DNS record already points to a distribution that is not the distribution that you are updating, then you only add the alternate domain name to your distribution after you update your DNS\. For more information, see [Restrictions on using alternate domain names](#alternate-domain-names-restrictions)\.   
**Route 53**  
Create an alias resource record set\. With an alias resource record set, you don’t pay for Route 53 queries\. In addition, you can create an alias resource record set for the root domain name \(example\.com\), which DNS doesn’t allow for CNAMEs\. For more information, see [Routing traffic to an Amazon CloudFront web distribution by using your domain name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.  
**Another DNS service provider**  
Use the method provided by your DNS service provider to add a CNAME record for your domain\. This new CNAME record will redirect DNS queries from your alternate domain name \(for example, www\.example\.com\) to the CloudFront domain name for your distribution \(for example, d111111abcdef8\.cloudfront\.net\)\. For more information, see the documentation provided by your DNS service provider\.  
If you already have an existing CNAME record for your alternate domain name, update that record or replace it with a new one that points to the CloudFront domain name for your distribution\.

1. Using `dig` or a similar DNS tool, confirm that the DNS configuration that you created in the previous step points to the domain name for your distribution\.

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

   The answer section shows a CNAME record that routes queries for www\.example\.com to the CloudFront distribution domain name d111111abcdef8\.cloudfront\.net\. If the name on the right side of `CNAME` is the domain name for your CloudFront distribution, the CNAME record is configured correctly\. If it’s any other value, for example, the domain name for your Amazon S3 bucket, then the CNAME record is configured incorrectly\. In that case, go back to step 7 and correct the CNAME record to point to the domain name for your distribution\.

1. Test the alternate domain name by visiting URLs with your domain name instead of the CloudFront domain name for your distribution\.

1. In your application, change the URLs for your objects to use your alternate domain name instead of the domain name of your CloudFront distribution\.

## Moving an alternate domain name to a different distribution<a name="alternate-domain-names-move"></a>

When you try to add an alternate domain name to a distribution but the alternate domain name is already in use on a different distribution, you get a `CNAMEAlreadyExists` error \(One or more of the CNAMEs you provided are already associated with a different resource\)\. For example, you get this error when you attempt to add www\.example\.com to a distribution, but www\.example\.com is already associated with a different distribution\.

In that case, you might want to move the existing alternate domain name from one distribution \(the *source distribution*\) to another \(the *target distribution*\)\. The following steps are an overview of the process\. For more information, follow the link at each step in the overview\.

**To move an alternate domain name**

1. Set up the target distribution\. This distribution must have an SSL/TLS certificate that covers the alternate domain name that you are moving\. For more information, see [Set up the target distribution](#alternate-domain-names-move-create-target)\.

1. Find the source distribution\. You can use the AWS Command Line Interface \(AWS CLI\) to find the distribution that the alternate domain name is associated with\. For more information, see [Find the source distribution](#alternate-domain-names-move-find-source)\.

1. Move the alternate domain name\. The way you do this depends on whether the source and target distributions are in the same AWS account\. For more information, see [Move the alternate domain name](#alternate-domain-names-move-options)\.

### Set up the target distribution<a name="alternate-domain-names-move-create-target"></a>

Before you can move an alternate domain name, you must set up the target distribution \(the distribution that you are moving the alternate domain name to\)\.

**To set up the target distribution**

1. Get an SSL/TLS certificate that includes the alternate domain name that you are moving\. If you don’t have one, you can request one from [AWS Certificate Manager \(ACM\)](https://console.aws.amazon.com/acm), or get one from another certificate authority \(CA\) and import it into ACM\. Make sure that you request or import the certificate in the US East \(N\. Virginia\) \(`us-east-1`\) Region\.

1. If you haven’t created the target distribution, create one now\. As part of creating the target distribution, associate your certificate \(from the previous step\) with the distribution\. For more information, see [Creating a distribution](distribution-web-creating-console.md)\.

   If you already have a target distribution, associate your certificate \(from the previous step\) with the target distribution\. For more information, see [Updating a distribution](HowToUpdateDistribution.md)\.

1. Create a DNS TXT record that associates the alternate domain name with the distribution domain name of the target distribution\. Create your TXT record with an underscore \(\_\) in front of the alternate domain name\. The following shows an example TXT record in DNS:

   `_www.example.com TXT d111111abcdef8.cloudfront.net`

   CloudFront uses this TXT record to validate your ownership of the alternate domain name\.

### Find the source distribution<a name="alternate-domain-names-move-find-source"></a>

Before you move an alternate domain name from one distribution to another, you should find the *source distribution* \(the distribution where the alternate domain name is currently in use\)\. When you know the AWS account ID of both the source and target distributions, you can determine how to move the alternate domain name\.

**To find the source distribution for the alternate domain name**

1. Use the [CloudFront list\-conflicting\-aliases command in the AWS Command Line Interface \(AWS CLI\)](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudfront/list-conflicting-aliases.html) as shown in the following example\. Replace *www\.example\.com* with the alternate domain name, and *EDFDVBD6EXAMPLE* with the ID of the target distribution [that you set up previously](#alternate-domain-names-move-create-target)\. Run this command using credentials that are in the same AWS account as the target distribution\. To use this command, you must have `cloudfront:GetDistribution` and `cloudfront:ListConflictingAlias` permissions on the target distribution\.

   ```
   aws cloudfront list-conflicting-aliases --alias www.example.com --distribution-id EDFDVBD6EXAMPLE
   ```

   The command’s output shows a list of all the alternate domain names that conflict or overlap with the provided one\. For example:
   + If you provide www\.example\.com to the command, the command’s output includes www\.example\.com and the overlapping wildcard alternate domain name \(\*\.example\.com\) if it exists\.
   + If you provide \*\.example\.com to the command, the command’s output includes \*\.example\.com and any alternate domain names covered by that wildcard \(for example, www\.example\.com, test\.example\.com, dev\.example\.com, and so on\)\.

   For each alternate domain name in the command’s output, you can see the ID of the distribution that it’s associated with, and the AWS account ID that owns the distribution\. The distribution and account IDs are partially hidden, which allows you to identify the distributions and accounts that you own, but helps to protect the information of ones that you don’t own\.

1. In the command’s output, find the distribution for the alternate domain name that you are moving, and note the source distribution’s AWS account ID\. Compare the source distribution’s account ID with the account ID where you created the target distribution, and determine whether these two distribution are in the same AWS account\. This helps you determine how to move the alternate domain name\.

   To move the alternate domain name, see the following topic\.

### Move the alternate domain name<a name="alternate-domain-names-move-options"></a>

Depending on your situation, choose from the following ways to move the alternate domain name:

**If the source and target distributions are in the same AWS account**  
Use the associate\-alias command in the AWS CLI to move the alternate domain name\. This method works for all same\-account moves, including when the alternate domain name is an apex domain \(also called a *root domain*, like example\.com\)\. For more information, see [Use associate\-alias to move an alternate domain name](#alternate-domain-names-move-associate-alias)\.

**If the source and target distributions are in different AWS accounts**  
If you have access to the source distribution, the alternate domain name is *not* an apex domain \(also called a root domain, like `example.com`\), and you are not already using a wildcard that overlaps with that alternate domain name, use a wildcard to move the alternate domain name\. For more information, see [Use a wildcard to move an alternate domain name](#alternate-domain-names-move-use-wildcard)\.  
If you don’t have access to the source distribution’s AWS account, you can try to use the `associate-alias` command in the AWS CLI to move the alternate domain name\. If the source distribution is disabled, you can move the alternate domain name\. For more information, see [Use associate\-alias to move an alternate domain name](#alternate-domain-names-move-associate-alias)\.  
If the alternate domain name that you’re moving is an apex domain or you don’t have access to the source distribution’s AWS account, contact AWS Support\. For more information, see [Contact AWS Support to move an alternate domain name](#alternate-domain-names-move-contact-support)\.

#### Use associate\-alias to move an alternate domain name<a name="alternate-domain-names-move-associate-alias"></a>

If the source distribution is in the same AWS account as the target distribution, or if it’s in a different account but disabled, you can use the [CloudFront associate\-alias command in the AWS CLI](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudfront/associate-alias.html) to move the alternate domain name\.

**To use associate\-alias to move an alternate domain name**

1. Use the AWS CLI to run the CloudFront associate\-alias command, as shown in the following example\. Replace *www\.example\.com* with the alternate domain name, and *EDFDVBD6EXAMPLE* with the target distribution ID\. Run this command using credentials that are in the same AWS account as the target distribution\. Note the following restrictions for using this command:
   + You must have `cloudfront:AssociateAlias` and `cloudfront:UpdateDistribution` permissions on the target distribution\.
   + If the source and target distributions are in the same AWS account, you must have `cloudfront:UpdateDistribution` permission on the source distribution\.
   + If the source and target distributions are in different AWS accounts, the source distribution must be disabled\.
   + The target distribution must be set up as described in [Set up the target distribution](#alternate-domain-names-move-create-target)\.

   ```
   aws cloudfront associate-alias --alias www.example.com --target-distribution-id EDFDVBD6EXAMPLE
   ```

   This command updates both distributions by removing the alternate domain name from the source distribution and adding it to the target distribution\.

1. After the target distribution is fully deployed, update your DNS configuration to point the alternate domain name’s DNS record to the distribution domain name of the target distribution\.

#### Use a wildcard to move an alternate domain name<a name="alternate-domain-names-move-use-wildcard"></a>

If the source distribution is in a different AWS account than the target distribution, and the source distribution is enabled, you can use a wildcard to move the alternate domain name\.

**Note**  
You can’t use a wildcard to move an apex domain \(like example\.com\)\. To move an apex domain when the source and target distributions are in different AWS accounts, contact AWS Support\. For more information, see [Contact AWS Support to move an alternate domain name](#alternate-domain-names-move-contact-support)\.

**To use a wildcard to move an alternate domain name**
**Note**  
This process involves multiple updates to your distributions\. Wait for each distribution to fully deploy the latest change before proceeding to the next step\.

1. Update the target distribution to add a wildcard alternate domain name that covers the alternate domain name that you are moving\. For example, if the alternate domain name that you’re moving is www\.example\.com, add the alternate domain name \*\.example\.com to the target distribution\. To do this, the SSL/TLS certificate on the target distribution must include the wildcard domain name\. For more information, see [Updating a distribution](HowToUpdateDistribution.md)\.

1. Update the DNS settings for the alternate domain name to point to the domain name of the target distribution\. For example, if the alternate domain name that you’re moving is www\.example\.com, update the DNS record for www\.example\.com to route traffic to the domain name of the target distribution \(for example d111111abcdef8\.cloudfront\.net\)\.
**Note**  
Even after you update the DNS settings, the alternate domain name is still served by the source distribution because that’s where the alternate domain name is currently configured\.

1. Update the source distribution to remove the alternate domain name\. For more information, see [Updating a distribution](HowToUpdateDistribution.md)\.

1. Update the target distribution to add the alternate domain name\. For more information, see [Updating a distribution](HowToUpdateDistribution.md)\.

1. Use dig \(or a similar DNS query tool\) to validate that the DNS record for the alternate domain name resolves to the domain name of the target distribution\.

1. \(Optional\) Update the target distribution to remove the wildcard alternate domain name\.

#### Contact AWS Support to move an alternate domain name<a name="alternate-domain-names-move-contact-support"></a>

If the alternate domain name that you’re moving is an apex domain, the source and target distributions are in different AWS accounts, and you don’t have access to the source distribution’s AWS account or can’t disable the source distribution, you can contact AWS Support to move the alternate domain name\.

**To contact AWS Support to move an alternate domain name**

1. Set up a target distribution, including the DNS TXT record that points to the target distribution\. For more information, see [Set up the target distribution](#alternate-domain-names-move-create-target)\.

1. [Contact AWS Support](https://console.aws.amazon.com/support/home) to request that they verify that you own the domain, and move the domain to the new CloudFront distribution for you\.

## Removing an alternate domain name<a name="alternate-domain-names-remove-domain"></a>

If you want to stop routing traffic for a domain or subdomain to a CloudFront distribution, follow the steps in this section to update both the DNS configuration and the CloudFront distribution\.

It’s important that you remove the alternate domain names from the distribution as well as update your DNS configuration\. This helps prevent issues later if you want to associate the domain name with another CloudFront distribution\. If an alternate domain name is already associated with one distribution, it can’t be set up with another\.

**Note**  
If you want to remove the alternate domain name from this distribution so you can add it to another one, follow the steps in [Moving an alternate domain name to a different distribution](#alternate-domain-names-move)\. If you follow the steps here instead \(to remove a domain\) and then add the domain to another distribution, there will be a period of time during which the domain won’t link to the new distribution because CloudFront is propagating to the updates to edge locations\.<a name="RemovingADomain"></a>

**To remove an alternate domain name from a distribution**

1. To start, route internet traffic for your domain to another resource that isn’t your CloudFront distribution, such as an Elastic Load Balancing load balancer\. Or you can delete the DNS record that’s routing traffic to CloudFront\.

   Do one of the following, depending on the DNS service for your domain:
   + **If you’re using Route 53**, update or delete alias records or CNAME records\. For more information, see [Editing records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html) or [Deleting records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-deleting.html)\.
   + **If you’re using another DNS service provider**, use the method provided by the DNS service provider to update or delete the CNAME record that directs traffic to CloudFront\. For more information, see the documentation provided by your DNS service provider\.

1. After you update your domain’s DNS records, wait until the changes have propagated and DNS resolvers are routing traffic to the new resource\. You can check to see when this is complete by creating some test links that use your domain in the URL\.

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home), and update your CloudFront distribution to remove the domain name by doing the following:

   1. Choose the ID for the distribution that you want to update\.

   1. On the **General** tab, choose **Edit**\.

   1. In **Alternate Domain Names \(CNAMEs\)**, remove the alternate domain name \(or domain names\) that you no longer want to use for your distribution\.

   1. Choose **Yes, Edit**\.

## Using wildcards in alternate domain names<a name="alternate-domain-names-wildcard"></a>

When you add alternate domain names, you can use the \* wildcard at the beginning of a domain name instead of adding subdomains individually\. For example, with an alternate domain name of \*\.example\.com, you can use any domain name that ends with example\.com in your URLs, such as www\.example\.com, product\-name\.example\.com, marketing\.product\-name\.example\.com, and so on\. The path to the object is the same regardless of the domain name, for example:
+ www\.example\.com/images/image\.jpg
+ product\-name\.example\.com/images/image\.jpg
+ marketing\.product\-name\.example\.com/images/image\.jpg

Follow these requirements for alternate domain names that include wildcards:
+ The alternate domain name must begin with an asterisk and a dot \(\*\.\)\.
+ You *cannot* use a wildcard to replace part of a subdomain name, like this: \*domain\.example\.com\.
+ You cannot replace a subdomain in the middle of a domain name, like this: subdomain\.\*\.example\.com\.
+ All alternate domain names, including alternate domain names that use wildcards, must be covered by the subject alternative name \(SAN\) on the certificate\.

A wildcard alternate domain name, such as \*\.example\.com, can include another alternate domain name that’s in use, such as example\.com\.

## Requirements for using alternate domain names<a name="alternate-domain-names-requirements"></a>

When you add an alternate domain name, such as www\.example\.com, to a CloudFront distribution, the following are requirements:

**Alternate domain names must be lowercase**  
All alternate domain names \(CNAMEs\) must be lowercase\.

**Alternate domain names must be covered by a valid SSL/TLS certificate**  
To add an alternate domain name \(CNAME\) to a CloudFront distribution, you must attach to your distribution a trusted, valid SSL/TLS certificate that covers the alternate domain name\. This ensures that only people with access to your domain’s certificate can associate with CloudFront a CNAME related to your domain\.  
A trusted certificate is one that is issued by AWS Certificate Manager \(ACM\) or by another valid certificate authority \(CA\); you can’t use a self\-signed certificate\. CloudFront supports the same certificate authorities as Mozilla\. For the current list, see [ Mozilla Included CA Certificate List](https://wiki.mozilla.org/CA/Included_Certificates)\.   
To verify an alternate domain name by using the certificate that you attach, including alternate domain names that include wildcards, CloudFront checks the subject alternative name \(SAN\) on the certificate\. The alternate domain name that you’re adding must be covered by the SAN\.  
Only one certificate can be attached to a CloudFront distribution at a time\.
You prove that you are authorized to add a specific alternate domain name to your distribution by doing one of the following:  
+ Attaching a certificate that includes the alternate domain name, like product\-name\.example\.com\.
+ Attaching a certificate that includes a \* wildcard at the beginning of a domain name, to cover multiple subdomains with one certificate\. When you specify a wildcard, you can add multiple subdomains as alternate domain names in CloudFront\.
The following examples illustrate how using wildcards in domain names in a certificate work to authorize you to add specific alternate domain names in CloudFront\.  
+ You want to add marketing\.example\.com as an alternate domain name\. You list in your certificate the following domain name: \*\.example\.com\. When you attach this certificate to CloudFront, you can add any alternate domain name for your distribution that replaces the wildcard at that level, including marketing\.example\.com\. You can also, for example, add the following alternate domain names:
  + product\.example\.com
  + api\.example\.com

  However, you can’t add alternate domain names that are at levels higher or lower than the wildcard\. For example, you can’t add the alternate domain names example\.com or marketing\.product\.example\.com\. 
+ You want to add example\.com as an alternate domain name\. To do this, you must list the domain name example\.com itself on the certificate that you attach to your distribution\.
+ You want to add marketing\.product\.example\.com as an alternate domain name\. To do this, you can list \*\.product\.example\.com on the certificate, or you can list marketing\.product\.example\.com itself on the certificate\.

**Permission to change DNS configuration**  
When you add alternate domain names, you must create CNAME records to route DNS queries for the alternate domain names to your CloudFront distribution\. To do this, you must have permission to create CNAME records with the DNS service provider for the alternate domain names that you’re using\. Typically, this means that you own the domains, but you might be developing an application for the domain owner\.

**Alternate domain names and HTTPS**  
If you want viewers to use HTTPS with an alternate domain name, you must complete some additional configuration\. For more information, see [Using alternate domain names and HTTPS](using-https-alternate-domain-names.md)\.

## Restrictions on using alternate domain names<a name="alternate-domain-names-restrictions"></a>

Note the following restrictions on using alternate domain names:

**Maximum number of alternate domain names**  
For the current maximum number of alternate domain names that you can add to a distribution, or to request a higher quota \(formerly known as limit\), see [General quotas on distributions](cloudfront-limits.md#limits-web-distributions)\.

**Duplicate and overlapping alternate domain names**  
You cannot add an alternate domain name to a CloudFront distribution if the same alternate domain name already exists in another CloudFront distribution, even if your AWS account owns the other distribution\.  
However, you can add a wildcard alternate domain name, such as \*\.example\.com, that includes \(that overlaps with\) a non\-wildcard alternate domain name, such as www\.example\.com\. If you have overlapping alternate domain names in two distributions, CloudFront sends the request to the distribution with the more specific name match, regardless of the distribution that the DNS record points to\. For example, marketing\.domain\.com is more specific than \*\.domain\.com\.

**Domain fronting**  
CloudFront includes protection against domain fronting occurring across different AWS accounts\. Domain fronting is a scenario in which a non\-standard client creates a TLS/SSL connection to a domain name in one AWS account, but then makes an HTTPS request for an unrelated name in another AWS account\. For example, the TLS connection might connect to www\.example\.com, and then send an HTTP request for www\.example\.org\.  
To prevent cases where domain fronting crosses different AWS accounts, CloudFront makes sure that the AWS account that owns the certificate that it serves for a specific connection always matches the AWS account that owns the request that it handles on that same connection\.  
If the two AWS account numbers do not match, CloudFront responds with an HTTP 421 Misdirected Request response to give the client a chance to connect using the correct domain\.

**Adding an alternate domain name at the top node \(zone apex\) for a domain**  
When you add an alternate domain name to a distribution, you typically create a CNAME record in your DNS configuration to route DNS queries for the domain name to your CloudFront distribution\. However, you can’t create a CNAME record for the top node of a DNS namespace, also known as the zone apex; the DNS protocol doesn’t allow it\. For example, if you register the DNS name example\.com, the zone apex is example\.com\. You can’t create a CNAME record for example\.com, but you can create CNAME records for www\.example\.com, newproduct\.example\.com, and so on\.  
If you’re using Route 53 as your DNS service, you can create an alias resource record set, which has two advantages over CNAME records\. You can create an alias resource record set for a domain name at the top node \(example\.com\)\. In addition, when you use an alias resource record set, you don’t pay for Route 53 queries\.  
If you enable IPv6, you must create two alias resource record sets: one to route IPv4 traffic \(an A record\) and one to route IPv6 traffic \(an AAAA record\)\. For more information, see [Enable IPv6](distribution-web-values-specify.md#DownloadDistValuesEnableIPv6) in the topic [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\. 
For more information, see [Routing traffic to an Amazon CloudFront web distribution by using your domain name](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-cloudfront-distribution.html) in the *Amazon Route 53 Developer Guide*\.