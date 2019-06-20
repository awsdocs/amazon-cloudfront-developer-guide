# Getting Started with CloudFront<a name="GettingStarted"></a>

The example in this topic gives you a quick overview of how to use CloudFront to set up a basic configuration that:
+ Stores the original versions of your objects in one Amazon Simple Storage Service \(Amazon S3\) bucket
+ Distributes content such as text or graphics
+ Makes your objects accessible to everyone
+ Uses the CloudFront domain name in URLs for your objects \(for example, `http://d111111abcdef8.cloudfront.net/image.jpg`\)
+ Keeps your objects in CloudFront edge locations for the default duration of 24 hours \(the minimum duration is 0 seconds\)

Most of these options are customizable\. For example, you can store your content on your own web server instead of using an S3 bucket, and you can limit who has access to the content by using signed URLs or cookies\. For information about how to customize your CloudFront distribution options, see [Steps for Creating a Distribution \(Overview\)](distribution-web-creating.md)\.

You only have to complete a few basic steps to start delivering your content by using CloudFront\. The first step is signing up\. After that, you create a CloudFront distribution, and then use the CloudFront domain name in URLs in your web pages or applications to reference the content\.

**Topics**
+ [Prerequisites](#GettingStartedSignup)
+ [Step 1: Upload your content to Amazon S3 and grant object permissions](#GettingStartedUploadContent)
+ [Step 2: Create a CloudFront distribution](#GettingStartedCreateDistribution)
+ [Step 3: Test your links](#GettingStartedAccessingDistributions)

## Prerequisites<a name="GettingStartedSignup"></a>

Before you begin, make sure that you've completed the steps in [Setting Up Amazon CloudFront](setting-up-cloudfront.md)\.

## Step 1: Upload your content to Amazon S3 and grant object permissions<a name="GettingStartedUploadContent"></a>

An Amazon S3 bucket is a container that can contain files \(objects\) or folders\. CloudFront can distribute almost any type of file for you using an Amazon S3 bucket as the source, for example, text, images, and videos\. You can create multiple buckets, and there is no limit to the amount of data that you can store on Amazon S3\.

By default, your Amazon S3 bucket and all of the files in it are private—only the AWS account that created the bucket has permission to read or write the files in it\. If you want to allow anyone to access the files in your Amazon S3 bucket using CloudFront URLs, you must grant public read permissions to the objects\. \(This is one of the most common mistakes when working with CloudFront and Amazon S3\. You must explicitly grant privileges to each object in an Amazon S3 bucket\.\)

**Note**  
If you want to restrict who can download your content, you can use the CloudFront private content feature\. For more information about distributing private content, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

**To upload your content to Amazon S3 and grant read permission to everyone**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the Amazon S3 console, choose **Create Bucket**\.

1. In the **Create Bucket** dialog, on the **Name and region** page, do the following:

   1. Enter a bucket name\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

   1. Select an AWS Region for your bucket\. By default, Amazon S3 creates buckets in the US East \(N\. Virginia\) Region\. We recommend that you choose a Region close to you to optimize latency and minimize costs, or you might choose another Region to address regulatory requirements\.

1. Choose **Next**\.

1. On the **Configure options** page, choose options for versioning, tagging, and other features\.

1. Choose **Next**\.

1. On the **Set permissions** page, clear the following checkbox:
   + **Block all public access**

   You must allow public read access to the bucket and files so that CloudFront URLs can serve content from the bucket\. However, you can restrict access to specific content by using the CloudFront private content feature\. For more information, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

1. Choose **Next**, and then choose **Create bucket**\.

1. Choose your bucket in the **Buckets** pane, and then choose **Upload**\.

1. On the **Select files** page, drag and drop your files to the bucket, or choose **Add files**, and choose the files that you want to upload\.

1. Choose **Next**\.

1. On the **Set permissions** page, grant public read privileges for each file that you upload to your Amazon S3 bucket\. 

   1. Choose **Next** to set permissions\.

   1. In the **Manage public permissions** drop\-down list, choose **Grant public read access to this object\(s\)**\.

   1. Choose **Next**\.

1. Set any properties that you want for the object, such as encryption or tagging, and then choose **Next**\.

1. Choose **Upload**\.

   After the upload completes, you can navigate to the item by using its URL\. In the case of the previous example, the URL would be:

   `http://s3-myregion.amazonaws.com/example-myawsbucket/filename`

   Use your Amazon S3 URL to verify that your content is publicly accessible, but remember that this is not the URL you'll use when you're ready to distribute your content with CloudFront\.

## Step 2: Create a CloudFront distribution<a name="GettingStartedCreateDistribution"></a><a name="GettingStartedCreateDistributionProcedure"></a>

**To create a CloudFront distribution**

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the **Select a delivery method for your content** page, in the **Web** section or the **RTMP** section, choose **Get Started**\.
   + For most scenarios, you create a web distribution\.
   + To create a distribution to stream media files using Adobe Media Server and the Adobe Real\-Time Messaging Protocol \(RTMP\), you create an RTMP distribution\. For more information, see [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)\.

1. On the **Create Distribution** page, under **Origin Settings**, choose the Amazon S3 bucket that you created earlier\. For **Origin ID**, **Origin Path**, **Restrict Bucket Access**, and **Origin Custom Headers**, accept the default values\.  
![\[Specify the Amazon S3 bucket.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/create-download-dist-2-origin-s3-get-started.png)

1. Under **Default Cache Behavior Settings**, accept the default values, and CloudFront will:
   + Forward all requests that use the CloudFront URL for your distribution \(for example, `http://d111111abcdef8.cloudfront.net/image.jpg`\) to the Amazon S3 bucket that you specified in Step 4\.
   + Allow end users to use either HTTP or HTTPS to access your objects\.
   + Respond to requests for your objects\.
   + Cache your objects at CloudFront edge locations for 24 hours\.
   + Forward only the default request headers to your origin and not cache your objects based on the values in the headers\.
   + Exclude cookies and query string parameters, if any, when forwarding requests for objects to your origin\. \(Amazon S3 doesn't process cookies and processes only a limited set of query string parameters\.\)
   + Not be configured to distribute media files in the Microsoft Smooth Streaming format\.
   + Allow everyone to view your content\.
   + Not automatically compress your content\.

   For more information about cache behavior options, see [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)\.  
![\[Define cache behavior.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/create-download-dist-3-cache-behavior-get-started.png)

1. Under **Distribution Settings**, choose the values for your distribution:  
**Price Class**  
Select the price class that corresponds with the maximum price that you want to pay for CloudFront service\. By default, CloudFront serves your objects from edge locations in all CloudFront regions\.   
For more information about price classes and about how your choice of price class affects CloudFront performance for your distribution, go to [Choosing the Price Class for a CloudFront Distribution](PriceClass.md)\. For information about CloudFront pricing, including how price classes map to CloudFront regions, go to [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.  
**AWS WAF Web ACL**  
If you want to use AWS WAF to allow or block HTTP and HTTPS requests based on criteria that you specify, choose the web ACL to associate with this distribution\. For more information about AWS WAF, see the [AWS WAF Developer Guide](http://docs.aws.amazon.com/waf/latest/developerguide/)\.  
**Alternate Domain Names \(CNAMEs\) \(Optional\)**  
Specify one or more domain names that you want to use for URLs for your objects instead of the domain name that CloudFront assigns when you create your distribution\. For example, if you want the URL for the object:  
`/images/image.jpg`  
to look like this:   
`http://www.example.com/images/image.jpg`  
instead of like this:  
`http://d111111abcdef8.cloudfront.net/images/image.jpg`  
you would create a CNAME for `www.example.com`\.  
If you add a CNAME for `www.example.com` to your distribution, you also need to create \(or update\) a CNAME record with your DNS service to route queries for `www.example.com` to `d111111abcdef8.cloudfront.net`\. You must have permission to create a CNAME record with the DNS service provider for the domain\. Typically, this means that you own the domain, but you may also be developing an application for the domain owner\. For more information about CNAMEs, see [Using Custom URLs for Files by Adding Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.
For the current limit on the number of alternate domain names that you can add to a distribution or request a higher limit, see [General Limits on Web Distributions](cloudfront-limits.md#limits-web-distributions)\.  
**SSL Certificate**  
Accept the default value, **Default CloudFront Certificate**\.   
**Default Root Object \(Optional\)**  
The object that you want CloudFront to request from your origin \(for example, `index.html`\) when a viewer requests the root URL of your distribution \(`http://www.example.com/`\) instead of an object in your distribution \(`http://www.example.com/product-description.html`\)\. Specifying a default root object avoids exposing the contents of your distribution\.   
**Logging \(Optional\)**  
If you want CloudFront to log information about each request for an object and store the log files in an Amazon S3 bucket, select **On**, and specify the bucket and an optional prefix for the names of the log files\. There is no extra charge to enable logging, but you accrue the usual Amazon S3 charges for storing and accessing the files\. CloudFront doesn't delete the logs automatically, but you can delete them at any time\.   
**Cookie Logging**  
In this example, we're using Amazon S3 as the origin for your objects, and Amazon S3 doesn't process cookies, so we recommend that you select **Off** for the value of **Cookie Logging**\.  
**Comment \(Optional\)**  
Enter any comments that you want to save with the distribution\.  
**Distribution State**  
Select **Enabled** if you want CloudFront to begin processing requests as soon as the distribution is created, or select **Disabled** if you do not want CloudFront to begin processing requests after the distribution is created\.   
![\[Distribution settings\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/create-download-dist-4-distribution-details.png)

1. Choose **Create Distribution**\.

1. After CloudFront has created your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. If you chose to enable the distribution, it will then be ready to process requests\. This typically takes between 20 and 40 minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. \(It also appears on the **General** tab for a selected distribution\.\)

## Step 3: Test your links<a name="GettingStartedAccessingDistributions"></a>

After you've created your distribution, CloudFront knows where your Amazon S3 origin server is, and you know the domain name associated with the distribution\. You can create a link to your Amazon S3 bucket content with that domain name, and have CloudFront serve it\. 

**Note**  
You must wait until the status of your distribution changes to **Deployed** before testing your links\.

****<a name="GettingStartedAccessingDistributionsProcedure"></a>

**To link to your objects**

1. Copy the following HTML into a new file:
   + Replace <domain name> with the domain name that CloudFront assigned to your distribution\.
   + Replace <object name> with the name of a file in your Amazon S3 bucket\.

   ```
   1. <html>
   2. <head>My CloudFront Test</head>
   3. <body>
   4. <p>My text content goes here.</p>
   5. <p><img src="http://domain name/object name" alt="my test image"/>
   6. </body>
   7. </html>
   ```

   For example, if your domain name was `d111111abcdef8.cloudfront.net` and your object was `image.jpg`, the URL for the link would be:

   `http://d111111abcdef8.cloudfront.net/image.jpg`\.

   If your object is in a folder within your bucket, include the folder in the URL\. For example, if image\.jpg is located in an images folder, then the URL would be:

   http://d111111abcdef8\.cloudfront\.net/images/image\.jpg

1. Save the text in a file that has a \.html filename extension\.

1. Open your web page in a browser to ensure that you can see your content\. If you cannot see the content, confirm that you have performed all of the steps correctly\. You can also see the tips in [Troubleshooting](Troubleshooting.md)\.

The browser returns your page with the embedded image file, served from the edge location that CloudFront determined was appropriate to serve the object\. 

For more information on using CloudFront, go to [Amazon CloudFront Related Information](Resources.md)\.