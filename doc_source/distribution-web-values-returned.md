# Values That CloudFront Displays in the Console<a name="distribution-web-values-returned"></a>

When you create a new distribution or update an existing distribution, CloudFront displays the following information in the CloudFront console\.

**Note**  
Active trusted signers, the AWS accounts that have an active CloudFront key pair and can be used to create valid signed URLs, are currently not visible in the CloudFront console\.

## Distribution ID \(General Tab\)<a name="DownloadDistReturnID"></a>

When you perform an action on a distribution using the CloudFront API, you use the distribution ID to specify which distribution to use, for example, `EDFDVBD6EXAMPLE`\. You can't change a distribution's distribution ID\.

## Distribution Status \(General Tab\)<a name="DownloadDistReturnStatus"></a>

The possible status values for a distribution are listed in the following table\.


****  

| Value | Description | 
| --- | --- | 
| **InProgress** | The distribution is still being created or updated, and the changes have not yet fully propagated to edge servers\. | 
| **Deployed** | The distribution has been created or updated and the changes have been fully propagated through the CloudFront system\.  | 

**Note**  
In addition to ensuring that the status for a distribution is **Deployed**, you must enable the distribution before users can use CloudFront to access your content\. For more information, see [Distribution State](distribution-web-values-specify.md#DownloadDistValuesEnabled)\. 

## Last Modified \(General Tab\)<a name="DownloadDistReturnLastModDate"></a>

The date and time that the distribution was last modified, using ISO 8601 format, for example, 2012\-05\-19T19:37:58Z\. For more information, see [http://www\.w3\.org/TR/NOTE\-datetime](http://www.w3.org/TR/NOTE-datetime)\. 

## Domain Name \(General Tab\)<a name="DownloadDistReturnDomainName"></a>

You use the distribution's domain name in the links to your objects\. For example, if your distribution's domain name is `d111111abcdef8.cloudfront.net`, the link to `/images/image.jpg` would be `http://d111111abcdef8.cloudfront.net/images/image.jpg`\. You can't change the CloudFront domain name for your distribution\. For more information about CloudFront URLs for links to your objects, see [Customizing the URL Format for Files in CloudFront](LinkFormat.md)\.

If you specified one or more alternate domain names \(CNAMEs\), you can use your own domain names for links to your objects instead of using the CloudFront domain name\. For more information about CNAMEs, see [Alternate Domain Names \(CNAMEs\)](distribution-web-values-specify.md#DownloadDistValuesCNAME)\. 

**Note**  
CloudFront domain names are unique\. Your distribution's domain name was never used for a previous distribution and will never be reused for another distribution in the future\.