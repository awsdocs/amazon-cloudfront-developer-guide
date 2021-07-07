# Getting started with a simple CloudFront distribution<a name="GettingStarted.SimpleDistribution"></a>

The procedures in this section show you how to use CloudFront to set up a basic configuration that does the following:
+ Stores the original versions of your objects in an Amazon Simple Storage Service \(Amazon S3\) bucket
+ Makes your objects accessible to everyone
+ Uses the CloudFront domain name in URLs for your objects \(for example, `http://d111111abcdef8.cloudfront.net/index.html`\)
+ Keeps your objects in CloudFront edge locations for the default duration of 24 hours \(the minimum duration is 0 seconds\)

Most of these options are customizable\. For example, you can store your content on your own web server instead of using an S3 bucket, and you can restrict who has access to the content by using signed URLs or cookies\. For information about how to customize your CloudFront distribution options, see [Steps for Creating a Distribution \(Overview\)](distribution-web-creating.md)\.

You have to complete only a few basic steps to start delivering your content with CloudFront\. The first step is signing up\. After that, you create a CloudFront distribution, and then use the CloudFront domain name in URLs in your webpages or applications to reference the content\.

**Topics**
+ [Prerequisites](#GettingStartedSignup)
+ [Step 1: Upload your content to Amazon S3 and grant object permissions](#GettingStartedUploadContent)
+ [Step 2: Create a CloudFront distribution](#GettingStartedCreateDistribution)
+ [Step 3: Access your content through CloudFront](#GettingStartedAccessingDistributions)

## Prerequisites<a name="GettingStartedSignup"></a>

Before you begin, make sure that you’ve completed the steps in [Setting up Amazon CloudFront](setting-up-cloudfront.md)\.

## Step 1: Upload your content to Amazon S3 and grant object permissions<a name="GettingStartedUploadContent"></a>

An Amazon S3 bucket is a container for files \(objects\) or folders\. CloudFront can distribute almost any type of file for you using an Amazon S3 bucket as the source\. For example, CloudFront can distribute text, images, and videos\. There is no maximum for the amount of data that you can store on Amazon S3\.

By default, your Amazon S3 bucket and all the files in it are private—only the AWS account that created the bucket has permission to read or write the files\. If you want to allow anyone to access the files in your Amazon S3 bucket using CloudFront URLs, you must grant public read permissions to the objects\.

**Note**  
If you want to restrict who can download your content, you can use the CloudFront private content feature\. For more information about distributing private content, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.

**To upload your content to Amazon S3 and grant read permissions to everyone**

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\.

1. For **Bucket name**, enter a bucket name\.
**Important**  
For your bucket to work with CloudFront, the name must conform to DNS naming requirements\. For more information, see [Bucket restrictions and limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. For **Region**, choose an AWS Region for your bucket\. We recommend that you choose a Region close to you to optimize latency and minimize costs, or you might choose another Region to address regulatory requirements\.

1. In the **Block Public Access settings for bucket** section, clear the check box for **Block *all* public access**\.

   You must allow public read access to the bucket and files so that CloudFront URLs can serve content from the bucket\. However, you can restrict access to specific content by using the CloudFront private content feature\. For more information, see [Serving private content with signed URLs and signed cookies](PrivateContent.md)\.

   Select the check box for **I acknowledge that the current settings might result in this bucket and the objects within becoming public\.**\.

1. Leave all other settings at their defaults, and then choose **Create bucket**\.

1. \(Optional\) If you don’t have your own website content, or if you just want to experiment with CloudFront before uploading your own content, use the following link to download a simple *hello world* webpage: [hello\-world\-html\.zip](samples/hello-world-html.zip)\.

1. In the **Buckets** section, choose your new bucket, and then choose **Upload**\.

1. Use the **Upload** page to add your content to the S3 bucket\. If you downloaded the simple *hello world* webpage, add the `index.html` file and the `css` folder \(with the `style.css` file inside it\)\.

1. Choose **Additional upload options** to expand the section\.

1. In the **Access control list \(ACL\)** section, select the check box for **Read** next to **Everyone \(public access\)** in the **Objects** column\.

1. Select the check box for **I understand the effects of these changes on the specified objects\.**

1. At the bottom of the page, choose **Upload**\.

   After the upload is complete, you can navigate to the item by using its URL\. For example:

   ```
   https://<bucket name>.s3-<AWS Region>.amazonaws.com/<object name>
   ```

   Replace `<bucket name>`, `<AWS Region>`, and `<object name>` with the appropriate values based on your bucket and content\. If you used the simple *hello world* website in this procedure, replace `<object name>` with `index.html`\.
**Note**  
If you created the bucket in the US East \(N\. Virginia\) Region \(us\-east\-1\), omit the *<AWS Region>* portion of the URL\. For example:  

   ```
   https://<bucket name>.s3.amazonaws.com/<object name>
   ```

   Use the Amazon S3 URL to verify that your content is publicly accessible, but remember that this is not the URL you’ll use to access your content with CloudFront\.

## Step 2: Create a CloudFront distribution<a name="GettingStartedCreateDistribution"></a><a name="GettingStartedCreateDistributionProcedure"></a>

**To create a CloudFront distribution**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose **Create Distribution**, and then choose **Get Started**\.

1. Under **Origin Settings**, for **Origin Domain Name**, choose the Amazon S3 bucket that you created earlier\.

   For the other settings under **Origin Settings**, accept the default values\.

1. For the settings under **Default Cache Behavior Settings**, accept the default values\.

   For more information about cache behavior options, see [Cache Behavior Settings](distribution-web-values-specify.md#DownloadDistValuesCacheBehavior)\.

1. For the settings under **Distribution Settings**, accept the default values\.

   For more information about distribution options, see [Distribution Settings](distribution-web-values-specify.md#DownloadDistValuesGeneral)\.

1. At the bottom of the page, choose **Create Distribution**\.

1. After CloudFront creates your distribution, the value of the **Status** column for your distribution changes from **In Progress** to **Deployed**\. This typically takes a few minutes\.

   Record the domain name that CloudFront assigns to your distribution, which appears in the list of distributions\. \(It also appears on the **General** tab for a selected distribution\.\) It looks similar to the following: `d111111abcdef8.cloudfront.net`\.

## Step 3: Access your content through CloudFront<a name="GettingStartedAccessingDistributions"></a>

To access your content through CloudFront, combine your CloudFront distribution domain name with the path to access your content\. For example, your distribution domain name looks similar to the following: `d111111abcdef8.cloudfront.net`\. Traditionally, the path to access the main page of a website is `/index.html`\. In this case, you could access your content through CloudFront at a URL that looks similar to the following:

`https://d111111abcdef8.cloudfront.net/index.html`

If you followed the previous steps and used the simple *hello world* webpage, you should see the webpage’s content:

![\[The Hello world webpage.\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/hello-world-webpage.png)

When you upload new content to your S3 bucket, you can access the content through CloudFront by combining the CloudFront distribution domain name \(for example, `d111111abcdef8.cloudfront.net`\) with the path to the object in the S3 bucket\. For example, if you upload a new file named `new-page.html` to the root of your S3 bucket, you can access this page through CloudFront at a URL similar to the following:

`https://d111111abcdef8.cloudfront.net/new-page.html`

You have successfully configured CloudFront to serve your website’s content that’s stored in Amazon S3\. If you want to go a step further, you can configure your CloudFront distribution to use a custom domain name \(for example, `www.example.com` instead of `d111111abcdef8.cloudfront.net`\)\. For more information, see [Using custom URLs](CNAMEs.md)\.