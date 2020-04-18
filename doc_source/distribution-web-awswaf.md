# Using AWS WAF to Control Access to Your Content<a name="distribution-web-awswaf"></a>

AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to CloudFront, and lets you control access to your content\. Based on conditions that you specify, such as the values of query strings or the IP addresses that requests originate from, CloudFront responds to requests either with the requested content or with an HTTP status code `403 (Forbidden)`\. You can also configure CloudFront to return a custom error page when a request is blocked\. For more information about AWS WAF, see the [AWS WAF Developer Guide](http://docs.aws.amazon.com/waf/latest/developerguide/)\. 

After you create an AWS WAF web access control list \(web ACL\), create or update a web distribution to associate the distribution with the web ACL\. You can associate as many CloudFront distributions as you want with the same web ACL or with different web ACLs\. For information about creating a web distribution and associating it with a web ACL, see [Creating a Distribution](distribution-web-creating-console.md)\.

To associate or disassociate a web ACL and an existing distribution, or change the web ACL that is associated with a distribution, perform the following procedure\.<a name="distribution-web-awswaf-procedure"></a>

**To associate or disassociate an AWS WAF web ACL and an existing CloudFront distribution by using the CloudFront console**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID for the distribution that you want to update\.

1. On the **General** tab, choose **Edit**\.

1. On the **Distribution Settings** page, in the **AWS WAF Web ACL** list, choose the web ACL that you want to associate with this distribution\.

   If you want to disassociate the distribution from all web ACLs, choose **None**\. If you want to associate the distribution with a different web ACL, choose the new web ACL\.

1. Choose **Yes, Edit**\.

1. Repeat steps 2 through 5 for other distributions, if any, for which you want to add, delete, or change associations with AWS WAF web ACLs\.

1. After you change settings, the value of the **Status** column for the distributions that you updated changes to **InProgress** while CloudFront propagates the changes to edge locations\. When **Status** changes to **Deployed** for a distribution, the distribution is ready to use AWS WAF when it processes requests\. \(The value of the **State** column for the distribution must also be **Enabled**\.\) This should take less than 15 minutes after you save the last change to a distribution\.

**Note**  
[AWS Firewall Manager](https://aws.amazon.com/firewall-manager/) is a security management service that makes it easier to centrally configure and manage AWS WAF rules across your accounts and applications\. Using Firewall Manager, you can roll out AWS WAF rules to your CloudFront distributions across accounts in AWS Organizations\. For more information, see the [AWS Firewall Manager Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/fms-chapter.html)\.