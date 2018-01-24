# Getting Started with CloudFront<a name="GettingStarted"></a>

The example in this topic gives you a quick overview of how to use CloudFront to:

+ Store the original versions of your objects in one Amazon Simple Storage Service \(Amazon S3\) bucket\.

+ Distribute download content such as text or graphics\. 

+ Make your objects accessible to everyone\.

+ Use the CloudFront domain name in URLs for your objects \(for example, `http://d111111abcdef8.cloudfront.net/image.jpg`\) instead of your own domain name \(for example, `http://www.example.com/image.jpg`\)\.

+ Keep your objects in CloudFront edge locations for the default duration of 24 hours\. \(The minimum duration is 0 seconds\.\)

For information about how to use CloudFront when you want to use other options, see [Task List for Creating a Web Distribution](distribution-web-creating.md) or [Task List for Streaming Media Files Using RTMP](distribution-rtmp-creating.md)\.

You only need to perform a few basic steps to start delivering your content using CloudFront\. The first step is signing up\. After that, you create a CloudFront distribution, and then use the CloudFront domain name to reference content in your web pages or applications\.


+ [Step 1: Sign up for Amazon Web Services](#GettingStartedSignup)
+ [Step 2: Upload your content to Amazon S3 and grant object permissions](#GettingStartedUploadContent)
+ [Step 3: Create a CloudFront Web Distribution](#GettingStartedCreateDistribution)
+ [Step 4: Test your links](#GettingStartedAccessingDistributions)

## Step 1: Sign up for Amazon Web Services<a name="GettingStartedSignup"></a>

If you haven't already done so, sign up for Amazon Web Services at [http://aws\.amazon\.com](http://aws.amazon.com)\. Just choose **Sign Up Now** and enter any required information\. 

## Step 2: Upload your content to Amazon S3 and grant object permissions<a name="GettingStartedUploadContent"></a>

An Amazon S3 bucket is a container that can contain objects or folders\. CloudFront can distribute almost any type of object for you using an Amazon S3 bucket as the source, for example, text, images, and videos\. You can create multiple buckets, and there is no limit to the amount of data that you can store on Amazon S3\.

By default, your Amazon S3 bucket and all of the objects in it are private—only the AWS account that created the bucket has permission to read or write the objects in it\. If you want to allow anyone to access the objects in your Amazon S3 bucket using CloudFront URLs, you must grant public read permissions to the objects\. \(This is one of the most common mistakes when working with CloudFront and Amazon S3\. You must explicitly grant privileges to each object in an Amazon S3 bucket\.\)

**Note**  
If you want to restrict who can download your content, you can use the CloudFront private content feature\. For more information about distributing private content, see [Serving Private Content through CloudFront](PrivateContent.md)\.

**To upload your content to Amazon S3 and grant read permission to everyone**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. In the Amazon S3 console, choose **Create Bucket**\.

1. In the **Create Bucket** dialog, enter a bucket name\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, go to [Bucket Restrictions and Limitations](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. Select a region for your bucket\. By default, Amazon S3 creates buckets in the US East \(N\. Virginia\) region\. We recommend that you choose a region close to you to optimize latency, minimize costs, or to address regulatory requirements\.

1. Choose **Create**\.

1. Select your bucket in the **Buckets** pane, and choose **Upload**\.

1. On the **Upload \- Select Files** page, choose **Add Files**, and choose the files that you want to upload\.  
![\[Upload files to your Amazon S3 bucket.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/s3-add-files.png)

1. Enable public read privileges for each object that you upload to your Amazon S3 bucket\. 

   1. Choose **Set Details**\.

   1. On the **Set Details** page, choose **Set Permissions**\.

   1. On the **Set Permissions** page, choose **Make everything public**\.

1. Choose **Start Upload**\.

   After the upload completes, you can navigate to this item by its URL\. In the case of the previous example, the URL would be:

   `http://s3.amazonaws.com/example-myawsbucket/filename`

   Use your Amazon S3 URL to verify that your content is publicly accessible, but remember that this is not the URL you will use when you are ready to distribute your content\.

## Step 3: Create a CloudFront Web Distribution<a name="GettingStartedCreateDistribution"></a>

**To create a CloudFront web distribution**

1. Open the CloudFront console at [ https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Choose **Create Distribution**\.

1. On the **Select a delivery method for your content** page, in the **Web** section, choose **Get Started**\.  
![\[Select Web as the distribution type.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/create-download-dist-1.png)

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

1. Under **Distribution Settings**, enter the applicable values:  
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
If you add a CNAME for `www.example.com` to your distribution, you also need to create \(or update\) a CNAME record with your DNS service to route queries for `www.example.com` to `d111111abcdef8.cloudfront.net`\. You must have permission to create a CNAME record with the DNS service provider for the domain\. Typically, this means that you own the domain, but you may also be developing an application for the domain owner\. For more information about CNAMEs, see [Using Alternate Domain Names \(CNAMEs\)](CNAMEs.md)\.
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

1. After CloudFront has created your distribution, the value of the **Status** column for your distribution will change from **InProgress** to **Deployed**\. If you chose to enable the distribution, it will then be ready to process requests\. This should take less than 15 minutes\.

   The domain name that CloudFront assigns to your distribution appears in the list of distributions\. \(It also appears on the **General** tab for a selected distribution\.\)

## Step 4: Test your links<a name="GettingStartedAccessingDistributions"></a>

After you've created your distribution, CloudFront knows where your Amazon S3 origin server is, and you know the domain name associated with the distribution\. You can create a link to your Amazon S3 bucket content with that domain name, and have CloudFront serve it\. 

**Note**  
You must wait until the status of your distribution changes to **Deployed** before testing your links\.

****

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

For more information on using CloudFront, go to [Amazon CloudFront Resources](Resources.md)\.