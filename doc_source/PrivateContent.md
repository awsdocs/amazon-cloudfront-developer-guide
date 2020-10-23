# Serving private content with signed URLs and signed cookies<a name="PrivateContent"></a>

Many companies that distribute content over the internet want to restrict access to documents, business data, media streams, or content that is intended for selected users, for example, users who have paid a fee\. To securely serve this private content by using CloudFront, you can do the following:
+ Require that your users access your private content by using special CloudFront signed URLs or signed cookies\. 
+ Require that your users access your content by using CloudFront URLs, not URLs that access content directly on the origin server \(for example, Amazon S3 or a private HTTP server\)\. Requiring CloudFront URLs isn't necessary, but we recommend it to prevent users from bypassing the restrictions that you specify in signed URLs or signed cookies\.

**Topics**
+ [Overview of serving private content](private-content-overview.md)
+ [Task list for serving private content](private-content-task-list.md)
+ [Specifying the signers that can create signed URLs and signed cookies](private-content-trusted-signers.md)
+ [Choosing between signed URLs and signed cookies](private-content-choosing-signed-urls-cookies.md)
+ [Using signed URLs](private-content-signed-urls.md)
+ [Using signed cookies](private-content-signed-cookies.md)
+ [Using Linux commands and OpenSSL for base64 encoding and encryption](private-content-linux-openssl.md)
+ [Code examples for creating a signature for a signed URL](PrivateCFSignatureCodeAndExamples.md)