# Getting started with a secure static website<a name="getting-started-secure-static-website-cloudformation-template"></a>

You can get started with Amazon CloudFront by using the solution described in this topic to create a secure static website for your domain name\. A *static website* uses only static files—like HTML, CSS, JavaScript, images, and videos—and doesn’t need servers or server\-side processing\. With this solution, your website gets the following benefits:
+ **Uses the durable storage of [Amazon Simple Storage Service \(Amazon S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html)** – This solution creates an Amazon S3 bucket to host your static website’s content\. To update your website, just upload your new files to the S3 bucket\.
+ **Is sped up by the Amazon CloudFront content delivery network** – This solution creates a CloudFront distribution to serve your website to viewers with low latency\. The distribution is configured with an [origin access identity](private-content-restricting-access-to-s3.md) to make sure that the website is accessible only through CloudFront, not directly from S3\.
+ **Is secured by HTTPS and additional security headers** – This solution creates an SSL/TLS certificate in [AWS Certificate Manager \(ACM\)](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html), and attaches it to the CloudFront distribution\. This certificate enables the distribution to serve your domain’s website securely with HTTPS\.

  This solution also uses [Lambda@Edge](lambda-at-the-edge.md) to add security headers to every server response\. Security headers are a group of headers in the web server response that tell web browsers to take extra security precautions\. For more information, refer to this blog post: [Adding HTTP Security Headers Using Lambda@Edge and Amazon CloudFront](http://aws.amazon.com/blogs/networking-and-content-delivery/adding-http-security-headers-using-lambdaedge-and-amazon-cloudfront/)\.
+ **Is configured and deployed with [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)** – This solution uses an AWS CloudFormation template to set up all the components, so you can focus more on your website’s content and less on configuring components\.

This solution is open source on GitHub\. To view the code, submit a pull request, or open an issue, go to [https://github.com/aws-samples/amazon-cloudfront-secure-static-site](https://github.com/aws-samples/amazon-cloudfront-secure-static-site)\.

**Topics**
+ [Solution overview](#cloudformation-website-overview)
+ [Deploying the solution](#deploy-secure-static-website-cloudformation)

## Solution overview<a name="cloudformation-website-overview"></a>

The following diagram shows an overview of how this static website solution works:

![\[Overview diagram of a secure static website with CloudFront\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/cloudfront-secure-static-website-overview.png)

1. The viewer requests the website at www\.example\.com\.

1. If the requested object is cached, CloudFront returns the object from its cache to the viewer\.

1. If the object is not in CloudFront’s cache, CloudFront requests the object from the origin \(an S3 bucket\)\.

1. S3 returns the object to CloudFront, which triggers the Lambda@Edge [origin response event](lambda-cloudfront-trigger-events.md)\.

1. The object, including the security headers added by the Lambda@Edge function, is added to CloudFront’s cache\.

1. \(Not shown\) The objects is returned to the viewer\. Subsequent requests for the object that come to the same CloudFront edge location are served from the CloudFront cache\.

## Deploying the solution<a name="deploy-secure-static-website-cloudformation"></a>

To deploy this secure static website solution, you can choose from either of the following options:
+ Use the AWS CloudFormation console to deploy the solution with default content, then upload your website content to Amazon S3\.
+ Clone the solution to your computer to add your website content\. Then, deploy the solution with the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [Prerequisites](#deploy-website-cloudformation-prerequisites)
+ [Using the AWS CloudFormation console](#deploy-website-cloudformation-console)
+ [Cloning the solution locally](#deploy-website-cloudformation-clone)
+ [Finding access logs](#deploy-website-cloudformation-logs)

### Prerequisites<a name="deploy-website-cloudformation-prerequisites"></a>

To use this solution, you must have the following prerequisites:
+ A registered domain name, such as example\.com, that’s pointed to an Amazon Route 53 hosted zone\. The hosted zone must be in the same AWS account where you deploy this solution\. If you don’t have a registered domain name, you can [register one with Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar.html)\. If you have a registered domain name but it’s not pointed to a Route 53 hosted zone, [configure Route 53 as your DNS service](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html)\.
+ AWS Identity and Access Management \(IAM\) permissions to launch CloudFormation templates that create IAM roles, and permissions to create all the AWS resources in the solution\.

You are responsible for the costs incurred while using this solution\. For more information about costs, see [the pricing pages for each AWS service](http://aws.amazon.com/pricing/)\.

### Using the AWS CloudFormation console<a name="deploy-website-cloudformation-console"></a>

**To deploy using the CloudFormation console**

1. Choose **Launch on AWS** to open this solution in the AWS CloudFormation console\. If necessary, sign in to your AWS account\.

   [https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=amazon-cloudfront-secure-static-site-templates-main&templateURL=https://s3.amazonaws.com/solution-builders-us-east-1/amazon-cloudfront-secure-static-site/latest/main.yaml](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=amazon-cloudfront-secure-static-site-templates-main&templateURL=https://s3.amazonaws.com/solution-builders-us-east-1/amazon-cloudfront-secure-static-site/latest/main.yaml)

1. The **Create stack** wizard opens in the AWS CloudFormation console, with prepopulated fields that specify this solution’s CloudFormation template\.

   At the bottom of the page, choose **Next**\.

1. On the **Specify stack details** page, enter values for the following fields:
   + **SubDomain** – Enter the subdomain to use for your website\. For example, if the subdomain is *www*, your website is available at *www*\.example\.com\. \(Replace example\.com with your domain name, as explained in the following bullet\.\)
   + **DomainName** – Enter your domain name, such as *example\.com*\. This domain must be pointed to a Route 53 hosted zone\.

   When finished, choose **Next**\.

1. \(Optional\) On the **Configure stack options** page, [add tags and other stack options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html)\.

   When finished, choose **Next**\.

1. On the **Review** page, scroll to the bottom of the page, then select the two boxes in the **Capabilities** section\. These capabilities allow AWS CloudFormation to create an IAM role that allows access to the stack’s resources, and to name the resources dynamically\.

1. Choose **Create stack**\.

1. Wait for the stack to finish creating\. The stack creates some nested stacks, and can take several minutes to finish\. When it’s finished, the **Status** changes to **CREATE\_COMPLETE**\.

   When the status is **CREATE\_COMPLETE**, go to https://*www\.example\.com* to view your website \(replace www\.example\.com with the subdomain and domain name that you specified in step 3\)\. You should see the website’s default content:  
![\[This solution’s static website default content. It says: “I am a static website!”\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/cloudfront-secure-static-website-content.png)

**To replace the website’s default content with your own**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket whose name begins with **amazon\-cloudfront\-secure\-static\-site\-s3bucketroot\-**\.
**Note**  
Make sure to choose the bucket with **s3bucketroot** in its name, not **s3bucketlogs**\. The bucket with **s3bucketroot** in its name contains the website content\. The one with **s3bucketlogs** contains only log files\.

1. Delete the website’s default content, then upload your own\.
**Note**  
If you viewed your website with this solution’s default content, then it’s likely that some of the default content is cached in a CloudFront edge location\. To make sure that viewers see your updated website content, *invalidate* the files to remove the cached copies from CloudFront edge locations\. For more information, see [Invalidating Files](Invalidation.md)\.

### Cloning the solution locally<a name="deploy-website-cloudformation-clone"></a>

**Prerequisites**

To add your website content before deploying this solution, you must package the solution’s artifacts locally, which requires Node\.js and npm\. For more information, see [https://www.npmjs.com/get-npm](https://www.npmjs.com/get-npm)\.

**To add your website content and deploy the solution**

1. Clone or download the solution from [https://github.com/aws-samples/amazon-cloudfront-secure-static-site](https://github.com/aws-samples/amazon-cloudfront-secure-static-site)\. After you clone or download it, open a command prompt or terminal and navigate to the `amazon-cloudfront-secure-static-site` folder\.

1. Run the following command to install and package the solution’s artifacts:

   ```
   make package-function
   ```

1. Copy your website’s content into the `www` folder, overwriting the default website content\.

1. Run the following AWS CLI command to create an Amazon S3 bucket to store the solution’s artifacts\. Replace *example\-bucket\-for\-artifacts* with your own bucket name\.

   ```
   aws s3 mb s3://example-bucket-for-artifacts --region us-east-1
   ```

1. Run the following AWS CLI command to package the solution’s artifacts as an AWS CloudFormation template\. Replace *example\-bucket\-for\-artifacts* with the name of the bucket that you created in the previous step\.

   ```
   aws cloudformation package \
       --region us-east-1 
       --template-file templates/main.yaml \
       --s3-bucket example-bucket-for-artifacts \
       --output-template-file packaged.template
   ```

1. Run the following command to deploy the solution with AWS CloudFormation, replacing the following values:
   + *your\-CloudFormation\-stack\-name* – Replace with a name for the AWS CloudFormation stack\.
   + *example\.com* – Replace with your domain name\. This domain must be pointed to a Route 53 hosted zone in the same AWS account\.
   + *www* – Replace with the subdomain to use for your website\. For example, if the subdomain is *www*, your website is available at www\.example\.com\.

   ```
   aws cloudformation deploy \
       --region us-east-1
       --stack-name your-CloudFormation-stack-name \
       --template-file packaged.template \
       --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
       --parameter-overrides DomainName=example.com SubDomain=www
   ```

1. Wait for the AWS CloudFormation stack to finish creating\. The stack creates some nested stacks, and can take several minutes to finish\. When it’s finished, the **Status** changes to **CREATE\_COMPLETE**\.

   When the status changes to **CREATE\_COMPLETE**, go to https://www\.example\.com to view your website \(replace www\.example\.com with the subdomain and domain name that you specified in the previous step\)\. You should see your website’s content\.

### Finding access logs<a name="deploy-website-cloudformation-logs"></a>

This solution enables [access logs](AccessLogs.md) for the CloudFront distribution\. Complete the following steps to locate the distribution’s access logs\.

**To locate the distribution’s access logs**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket whose name begins with **amazon\-cloudfront\-secure\-static\-site\-s3bucketlogs\-**\.
**Note**  
Make sure to choose the bucket with **s3bucketlogs** in its name, not **s3bucketroot**\. The bucket with **s3bucketlogs** in its name contains log files\. The one with **s3bucketroot** contains the website content\.

1. The folder named **cdn** contains the CloudFront access logs\.