# Switching from a Custom SSL/TLS Certificate with Dedicated IP Addresses to SNI<a name="cnames-and-https-switch-dedicated-to-sni"></a>

If you configured CloudFront to use a custom SSL/TLS certificate with dedicated IP addresses, you can switch to using a custom SSL/TLS certificate with SNI instead and eliminate the charge that is associated with dedicated IP addresses\. The following procedure shows you how\.

**Important**  
This update to your CloudFront configuration has no effect on viewers that support SNI\. Viewers can access your content before and after the change, as well as while the change is propagating to CloudFront edge locations\. Viewers that don't support SNI cannot access your content after the change\. For more information, see [Choosing How CloudFront Serves HTTPS Requests](cnames-https-dedicated-ip-or-sni.md)\. <a name="cnames-and-https-switch-dedicated-to-sni-proc"></a>

**To switch from a custom SSL/TLS certificate with dedicated IP addresses to SNI**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose the ID of the distribution that you want to view or update\.

1. Choose **Distribution Settings**\.

1. On the **General** tab, choose **Edit**\.

1. Change the setting of **Custom SSL Client Support** to **Only Clients that Support Server Name Indication \(SNI\)**\.

1. Choose **Yes, Edit**\.