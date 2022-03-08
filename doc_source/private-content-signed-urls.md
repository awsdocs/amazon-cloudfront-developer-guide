# Using signed URLs<a name="private-content-signed-urls"></a>

**Topics**
+ [Choosing between canned and custom policies for signed URLs](#private-content-choosing-canned-custom-policy)
+ [How signed URLs work](#private-content-how-signed-urls-work)
+ [Choosing how long signed URLs are valid](#private-content-overview-choosing-duration)
+ [When does CloudFront check the expiration date and time in a signed URL?](#private-content-check-expiration)
+ [Example code and third\-party tools](#private-content-overview-sample-code)
+ [Creating a signed URL using a canned policy](private-content-creating-signed-url-canned-policy.md)
+ [Creating a signed URL using a custom policy](private-content-creating-signed-url-custom-policy.md)

A signed URL includes additional information, for example, an expiration date and time, that gives you more control over access to your content\. This additional information appears in a policy statement, which is based on either a canned policy or a custom policy\. The differences between canned and custom policies are explained in the next two sections\.

**Note**  
You can create some signed URLs using canned policies and create some signed URLs using custom policies for the same distribution\.

## Choosing between canned and custom policies for signed URLs<a name="private-content-choosing-canned-custom-policy"></a>

When you create a signed URL, you write a policy statement in JSON format that specifies the restrictions on the signed URL, for example, how long the URL is valid\. You can use either a canned policy or a custom policy\. Here's how canned and custom policies compare:


****  

| Description | Canned policy | Custom policy | 
| --- | --- | --- | 
| You can reuse the policy statement for multiple files\. To reuse the policy statement, you must use wildcard characters in the `Resource` object\. For more information, see [Values that you specify in the policy statement for a signed URL that uses a custom policy](private-content-creating-signed-url-custom-policy.md#private-content-custom-policy-statement-values)\.\)  | No | Yes | 
| You can specify the date and time that users can begin to access your content\. | No | Yes \(optional\) | 
| You can specify the date and time that users can no longer access your content\. | Yes | Yes | 
| You can specify the IP address or range of IP addresses of the users who can access your content\. | No | Yes \(optional\) | 
| The signed URL includes a base64\-encoded version of the policy, which results in a longer URL\. | No | Yes | 

For information about creating signed URLs using a *canned* policy, see [Creating a signed URL using a canned policy](private-content-creating-signed-url-canned-policy.md)\.

For information about creating signed URLs using a *custom* policy, see [Creating a signed URL using a custom policy](private-content-creating-signed-url-custom-policy.md)\.

## How signed URLs work<a name="private-content-how-signed-urls-work"></a>

Here's an overview of how you configure CloudFront and Amazon S3 for signed URLs and how CloudFront responds when a user uses a signed URL to request a file\. 

1. In your CloudFront distribution, specify one or more trusted key groups, which contain the public keys that CloudFront can use to verify the URL signature\. You use the corresponding private keys to sign the URLs\.

   For more information, see [Specifying the signers that can create signed URLs and signed cookies](private-content-trusted-signers.md)\.

1. Develop your application to determine whether a user should have access to your content and to create signed URLs for the files or parts of your application that you want to restrict access to\. For more information, see the following topics:
   + [Creating a signed URL using a canned policy](private-content-creating-signed-url-canned-policy.md)
   + [Creating a signed URL using a custom policy](private-content-creating-signed-url-custom-policy.md)

1. A user requests a file for which you want to require signed URLs\.

1. Your application verifies that the user is entitled to access the file: they've signed in, they've paid for access to the content, or they've met some other requirement for access\.

1. Your application creates and returns a signed URL to the user\.

1. The signed URL allows the user to download or stream the content\.

   This step is automatic; the user usually doesn't have to do anything additional to access the content\. For example, if a user is accessing your content in a web browser, your application returns the signed URL to the browser\. The browser immediately uses the signed URL to access the file in the CloudFront edge cache without any intervention from the user\.

1. CloudFront uses the public key to validate the signature and confirm that the URL hasn't been tampered with\. If the signature is invalid, the request is rejected\. 

   If the signature is valid, CloudFront looks at the policy statement in the URL \(or constructs one if you're using a canned policy\) to confirm that the request is still valid\. For example, if you specified a beginning and ending date and time for the URL, CloudFront confirms that the user is trying to access your content during the time period that you want to allow access\. 

   If the request meets the requirements in the policy statement, CloudFront does the standard operations: determines whether the file is already in the edge cache, forwards the request to the origin if necessary, and returns the file to the user\.

**Note**  
If an unsigned URL contains query string parameters, make sure you include them in the portion of the URL that you sign\. If you add a query string to a signed URL after signing it, the URL returns an HTTP 403 status\.

## Choosing how long signed URLs are valid<a name="private-content-overview-choosing-duration"></a>

You can distribute private content using a signed URL that is valid for only a short time—possibly for as little as a few minutes\. Signed URLs that are valid for such a short period are good for distributing content on\-the\-fly to a user for a specific purpose, such as distributing movie rentals or music downloads to customers on demand\. If your signed URLs will be valid for just a short period, you'll probably want to generate them automatically using an application that you develop\. When the user starts to download a file or starts to play a media file, CloudFront compares the expiration time in the URL with the current time to determine whether the URL is still valid\.

You can also distribute private content using a signed URL that is valid for a longer time, possibly for years\. Signed URLs that are valid for a longer period are useful for distributing private content to known users, such as distributing a business plan to investors or distributing training materials to employees\. You can develop an application to generate these longer\-term signed URLs for you\.

## When does CloudFront check the expiration date and time in a signed URL?<a name="private-content-check-expiration"></a>

CloudFront checks the expiration date and time in a signed URL at the time of the HTTP request\. If a client begins to download a large file immediately before the expiration time, the download should complete even if the expiration time passes during the download\. If the TCP connection drops and the client tries to restart the download after the expiration time passes, the download will fail\.

If a client uses Range GETs to get a file in smaller pieces, any GET request that occurs after the expiration time passes will fail\. For more information about Range GETs, see [How CloudFront processes partial requests for an object \(range GETs\)](RangeGETs.md)\.

## Example code and third\-party tools<a name="private-content-overview-sample-code"></a>

For example code that creates the hashed and signed part of signed URLs, see the following topics:
+ [Create a URL signature using Perl](CreateURLPerl.md)
+ [Create a URL signature using PHP](CreateURL_PHP.md)
+ [Create a URL signature using C\# and the \.NET Framework](CreateSignatureInCSharp.md)
+ [Create a URL signature using Java](CFPrivateDistJavaDevelopment.md)