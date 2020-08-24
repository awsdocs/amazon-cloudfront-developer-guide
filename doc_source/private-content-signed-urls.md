# Using Signed URLs<a name="private-content-signed-urls"></a>

**Topics**
+ [Choosing Between Canned and Custom Policies for Signed URLs](#private-content-choosing-canned-custom-policy)
+ [How Signed URLs Work](#private-content-how-signed-urls-work)
+ [Choosing How Long Signed URLs Are Valid](#private-content-overview-choosing-duration)
+ [When Does CloudFront Check the Expiration Date and Time in a Signed URL?](#private-content-check-expiration)
+ [Sample Code and Third\-Party Tools](#private-content-overview-sample-code)
+ [Creating a Signed URL Using a Canned Policy](private-content-creating-signed-url-canned-policy.md)
+ [Creating a Signed URL Using a Custom Policy](private-content-creating-signed-url-custom-policy.md)

A signed URL includes additional information, for example, an expiration date and time, that gives you more control over access to your content\. This additional information appears in a policy statement, which is based on either a canned policy or a custom policy\. The differences between canned and custom policies are explained in the next two sections\.

**Note**  
You can create some signed URLs using canned policies and create some signed URLs using custom policies for the same distribution\.

## Choosing Between Canned and Custom Policies for Signed URLs<a name="private-content-choosing-canned-custom-policy"></a>

When you create a signed URL, you write a policy statement in JSON format that specifies the restrictions on the signed URL, for example, how long the URL is valid\. You can use either a canned policy or a custom policy\. Here's how canned and custom policies compare:


****  

| Description | Canned Policy | Custom Policy | 
| --- | --- | --- | 
| You can reuse the policy statement for multiple files\. To reuse the policy statement, you must use wildcard characters in the `Resource` object\. For more information, see [Values that You Specify in the Policy Statement for a Signed URL That Uses a Custom Policy](private-content-creating-signed-url-custom-policy.md#private-content-custom-policy-statement-values)\.\)  | No | Yes | 
| You can specify the date and time that users can begin to access your content\. | No | Yes \(optional\) | 
| You can specify the date and time that users can no longer access your content\. | Yes | Yes | 
| You can specify the IP address or range of IP addresses of the users who can access your content\. | No | Yes \(optional\) | 
| The signed URL includes a base64\-encoded version of the policy, which results in a longer URL\. | No | Yes | 

For information about creating signed URLs using a *canned* policy, see [Creating a Signed URL Using a Canned Policy](private-content-creating-signed-url-canned-policy.md)\.

For information about creating signed URLs using a *custom* policy, see [Creating a Signed URL Using a Custom Policy](private-content-creating-signed-url-custom-policy.md)\.

## How Signed URLs Work<a name="private-content-how-signed-urls-work"></a>

Here's an overview of how you configure CloudFront and Amazon S3 for signed URLs and how CloudFront responds when a user uses a signed URL to request a file\. 

1. In your CloudFront distribution, specify one or more trusted signers, which are the AWS accounts that you want to have permission to create signed URLs\.

   For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.

1. You develop your application to determine whether a user should have access to your content and to create signed URLs for the files or parts of your application that you want to restrict access to\. For more information, see the following topics:
   + [Creating a Signed URL Using a Canned Policy](private-content-creating-signed-url-canned-policy.md)
   + [Creating a Signed URL Using a Custom Policy](private-content-creating-signed-url-custom-policy.md)

1. A user requests a file for which you want to require signed URLs\.

1. Your application verifies that the user is entitled to access the file: they've signed in, they've paid for access to the content, or they've met some other requirement for access\.

1. Your application creates and returns a signed URL to the user\.

1. The signed URL allows the user to download or stream the content\.

   This step is automatic; the user usually doesn't have to do anything additional to access the content\. For example, if a user is accessing your content in a web browser, your application returns the signed URL to the browser\. The browser immediately uses the signed URL to access the file in the CloudFront edge cache without any intervention from the user\.

1. CloudFront uses the public key to validate the signature and confirm that the URL hasn't been tampered with\. If the signature is invalid, the request is rejected\. 

   If the signature is valid, CloudFront looks at the policy statement in the URL \(or constructs one if you're using a canned policy\) to confirm that the request is still valid\. For example, if you specified a beginning and ending date and time for the URL, CloudFront confirms that the user is trying to access your content during the time period that you want to allow access\. 

   If the request meets the requirements in the policy statement, CloudFront does the standard operations: determines whether the file is already in the edge cache, forwards the request to the origin if necessary, and returns the file to the user\.

**Note**  
Signed CloudFront URLs cannot contain extra query string arguments\. If you add a query string to a signed URL after you create it, the URL returns an HTTP 403 status\. 

## Choosing How Long Signed URLs Are Valid<a name="private-content-overview-choosing-duration"></a>

You can distribute private content using a signed URL that is valid for only a short time—possibly for as little as a few minutes\. Signed URLs that are valid for such a short period are good for distributing content on\-the\-fly to a user for a specific purpose, such as distributing movie rentals or music downloads to customers on demand\. If your signed URLs will be valid for just a short period, you'll probably want to generate them automatically using an application that you develop\. When the user starts to download a file or starts to play a media file, CloudFront compares the expiration time in the URL with the current time to determine whether the URL is still valid\.

You can also distribute private content using a signed URL that is valid for a longer time, possibly for years\. Signed URLs that are valid for a longer period are useful for distributing private content to known users, such as distributing a business plan to investors or distributing training materials to employees\. You can develop an application to generate these longer\-term signed URLs for you\.

## When Does CloudFront Check the Expiration Date and Time in a Signed URL?<a name="private-content-check-expiration"></a>

When CloudFront checks the expiration date and time in a signed URL to determine whether the URL is still valid depends on whether the URL is for a web distribution or an RTMP distribution:
+ **Web distributions** – CloudFront checks the expiration date and time in a signed URL at the time of the HTTP request\. If a client begins to download a large file immediately before the expiration time, the download should complete even if the expiration time passes during the download\. If the TCP connection drops and the client tries to restart the download after the expiration time passes, the download will fail\.

  If a client uses Range GETs to get a file in smaller pieces, any GET request that occurs after the expiration time passes will fail\. For more information about Range GETs, see [How CloudFront Processes Partial Requests for an Object \(Range GETs\)](RangeGETs.md)\.
+ **RTMP distributions** – CloudFront checks the expiration time in a signed URL at the start of a play event\. If a client starts to play a media file before the expiration time passes, CloudFront allows the entire media file to play\. However, depending on the media player, pausing and restarting might trigger another play event\. Skipping to another position in the media file will trigger another play event\. If the subsequent play event occurs after the expiration time passes, CloudFront won't serve the media file\.
**Note**  
[Adobe designated Flash as end\-of\-life at the end of 2020](https://blog.adobe.com/en/publish/2017/07/25/adobe-flash-update.html)\. As a result, Amazon CloudFront will no longer support Adobe Flash Media Server and will be deprecating Real\-Time Messaging Protocol \(RTMP\) distributions by December 31, 2020\. For more information, [read the full announcement on the Amazon CloudFront discussion forum](https://forums.aws.amazon.com/ann.jspa?annID=7356)\.

## Sample Code and Third\-Party Tools<a name="private-content-overview-sample-code"></a>

For sample code that creates the hashed and signed part of signed URLs, see the following topics:
+ [Create a URL Signature Using Perl](CreateURLPerl.md)
+ [Create a URL Signature Using PHP](CreateURL_PHP.md)
+ [Create a URL Signature Using C\# and the \.NET Framework](CreateSignatureInCSharp.md)
+ [Create a URL Signature Using Java](CFPrivateDistJavaDevelopment.md)