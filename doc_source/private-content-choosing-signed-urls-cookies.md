# Choosing Between Signed URLs and Signed Cookies<a name="private-content-choosing-signed-urls-cookies"></a>

CloudFront signed URLs and signed cookies provide the same basic functionality: they allow you to control who can access your content\. If you want to serve private content through CloudFront and you're trying to decide whether to use signed URLs or signed cookies, consider the following\.

Use signed URLs in the following cases:
+ You want to use an RTMP distribution\. Signed cookies aren't supported for RTMP distributions\.
+ You want to restrict access to individual files, for example, an installation download for your application\.
+ Your users are using a client \(for example, a custom HTTP client\) that doesn't support cookies\.

Use signed cookies in the following cases:
+ You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of website\.
+ You don't want to change your current URLs\.

If you are not currently using signed URLs, and if your \(unsigned\) URLs contain any of the following query string parameters, you cannot use either signed URLs or signed cookies:
+ `Expires`
+ `Policy`
+ `Signature`
+ `Key-Pair-Id`

CloudFront assumes that URLs that contain any of those query string parameters are signed URLs, and therefore won't look at signed cookies\.

## Using Both Signed URLs and Signed Cookies<a name="private-content-using-signed-urls-and-cookies"></a>

If you use both signed URLs and signed cookies to control access to the same files and a viewer uses a signed URL to request a file, CloudFront determines whether to return the file to the viewer based only on the signed URL\.