# Values that CloudFront Displays in the Console When You Create or Update an RTMP Distribution<a name="distribution-rtmp-values-returned"></a>

**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

When you create a new RTMP distribution or update an existing distribution, CloudFront displays the following information in the CloudFront console\.

**Note**  
Active trusted signers, the AWS accounts that have an active CloudFront key pair and can be used to create valid signed URLs, are currently not visible in the CloudFront console\.

## Distribution ID<a name="StreamingDistReturnValuesID"></a>

When you perform an action on a distribution using the CloudFront API, you use the distribution ID to specify which distribution you want to perform the action on, for example, `EDFDVBD6EXAMPLE`\. You can't change the distribution ID\.

## Status<a name="StreamingDistReturnValuesStatus"></a>

The possible status values for a distribution are listed in the following table\.


****  

| Value | Description | 
| --- | --- | 
| **InProgress** | The distribution is still being created or updated\. | 
| **Deployed** | The distribution has been created or updated and the changes have been fully propagated through the CloudFront system\.  | 

In addition to ensuring that the status for a distribution is **Deployed**, you must enable the distribution before end users can use CloudFront to access your content\. For more information, see [Distribution State](distribution-rtmp-values-specify.md#StreamingDistValuesEnabled)\. 

## Last Modified<a name="StreamingDistReturnValuesLastModDate"></a>

The date and time that the distribution was last modified, using ISO 8601 format, for example, 2012\-05\-19T19:37:58Z\. For more information, go to [http://www\.w3\.org/TR/NOTE\-datetime](http://www.w3.org/TR/NOTE-datetime)\. 

## Domain Name<a name="StreamingDistReturnValuesDomainName"></a>

You use the distribution's domain name in the links to your objects, unless you're using alternate domain names \(CNAMEs\)\. For example, if your distribution's domain name is `d111111abcdef8.cloudfront.net`, the link to the example `/images/image.jpg` file would be `http://d111111abcdef8.cloudfront.net/images/image.jpg`\. You can't change the CloudFront domain name for your distribution\. For more information about CloudFront URLs for links to your objects, see [Customizing the URL Format for Files in CloudFront](LinkFormat.md)\.

If you specified one or more alternate domain names \(CNAMEs\), you can use your own domain names for links to your objects instead of using the CloudFront domain name\. For more information about CNAMEs, see [Alternate Domain Names \(CNAMEs\)](distribution-web-values-specify.md#DownloadDistValuesCNAME)\. 

**Note**  
CloudFront domain names are unique\. Your distribution's domain name was never used for a previous distribution and will never be reused for another distribution in the future\.