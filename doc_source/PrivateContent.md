# Serving Private Content with Signed URLs and Signed Cookies<a name="PrivateContent"></a>

Many companies that distribute content over the internet want to restrict access to documents, business data, media streams, or content that is intended for selected users, for example, users who have paid a fee\. To securely serve this private content by using CloudFront, you can do the following:
+ Require that your users access your private content by using special CloudFront signed URLs or signed cookies\. 
+ Require that your users access your content by using CloudFront URLs, not URLs that access content directly on the origin server \(for example, Amazon S3 or a private HTTP server\)\. Requiring CloudFront URLs isn't necessary, but we recommend it to prevent users from bypassing the restrictions that you specify in signed URLs or signed cookies\.

**Topics**
+ [Overview of Serving Private Content](private-content-overview.md)
+ [Task List: Serving Private Content](private-content-task-list.md)
+ [Specifying the AWS Accounts That Can Create Signed URLs and Signed Cookies \(Trusted Signers\)](private-content-trusted-signers.md)
+ [Choosing Between Signed URLs and Signed Cookies](private-content-choosing-signed-urls-cookies.md)
+ [Using Signed URLs](private-content-signed-urls.md)
+ [Using Signed Cookies](private-content-signed-cookies.md)
+ [Using a Linux Command and OpenSSL for Base64\-Encoding and Encryption](private-content-linux-openssl.md)
+ [Code Examples for Creating a Signature for a Signed URL](PrivateCFSignatureCodeAndExamples.md)