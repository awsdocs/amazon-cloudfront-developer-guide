# Invalidating Files<a name="Invalidation"></a>

If you need to remove a file from CloudFront edge caches before it expires, you can do one of the following:
+ Invalidate the file from edge caches\. The next time a viewer requests the file, CloudFront returns to the origin to fetch the latest version of the file\.
+ Use file versioning to serve a different version of the file that has a different name\. For more information, see [Updating Existing Files Using Versioned File Names](UpdatingExistingObjects.md#ReplacingObjects)\.

**Important**  
You cannot invalidate objects that are served by an RTMP distribution\.

To invalidate files, you can specify either the path for individual files or a path that ends with the `*` wildcard, which might apply to one file or to many, as shown in the following examples:
+ `/images/image1.jpg`
+ `/images/image*`
+ `/images/*`

**Note**  
If you use the AWS command line interface \(CLI\) for invalidating files and you specify a path that includes the `*` wildcard, you must use quotes \(`"`\) around the path\.   
For example: `aws cloudfront create-invalidation --distribution-id distribution_ID --paths "/*"`

You can submit a certain number of invalidation paths each month for free\. If you submit more than the allotted number of invalidation paths in a month, you pay a fee for each invalidation path that you submit\. For more information about the charges for invalidation, see [Paying for File Invalidation](#PayingForInvalidation)\. 

**Topics**
+ [Choosing Between Invalidating Files and Using Versioned File Names](#Invalidation_Expiration)
+ [Determining Which Files to Invalidate](#invalidation-access-logs)
+ [Specifying the Files to Invalidate](#invalidation-specifying-objects)
+ [Invalidating Files Using the Console](#Invalidation_Requests)
+ [Invalidating Files Using the CloudFront API](#invalidating-objects-api)
+ [Concurrent Invalidation Request Maximum](#InvalidationLimits)
+ [Paying for File Invalidation](#PayingForInvalidation)

## Choosing Between Invalidating Files and Using Versioned File Names<a name="Invalidation_Expiration"></a>

To control the versions of files that are served from your distribution, you can either invalidate files or give them versioned file names\. If you want to update your files frequently, we recommend that you primarily use file versioning for the following reasons:
+ Versioning enables you to control which file a request returns even when the user has a version cached either locally or behind a corporate caching proxy\. If you invalidate the file, the user might continue to see the old version until it expires from those caches\.
+ CloudFront access logs include the names of your files, so versioning makes it easier to analyze the results of file changes\.
+ Versioning provides a way to serve different versions of files to different users\.
+ Versioning simplifies rolling forward and back between file revisions\.
+ Versioning is less expensive\. You still have to pay for CloudFront to transfer new versions of your files to edge locations, but you don't have to pay for invalidating files\. 

For more information about file versioning, see [Updating Existing Files Using Versioned File Names](UpdatingExistingObjects.md#ReplacingObjects)\.

## Determining Which Files to Invalidate<a name="invalidation-access-logs"></a>

If you want to invalidate multiple files such as all of the files in a directory or all files that begin with the same characters, you can include the `*` wildcard at the end of the invalidation path\. For more information about using the `*` wildcard, see [Invalidation paths](#invalidation-specifying-objects-paths)\.

If you want to invalidate selected files but your users don’t necessarily access every file on your origin, you can determine which files viewers have requested from CloudFront and invalidate only those files\. To determine which files viewers have requested, enable CloudFront access logging\. For more information about access logs, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

## Specifying the Files to Invalidate<a name="invalidation-specifying-objects"></a>

Note the following about specifying the files that you want to invalidate\.

**Case sensitivity**  
Invalidation paths are case sensitive, so `/images/image.jpg` and `/images/Image.jpg` specify two different files\.

 **Changing the URI Using a Lambda Function**  
If your CloudFront distribution triggers a Lambda function on viewer request events, and if the function changes the URI of the requested file, we recommend that you invalidate both URIs to remove the file from CloudFront edge caches:  
+ The URI in the viewer request
+ The URI after the function changed it
For example, suppose your Lambda function changes the URI for a file from this:  
`https://d111111abcdef8.cloudfront.net/index.html`  
to a URI that includes a language directory:  
`https://d111111abcdef8.cloudfront.net/en/index.html`  
To invalidate the file, you must specify the following paths:  
+ `/index.html`
+ `/en/index.html`
For more information, see [Invalidation paths](#invalidation-specifying-objects-paths)\.

 **Default root object**  
To invalidate the default root object \(file\), specify the path the same way that you specify the path for any other file\.

 **Distribution types**  
You can invalidate only files that are associated with a web distribution\. You cannot invalidate objects that are served by an RTMP distribution\.

 **Forwarding cookies**  
If you configured CloudFront to forward cookies to your origin, CloudFront edge caches might contain several versions of the file\. When you invalidate a file, CloudFront invalidates every cached version of the file regardless of its associated cookies\. You can’t selectively invalidate some versions and not others based on the associated cookies\. For more information, see [Caching Content Based on Cookies](Cookies.md)\.

 **Forwarding headers**  
If you configured CloudFront to forward a whitelist of headers to your origin and to cache based on the values of the headers, CloudFront edge caches might contain several versions of the file\. When you invalidate a file, CloudFront invalidates every cached version of the file regardless of the header values\. You can’t selectively invalidate some versions and not others based on header values\. \(If you configure CloudFront to forward all headers to your origin, CloudFront doesn't cache your files\.\) For more information, see [Caching Content Based on Request Headers](header-caching.md)\.

 **Forwarding query strings**  
If you configured CloudFront to forward query strings to your origin, you must include the query strings when invalidating files, as shown in the following examples:  
+ `/images/image.jpg?parameter1=a`
+ `/images/image.jpg?parameter1=b`
If client requests include five different query strings for the same file, you can either invalidate the file five times, once for each query string, or you can use the \* wildcard in the invalidation path, as shown in the following example:  
`/images/image.jpg*`  
For more information about using wildcards in the invalidation path, see [Invalidation paths](#invalidation-specifying-objects-paths)\. For more information about query strings, see [Caching Content Based on Query String Parameters](QueryStringParameters.md)\. To determine which query strings are in use, you can enable CloudFront logging\. For more information, see [Configuring and Using Standard Logs \(Access Logs\)](AccessLogs.md)\.

**Maximum Allowed**  
For information about the maximum number of invalidations allowed, see [Concurrent Invalidation Request Maximum](#InvalidationLimits)\.

 **Microsoft Smooth Streaming files**  
You cannot invalidate media files in the Microsoft Smooth Streaming format when you have enabled Smooth Streaming for the corresponding cache behavior\. 

 **Non\-ASCII or unsafe characters in the path**  
If the path includes non\-ASCII characters or unsafe characters as defined in [RFC 1783](https://tools.ietf.org/html/rfc1738), URL\-encode those characters\. Do not URL\-encode any other characters in the path, or CloudFront will not invalidate the old version of the updated file\.

 **Invalidation paths**  
The path is relative to the distribution\. For example, to invalidate the file at `https://d111111abcdef8.cloudfront.net/images/image2.jpg`, you would specify the following:  
`/images/image2.jpg`  
In the [CloudFront console](https://console.aws.amazon.com/cloudfront/home), you can omit the leading slash in the path, like this: `images/image2.jpg`\. When you use the CloudFront API directly, invalidation paths must begin with a leading slash\.
You can also invalidate multiple files simultaneously by using the `*` wildcard\. The `*`, which replaces 0 or more characters, must be the last character in the invalidation path\. Also, if you use the AWS command line interface \(CLI\) for invalidating files and you specify a path that includes the `*` wildcard, you must use quotes \(`"`\) around the path \(like this: `"/*"`\)\.  
The following are some examples:  
+ To invalidate all of the files in a directory:

  `/`*directory\-path*`/*`
+ To invalidate a directory, all of its subdirectories, and all of the files in the directory and subdirectories:

  `/`*directory\-path*`*`
+ To invalidate all files that have the same name but different file name extensions, such as logo\.jpg, logo\.png, and logo\.gif:

  `/`*directory\-path*`/`*file\-name*`.*`
+ To invalidate all of the files in a directory for which the file name starts with the same characters \(such as all of the files for a video in HLS format\), regardless of the file name extension:

  `/`*directory\-path*`/`*initial\-characters\-in\-file\-name*`*`
+ When you configure CloudFront to cache based on query string parameters and you want to invalidate every version of a file:

  `/`*directory\-path*`/`*file\-name*`.`*file\-name\-extension*`*`
+ To invalidate all of the files in a distribution:

  `/*`
The maximum length of a path is 4,000 characters\. You can’t use a wildcard within the path; only at the end of the path\.  
For information about invalidating files if you use a Lambda function to change the URI, see [Changing the URI Using a Lambda Function](#invalidation-lambda-at-edge)\.  
The charge to submit an invalidation path is the same regardless of the number of files you’re invalidating: a single file \(`/images/logo.jpg`\) or all of the files that are associated with a distribution \(`/*`\)\. For more information, see [Amazon CloudFront Pricing](https://aws.amazon.com/cloudfront/pricing/)\.  
If the invalidation path is a directory and if you have not standardized on a method for specifying directories—with or without a trailing slash \(/\)—we recommend that you invalidate the directory both with and without a trailing slash, for example, `/images` and `/images/`\.

**Signed URLs**  
If you are using signed URLs, invalidate a file by including only the portion of the URL before the question mark \(?\)\. 

## Invalidating Files Using the Console<a name="Invalidation_Requests"></a>

You can use the CloudFront console to create and run an invalidation, display a list of the invalidations that you submitted previously, and display detailed information about an individual invalidation\. You can also copy an existing invalidation, edit the list of file paths, and run the edited invalidation\. You can't remove invalidations from the list\.
+ [Invalidating Files](#invalidating-objects-console)
+ [Copying, Editing, and Rerunning an Existing Invalidation](#invalidating-objects-copy-console)
+ [Canceling Invalidations](#canceling-invalidations)
+ [Listing Invalidations](#listing-invalidations-console)
+ [Displaying Information about an Invalidation](#invalidation-details-console)

### Invalidating Files<a name="invalidating-objects-console"></a>

To invalidate files using the CloudFront console, do the following\.<a name="invalidating-objects-console-procedure"></a>

**To invalidate files**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the distribution for which you want to invalidate files\.

1. Choose **Distribution Settings**\.

1. Choose the **Invalidations** tab\.

1. Choose **Create Invalidation**\.

1. For the files that you want to invalidate, enter one invalidation path per line\. For information about specifying invalidation paths, see [Specifying the Files to Invalidate](#invalidation-specifying-objects)\. 
**Important**  
Specify file paths carefully\. You can’t cancel an invalidation request after you start it\.

1. Choose **Invalidate**\.

### Copying, Editing, and Rerunning an Existing Invalidation<a name="invalidating-objects-copy-console"></a>

You can copy an invalidation that you created previously, update the list of invalidation paths, and run the updated invalidation\. You cannot copy an existing invalidation, update the invalidation paths, and then save the updated invalidation without running it\.

**Important**  
If you copy an invalidation that is still in progress, update the list of invalidation paths, and then run the updated invalidation, CloudFront will not stop or delete the invalidation that you copied\. If any invalidation paths appear in the original and in the copy, CloudFront will try to invalidate the files twice, and both invalidations will count against your maximum number of free invalidations for the month\. If you’ve already reached the maximum number of free invalidations, you'll be charged for both invalidations of each file\. For more information, see [Concurrent Invalidation Request Maximum](#InvalidationLimits)\.<a name="invalidating-objects-copy-console-procedure"></a>

**To copy, edit, and rerun an existing invalidation**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the distribution that contains the invalidation that you want to copy\.

1. Choose **Distribution Settings**\.

1. Choose the **Invalidations** tab\.

1. Choose the invalidation that you want to copy\.

   If you aren’t sure which invalidation you want to copy, you can choose an invalidation and choose **Details** to display detailed information about that invalidation\.

1. Choose **Copy**\.

1. Update the list of invalidation paths if applicable\.

1. Choose **Invalidate**\.

### Canceling Invalidations<a name="canceling-invalidations"></a>

When you submit an invalidation request to CloudFront, CloudFront forwards the request to all edge locations within a few seconds, and each edge location starts processing the invalidation immediately\. As a result, you can’t cancel an invalidation after you submit it\.

### Listing Invalidations<a name="listing-invalidations-console"></a>

You can display a list of the last 100 invalidations that you’ve created and run for a distribution by using the CloudFront console\. If you want to get a list of more than 100 invalidations, use the `ListInvalidations` API action\. For more information, see [ListInvalidations](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListInvalidations.html) in the *Amazon CloudFront API Reference*\.<a name="listing-invalidations-console-procedure"></a>

**To list invalidations**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the distribution for which you want to display a list of invalidations\.

1. Choose **Distribution Settings**\.

1. Choose the **Invalidations** tab\.

**Note**  
You can’t remove invalidations from the list\.

### Displaying Information about an Invalidation<a name="invalidation-details-console"></a>

You can display detailed information about an invalidation, including distribution ID, invalidation ID, the status of the invalidation, the date and time that the invalidation was created, and a complete list of the invalidation paths\. <a name="invalidation-details-console-procedure"></a>

**To display information about an invalidation**

1. Sign in to the AWS Management Console and open the CloudFront console at [https://console\.aws\.amazon\.com/cloudfront/](https://console.aws.amazon.com/cloudfront/)\.

1. Select the distribution that contains the invalidation that you want to display detailed information for\.

1. Choose **Distribution Settings**\.

1. Choose the **Invalidations** tab\.

1. Choose the applicable invalidation\.

1. Choose **Details**\.

## Invalidating Files Using the CloudFront API<a name="invalidating-objects-api"></a>

For information about invalidating objects and about displaying information about invalidations using the CloudFront API, see the following topics in the *Amazon CloudFront API Reference*:
+ Invalidating files: [CreateInvalidation](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateInvalidation.html) 
+ Getting a list of your invalidations: [ListInvalidations](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_ListInvalidations.html)
+ Getting information about a specific invalidation: [GetInvalidation](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_GetInvalidation.html)

## Concurrent Invalidation Request Maximum<a name="InvalidationLimits"></a>

If you’re invalidating files individually, you can have invalidation requests for up to 3,000 files per distribution in progress at one time\. This can be one invalidation request for up to 3,000 files, up to 3,000 requests for one file each, or any other combination that doesn’t exceed 3,000 files\. For example, you can submit 30 invalidation requests that invalidate 100 files each\. As long as all 30 invalidation requests are still in progress, you can’t submit any more invalidation requests\. If you exceed the maximum, CloudFront returns an error message\.

If you’re using the \* wildcard, you can have requests for up to 15 invalidation paths in progress at one time\. You can also have invalidation requests for up to 3,000 individual files per distribution in progress at the same time; the maximum on wildcard invalidation requests allowed is independent of the maximum on invalidating files individually\.

## Paying for File Invalidation<a name="PayingForInvalidation"></a>

The first 1,000 invalidation paths that you submit per month are free; you pay for each invalidation path over 1,000 in a month\. An invalidation path can be for a single file \(such as `/images/logo.jpg`\) or for multiple files \(such as `/images/*`\)\. A path that includes the `*` wildcard counts as one path even if it causes CloudFront to invalidate thousands of files\.

The maximum of 1,000 free invalidation paths per month applies to the total number of invalidation paths across all of the distributions that you create with one AWS account\. For example, if you use the AWS account `john@example.com` to create three distributions, and you submit 600 invalidation paths for each distribution in a given month \(for a total of 1,800 invalidation paths\), AWS will charge you for 800 invalidation paths in that month\. For specific information about invalidation pricing, see [Amazon CloudFront Pricing](http://aws.amazon.com/cloudfront/pricing/)\. For more information about invalidation paths, see [Invalidation paths](#invalidation-specifying-objects-paths)\.