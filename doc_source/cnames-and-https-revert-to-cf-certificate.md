# Reverting from a Custom SSL/TLS Certificate to the Default CloudFront Certificate<a name="cnames-and-https-revert-to-cf-certificate"></a>

If you configured CloudFront to use HTTPS between viewers and CloudFront, and you configured CloudFront to use a custom SSL/TLS certificate, you can change your configuration to use the default CloudFront SSL/TLS certificate\. The process depends on whether you've used your distribution to distribute your content:
+ If you have not used your distribution to distribute your content, you can just change the configuration\. For more information, see [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md)\.
+ If you have used your distribution to distribute your content, you need to create a new CloudFront distribution and change the URLs for your objects to reduce or eliminate the amount of time that your content is unavailable\. To do that, perform the following procedure\.<a name="cnames-and-https-revert-to-cf-certificate-proc"></a>

**To revert to the default CloudFront certificate**

1. Create a new CloudFront distribution with the desired configuration\. For **SSL Certificate**, choose **Default CloudFront Certificate \(\*\.cloudfront\.net\)**\. 

   For more information, see [Task List for Creating a Web Distribution](distribution-web-creating.md)\.

1. For objects that you're distributing using CloudFront, update the URLs in your application to use the domain name that CloudFront assigned to the new distribution\. For example, change `https://www.example.com/images/logo.png` to `https://d111111abcdef8.cloudfront.net/images/logo.png`\.

1. Either delete the distribution that is associated with a custom SSL/TLS certificate, or update the distribution to change the value of **SSL Certificate** to **Default CloudFront Certificate \(\*\.cloudfront\.net\)**\. For more information, see [Viewing and Updating CloudFront Distributions](HowToUpdateDistribution.md)\.
**Important**  
Until you complete this step, AWS continues to charge you for using a custom SSL/TLS certificate\.

1. \(Optional\) Delete your custom SSL/TLS certificate\.

   1. Run the AWS CLI command `list-server-certificates` to get the certificate ID of the certificate that you want to delete\. For more information, see [list\-server\-certificates](http://docs.aws.amazon.com/cli/latest/reference/iam/list-server-certificates.html) in the *AWS CLI Command Reference*\.

   1. Run the AWS CLI command `delete-signing-certificate` to delete the certificate\. For more information, see [delete\-signing\-certificate](http://docs.aws.amazon.com/cli/latest/reference/iam/delete-signing-certificate.html) in the *AWS CLI Command Reference*\.