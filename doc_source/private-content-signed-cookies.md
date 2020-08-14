# Using Signed Cookies<a name="private-content-signed-cookies"></a>

CloudFront signed cookies allow you to control who can access your content when you don't want to change your current URLs or when you want to provide access to multiple restricted files, for example, all of the files in the subscribers' area of a website\. This topic explains the considerations when using signed cookies and describes how to set signed cookies using canned and custom policies\.

**Topics**
+ [Choosing Between Canned and Custom Policies for Signed Cookies](#private-content-choosing-canned-custom-cookies)
+ [How Signed Cookies Work](#private-content-how-signed-cookies-work)
+ [Preventing Misuse of Signed Cookies](#private-content-signed-cookie-misuse)
+ [When Does CloudFront Check the Expiration Date and Time in a Signed Cookie?](#private-content-check-expiration-cookie)
+ [Sample Code and Third\-Party Tools](#private-content-overview-sample-code-cookies)
+ [Setting Signed Cookies Using a Canned Policy](private-content-setting-signed-cookie-canned-policy.md)
+ [Setting Signed Cookies Using a Custom Policy](private-content-setting-signed-cookie-custom-policy.md)

## Choosing Between Canned and Custom Policies for Signed Cookies<a name="private-content-choosing-canned-custom-cookies"></a>

When you create a signed cookie, you write a policy statement in JSON format that specifies the restrictions on the signed cookie, for example, how long the cookie is valid\. You can use canned policies or custom policies\. The following table compares canned and custom policies:


****  

| Description | Canned Policy | Custom Policy | 
| --- | --- | --- | 
| You can reuse the policy statement for multiple files\. To reuse the policy statement, you must use wildcard characters in the `Resource` object\. For more information, see [Values That You Specify in the Policy Statement for a Custom Policy for Signed Cookies](private-content-setting-signed-cookie-custom-policy.md#private-content-custom-policy-statement-cookies-values)\.\)  | No | Yes | 
| You can specify the date and time that users can begin to access your content | No | Yes \(optional\) | 
| You can specify the date and time that users can no longer access your content | Yes | Yes | 
| You can specify the IP address or range of IP addresses of the users who can access your content | No | Yes \(optional\) | 

For information about creating signed cookies using a canned policy, see [Setting Signed Cookies Using a Canned Policy](private-content-setting-signed-cookie-canned-policy.md)\.

For information about creating signed cookies using a custom policy, see [Setting Signed Cookies Using a Custom Policy](private-content-setting-signed-cookie-custom-policy.md)\.

## How Signed Cookies Work<a name="private-content-how-signed-cookies-work"></a>

Here's an overview of how you configure CloudFront for signed cookies and how CloudFront responds when a user submits a request that contains a signed cookie\. 

1. In your CloudFront distribution, you specify one or more trusted signers, which are the AWS accounts that you want to have permission to create signed URLs and signed cookies\.

   For more information, see [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)\.

1. You develop your application to determine whether a user should have access to your content and, if so, to send three `Set-Cookie` headers to the viewer\. \(Each `Set-Cookie` header can contain only one name\-value pair, and a CloudFront signed cookie requires three name\-value pairs\.\) You must send the `Set-Cookie` headers to the viewer before the viewer requests your private content\. If you set a short expiration time on the cookie, you might also want to send three more `Set-Cookie` headers in response to subsequent requests, so that the user continues to have access\.

   Typically, your CloudFront distribution will have at least two cache behaviors, one that doesn't require authentication and one that does\. The error page for the secure portion of the site includes a redirector or a link to a login page\.

   If you configure your distribution to cache files based on cookies, CloudFront doesn't cache separate files based on the attributes in signed cookies\.

1. A user signs in to your website and either pays for content or meets some other requirement for access\.

1. Your application returns the `Set-Cookie` headers in the response, and the viewer stores the name\-value pairs\.

1. The user requests a file\.

   The user's browser or other viewer gets the name\-value pairs from step 4 and adds them to the request in a `Cookie` header\. This is the signed cookie\. 

1. CloudFront uses the public key to validate the signature in the signed cookie and to confirm that the cookie hasn't been tampered with\. If the signature is invalid, the request is rejected\.

   If the signature in the cookie is valid, CloudFront looks at the policy statement in the cookie \(or constructs one if you're using a canned policy\) to confirm that the request is still valid\. For example, if you specified a beginning and ending date and time for the cookie, CloudFront confirms that the user is trying to access your content during the time period that you want to allow access\.

   If the request meets the requirements in the policy statement, CloudFront serves your content as it does for content that isn't restricted: it determines whether the file is already in the edge cache, forwards the request to the origin if necessary, and returns the file to the user\.

## Preventing Misuse of Signed Cookies<a name="private-content-signed-cookie-misuse"></a>

If you specify the `Domain` parameter in a `Set-Cookie` header, specify the most precise value possible to reduce the potential for access by someone with the same root domain name\. For example, app\.example\.com is preferable to example\.com, especially when you don't control example\.com\. This helps prevent someone from accessing your content from www\.example\.com\.

To help prevent this type of attack, do the following:
+ Exclude the `Expires` and `Max-Age` cookie attributes, so that the `Set-Cookie` header creates a session cookie\. Session cookies are automatically deleted when the user closes the browser, which reduces the possibility of someone getting unauthorized access to your content\.
+ Include the `Secure` attribute, so that the cookie is encrypted when a viewer includes it in a request\.
+ When possible, use a custom policy and include the IP address of the viewer\.
+ In the `CloudFront-Expires` attribute, specify the shortest reasonable expiration time based on how long you want users to have access to your content\.

## When Does CloudFront Check the Expiration Date and Time in a Signed Cookie?<a name="private-content-check-expiration-cookie"></a>

To determine whether a signed cookie is still valid, CloudFront checks the expiration date and time in the cookie at the time of the HTTP request\. If a client begins to download a large file immediately before the expiration time, the download should complete even if the expiration time passes during the download\. If the TCP connection drops and the client tries to restart the download after the expiration time passes, the download will fail\.

If a client uses Range GETs to get a file in smaller pieces, any GET request that occurs after the expiration time passes will fail\. For more information about Range GETs, see [How CloudFront Processes Partial Requests for an Object \(Range GETs\)](RangeGETs.md)\.

## Sample Code and Third\-Party Tools<a name="private-content-overview-sample-code-cookies"></a>

The sample code for private content shows only how to create the signature for signed URLs\. However, the process for creating a signature for a signed cookie is very similar, so much of the sample code is still relevant\. For more information, see the following topics: 
+ [Create a URL Signature Using Perl](CreateURLPerl.md)
+ [Create a URL Signature Using PHP](CreateURL_PHP.md)
+ [Create a URL Signature Using C\# and the \.NET Framework](CreateSignatureInCSharp.md)
+ [Create a URL Signature Using Java](CFPrivateDistJavaDevelopment.md)