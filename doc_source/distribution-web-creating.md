# Steps for Creating a Distribution \(Overview\)<a name="distribution-web-creating"></a>

The following task list summarizes the process for creating a distribution\.<a name="create-download-distribution-task-list"></a>

**To Create a Distribution**

1. Create one or more Amazon S3 buckets or configure HTTP servers as your origin servers\. An origin is the location where you store the original version of your content\. When CloudFront gets a request for your files, it goes to the origin to get the files that it distributes at edge locations\. You can use any combination of Amazon S3 buckets and HTTP servers as your origin servers\.

   If you're using Amazon S3, note that the name of your bucket must be all lowercase and cannot contain spaces\.

   If you're using an Amazon EC2 server or another custom origin, review [Using Amazon EC2 or Other Custom Origins](DownloadDistS3AndCustomOrigins.md#concept_CustomOrigin)\.

   For the current maximum number of origins that you can create for a distribution, or to request a higher quota \(formerly known as limit\), see [General Quotas on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.

1. Upload your content to your origin servers\. If you don't want to restrict access to your content using CloudFront signed URLs, make the objects publicly readable\. 
**Important**  
You are responsible for ensuring the security of your origin server\. You must ensure that CloudFront has permission to access the server and that the security settings are appropriate to safeguard your content\.

1. Create your CloudFront distribution:
   + For more information about creating a distribution using the CloudFront console, see [Creating a Distribution](distribution-web-creating-console.md)\.
   + For information about creating a distribution using the CloudFront API, go to [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html) in the *Amazon CloudFront API Reference*\.

1. Optional: If you created your distribution using the CloudFront console, create more cache behaviors or origins for your distribution\. For more information, see [To Update a CloudFront Distribution](HowToUpdateDistribution.md#HowToUpdateDistributionProcedure)\.

1. Test your distribution\. For more information, see [Testing a Distribution](distribution-web-testing.md)\.

1. Develop your website or application to access your content using the domain name that CloudFront returned after you created your distribution in Step 3\. For example, if CloudFront returns d111111abcdef8\.cloudfront\.net as the domain name for your distribution, the URL for the file `image.jpg` in an Amazon S3 bucket or in the root directory on an HTTP server will be `http://d111111abcdef8.cloudfront.net/image.jpg`\. 

   If you specified one or more alternate domain names \(CNAMEs\) when you created your distribution, you can use your own domain name\. In that case, the URL for `image.jpg` might be `http://www.example.com/image.jpg`\.

   Note the following:
   + If you want to use signed URLs to restrict access to your content, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\. 
   + If you want to serve compressed content, see [Serving compressed files](ServingCompressedFiles.md)\.
   + For information about CloudFront request and response behavior for Amazon S3 and custom origins, see [Request and Response Behavior](RequestAndResponseBehavior.md)\.