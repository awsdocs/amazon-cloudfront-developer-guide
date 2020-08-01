# Getting started with a simple CloudFront distribution<a name="GettingStarted.SimpleDistribution"></a>

The procedures in this section show you how to use CloudFront to set up a basic configuration that does the following:
+ Stores the original versions of your objects in one Amazon Simple Storage Service \(Amazon S3\) bucket
+ Distributes content such as text or graphics
+ Makes your objects accessible to everyone
+ Uses the CloudFront domain name in URLs for your objects \(for example, `http://d111111abcdef8.cloudfront.net/image.jpg`\)
+ Keeps your objects in CloudFront edge locations for the default duration of 24 hours \(the minimum duration is 0 seconds\)

Most of these options are customizable\. For example, you can store your content on your own web server instead of using an S3 bucket, and you can restrict who has access to the content by using signed URLs or cookies\. For information about how to customize your CloudFront distribution options, see [Steps for Creating a Distribution \(Overview\)](distribution-web-creating.md)\.

You have to complete only a few basic steps to start delivering your content with CloudFront\. The first step is signing up\. After that, you create a CloudFront distribution, and then use the CloudFront domain name in URLs in your webpages or applications to reference the content\.

**Topics**
+ [Prerequisites](#GettingStartedSignup)
+ [Step 1: Upload your content to Amazon S3 and grant object permissions](#GettingStartedUploadContent)
+ [Step 2: Create a CloudFront distribution](#GettingStartedCreateDistribution)
+ [Step 3: Test your links](#GettingStartedAccessingDistributions)

## Prerequisites<a name="GettingStartedSignup"></a>

Before you begin, make sure that you've completed the steps in [Setting up Amazon CloudFront](setting-up-cloudfront.md)\.

## Step 1: Upload your content to Amazon S3 and grant object permissions<a name="GettingStartedUploadContent"></a>

An Amazon S3 bucket is a container that can contain files \(objects\) or folders\. CloudFront can distribute almost any type of file for you using an Amazon S3 bucket as the source\. For example, CloudFront can distribute text, images, and videos\. You can create multiple buckets, and there is no maximum for the amount of data that you can store on Amazon S3\.

By default, your Amazon S3 bucket and all the files in it are private—only the AWS account that created the bucket has read/write permission to the files\. If you want to allow anyone to access the files in your Amazon S3 bucket using CloudFront URLs, you must grant public read permissions to the objects\. \(This is one of the most common mistakes when working with CloudFront and Amazon S3\. You must explicitly grant permissions to each object in an Amazon S3 bucket\.\)

**Note**  
If you want to restrict who can download your content, you can use the CloudFront private content feature\. For more information about distributing private content, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

**To upload your content to Amazon S3 and grant read permissions to everyone**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. On the Amazon S3 console, choose **Create bucket**\.

1. In the **Create bucket** dialog box, on the **Name and region** page, do the following:

   1. Enter a bucket name\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, see [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

   1. For **Region**, choose an AWS Region for your bucket\. By default, Amazon S3 creates buckets in the US East \(N\. Virginia\) Region\. We recommend that you choose a Region close to you to optimize latency and minimize costs, or you might choose another Region to address regulatory requirements\.

1. Choose **Next**\.

1. On the **Configure options** page, choose options for versioning, tagging, and other features\.

1. Choose **Next**\.

1. On the **Set permissions** page, clear the following check box:
   + **Block all public access**

   You must allow public read access to the bucket and files so that CloudFront URLs can serve content from the bucket\. However, you can restrict access to specific content by using the CloudFront private content feature\. For more information, see [Serving Private Content with Signed URLs and Signed Cookies](PrivateContent.md)\.

   Select the check box for **I acknowledge that the current settings may result in this bucket and the objects within becoming public**\.

1. Choose **Next**, and then choose **Create bucket**\.

1. In the **Buckets** pane, choose your bucket, and then choose **Upload**\.

1. On the **Select files** page, drag and drop your files to the bucket\. Or choose **Add files**, and then choose the files that you want to upload\.

1. Choose **Next**\.

1. On the **Set permissions** page, for **Manage public permissions**, choose **Grant public read access to this object\(s\)**\.

1. Choose **Next**\.

1. Set any properties that you want for the object, such as encryption or tagging, and then choose **Next**\.

1. Choose **Upload**\.

   After the upload is complete, you can navigate to the item by using its URL\. For example:

   `https://<bucket name>.s3-<AWS Region>.amazonaws.com/<object name>`

   Use your Amazon S3 URL to verify that your content is publicly accessible, but remember that this is not the URL you'll use when you're ready to distribute your content with CloudFront\.

## Step 2: Create a CloudFront distribution<a name="GettingStartedCreateDistribution"></a><a name="GettingStartedCreateDistributionProcedure"></a>

**To create a CloudFront distribution**

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the **Select a delivery method for your content** page, in the **Web** section, choose **Get Started**\.

1. On the **Create Distribution** page, for **Origin Domain Name**, choose the Amazon S3 bucket that you created earlier\.

   For the other settings under **Origin Settings**, accept the default values\.

1. For the settings under **Default Cache Behavior Settings**, accept the default values\.

   CloudFront will:
   + Forward all requests that use the CloudFront URL for your distribution \(for example, `https://d111111abcdef8.cloudfront.net/image.jpg`\) to the Amazon S3 bucket that you specified in step 4 of this procedure\.
   + Allow end users to use either HTTP or HTTPS to access your objects\.
   + Respond to requests for your objects\.
   + Attempt to cache your objects at CloudFront edge locations for 24 hours, by default\.
   + Forward only the default request headers to your origin and not cache your objects based on the values in the headers\.
   + Exclude cookies and query string parameters, if any, when forwarding requests for objects to your origin\. \(Amazon S3 doesn't process cookies, and processes only some query string parameters\.\)
   + Allow everyone to view your content\.

   CloudFront will not:
   + Be configured to distribute media files in the Microsoft Smooth Streaming format\.
   + Automatically compress your content\.

   For more information about cache behavior options, see [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)\.

1. Under **Distribution Settings**, choose the values for your distribution:  
**Price Class**  
Choose the price class that corresponds with the maximum price that you want to pay for the CloudFront service\. By default, CloudFront serves your objects from edge locations in all CloudFront Regions\.   
For more information about price classes and how your choice of price class affects CloudFront performance for your distribution, see [Choosing the price class for a CloudFront distribution](PriceClass.md)\. For information about CloudFront pricing, including how price classes map to CloudFront Regions, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\.  
**AWS WAF Web ACL**  
Accept the default value, **None**\.  
**Alternate Domain Names \(CNAMEs\)**  
Accept the default value—that is, leave the field empty\.  
**SSL Certificate**  
Accept the default value, **Default CloudFront Certificate \(\*\.cloudfront\.net\)**\.  
**Supported HTTP Versions**  
Accept the default value, **HTTP/2, HTTP/1\.1, HTTP/1\.0**\.  
**Default Root Object \(Optional\)**  
The object that you want CloudFront to request from your origin \(for example, `index.html`\) when a viewer requests the root URL of your distribution \(`https://d111111abcdef8.cloudfront.net`\) instead of an object in your distribution \(`https://d111111abcdef8.cloudfront.net/product-description.html`\)\. Specifying a default root object avoids exposing the contents of your distribution\.  
**Logging \(Optional\)**  
Accept the default value, **Off**\.  
**Enable IPv6**  
Accept the default value\.  
**Comment \(Optional\)**  
Enter any comments that you want to save with the distribution\.  
**Distribution State**  
Choose **Enabled**\.

1. Choose **Create Distribution**\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution changes from **In Progress** to **Deployed**\. This typically takes a few minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. \(It also appears on the **General** tab for a selected distribution\.\)

## Step 3: Test your links<a name="GettingStartedAccessingDistributions"></a>

After you've created your distribution, CloudFront knows where your Amazon S3 origin server is, and you know the domain name associated with the distribution\. You can create a link to your Amazon S3 bucket content with that domain name and have CloudFront serve it\. 

**Note**  
You must wait until the status of your distribution changes to **Deployed** before testing your links\.<a name="GettingStartedAccessingDistributionsProcedure"></a>

**To link to your objects**

1. Copy the following HTML into a new file:
   + Replace <domain name> with the domain name that CloudFront assigned to your distribution\.
   + Replace <object name> with the name of a file in your Amazon S3 bucket\.

   ```
   1. <html>
   2. <head>My CloudFront Test</head>
   3. <body>
   4. <p>My text content goes here.</p>
   5. <p><img src="https://<domain name>/<object name>" alt="my test image"/></p>
   6. </body>
   7. </html>
   ```

   For example, if your domain name was `d111111abcdef8.cloudfront.net` and your object was `image.jpg`, the URL for the link would be this:

   `https://d111111abcdef8.cloudfront.net/image.jpg`\.

   If your object is in a folder within your bucket, include the folder in the URL\. For example, if `image.jpg` is located in an images folder, the URL would be this:

   `https://d111111abcdef8.cloudfront.net/images/image.jpg`

1. Save the text in a file that has a \.html file name extension\.

1. Open your webpage in a browser to ensure that you can see your content\. If you can't see the content, confirm that you performed all the steps correctly\. You can also see the tips in [Troubleshooting](Troubleshooting.md)\.

The browser returns your page with the embedded image file, served from the edge location that CloudFront determined was appropriate to serve the object\. 

For more information about using CloudFront, see [Amazon CloudFront Related Information](Resources.md)\.